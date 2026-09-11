
prompt:

```Here is the complete architectural plan and executable code for building an autonomous agent skill that converts your 2D emblem into a physical print on your Bambu P1P.

---

### 1. Skill Architecture Overview

```
[User triggers agent with 2D PNG]
                 │
                 ▼
  [Step 1: FAL.ai Trellis API] ────► Downloads raw 3D GLB mesh
                 │
                 ▼
  [Step 2: Python Geometry Mod] ───► Scales, cuts flat base, unions ring, punches hole
                 │
                 ▼
  [Step 3: Slicer CLI] ────────────► Headless OrcaSlicer/Bambu Studio creates .gcode.3mf
                 │
                 ▼
  [Step 4: P1P Dispatcher] ────────► Uploads via FTPS & triggers print over local MQTT

```

---

### 2. Required Packages & System Dependencies

#### OS / System Binaries

1. **OrcaSlicer or Bambu Studio CLI:** Must be installed in your environment path. (OrcaSlicer has the most reliable Linux/headless CLI support without requiring a virtual display buffer `xvfb`).
2. **OpenSSL:** Required for local FTPS/TLS communication.

#### Python Dependencies (`requirements.txt`)

```text
fal-client>=0.4.0
trimesh[easy]>=4.0.0
manifold3d>=2.4.0      # High-performance, watertight boolean CSG engine
numpy>=1.24.0
paho-mqtt>=2.0.0       # Local MQTT broker communication with Bambu
requests>=2.31.0

```

---

### 3. Agent Tool Implementation (Python)

Save this module as `keychain_agent_skill.py`. It provides four distinct steps that your agent orchestrator can call sequentially.

```python
import os
import ssl
import json
import time
import ftplib
import subprocess
import fal_client
import trimesh
import numpy as np
import paho.mqtt.client as mqtt

# ---------------------------------------------------------------------------
# CONFIGURATION
# ---------------------------------------------------------------------------
PRINTER_IP = os.getenv("BAMBU_PRINTER_IP", "192.168.1.50")
PRINTER_ACCESS_CODE = os.getenv("BAMBU_ACCESS_CODE", "12345678") # Screen -> Network -> Access Code
PRINTER_SERIAL = os.getenv("BAMBU_SERIAL", "01P00A123456789")    # Screen -> Device Info
FAL_KEY = os.getenv("FAL_KEY")

# ---------------------------------------------------------------------------
# STEP 1: Generate Mesh with FAL.ai
# ---------------------------------------------------------------------------
def generate_3d_mesh(image_url: str, output_glb_path: str = "raw_emblem.glb") -> str:
    print(f"[*] Calling FAL.ai Trellis for {image_url}...")
    result = fal_client.subscribe(
        "fal-ai/trellis",
        input={
            "image_url": image_url,
            "ss_sampling_steps": 30,
            "slat_sampling_steps": 30,
        }
    )
    model_url = result["model_mesh"]["url"]
    
    import requests
    response = requests.get(model_url)
    with open(output_glb_path, "wb") as f:
        f.write(response.content)
    print(f"[+] Downloaded raw 3D mesh: {output_glb_path}")
    return output_glb_path

# ---------------------------------------------------------------------------
# STEP 2: Add Keychain Ring & Flatten Base (Deterministic Geometry)
# ---------------------------------------------------------------------------
def postprocess_keychain(
    input_mesh: str, 
    output_stl: str = "keychain.stl", 
    target_width_mm: float = 45.0, 
    base_thickness_mm: float = 4.0
) -> str:
    print("[*] Processing mesh geometry...")
    mesh = trimesh.load(input_mesh, force='mesh')

    # Normalize scale
    extents = mesh.extents
    scale_factor = target_width_mm / max(extents[0], extents[1])
    mesh.apply_scale(scale_factor)

    # Re-align: Center XY, place bottom of mesh at Z=0
    bounds = mesh.bounds
    mesh.apply_translation([
        -(bounds[0][0] + bounds[1][0]) / 2.0,
        -(bounds[0][1] + bounds[1][1]) / 2.0,
        -bounds[0][2]
    ])

    # Keychain Loop Dimensions
    top_y = mesh.bounds[1][1]
    ring_outer_radius = 4.5  # 9 mm outer diameter
    ring_inner_radius = 2.2  # 4.4 mm inner hole for jump ring

    # Create solid outer cylinder
    outer_cyl = trimesh.creation.cylinder(radius=ring_outer_radius, height=base_thickness_mm, sections=36)
    outer_cyl.apply_translation([0, top_y + 2.0, base_thickness_mm / 2.0])

    # Create cutting cylinder (taller to guarantee clean through-cut)
    inner_cyl = trimesh.creation.cylinder(radius=ring_inner_radius, height=base_thickness_mm + 4.0, sections=36)
    inner_cyl.apply_translation([0, top_y + 2.0, base_thickness_mm / 2.0])

    # Watertight Manifold CSG Booleans
    combined = trimesh.boolean.union([mesh, outer_cyl], engine='manifold')
    final_mesh = trimesh.boolean.difference([combined, inner_cyl], engine='manifold')

    final_mesh.export(output_stl)
    print(f"[+] Cleaned printable STL exported: {output_stl}")
    return output_stl

# ---------------------------------------------------------------------------
# STEP 3: Slicer CLI (Generate .gcode.3mf)
# ---------------------------------------------------------------------------
def slice_stl_cli(stl_path: str, output_3mf: str = "print_job.gcode.3mf") -> str:
    print("[*] Slicing STL via CLI...")
    # OrcaSlicer CLI standard flags
    cmd = [
        "orca-slicer",
        "--slice", "0",
        "--export-3mf", output_3mf,
        stl_path
    ]
    
    result = subprocess.run(cmd, stdout=subprocess.PIPE, stderr=subprocess.PIPE, text=True)
    if result.returncode != 0:
        raise RuntimeError(f"Slicing failed: {result.stderr}")
        
    print(f"[+] Slicing complete: {output_3mf}")
    return output_3mf

# ---------------------------------------------------------------------------
# STEP 4: The Last Step — Upload to P1P via FTPS and Trigger Print via MQTT
# ---------------------------------------------------------------------------
def print_on_bambu_p1p(file_3mf_path: str, filename_on_sd: str = "keychain_job.gcode.3mf"):
    """
    1. Uploads the .gcode.3mf file directly to the P1P's internal MicroSD root over TLS-FTP (FTPS).
    2. Issues the start print command to the printer's local MQTT broker.
    """
    print(f"[*] Uploading {file_3mf_path} to P1P over FTPS...")
    
    # 1. FTPS Upload
    ftps = ftplib.FTP_TLS()
    ftps.connect(host=PRINTER_IP, port=990)
    ftps.login(user="bblp", passwd=PRINTER_ACCESS_CODE)
    ftps.prot_p()  # Set up secure data connection
    
    with open(file_3mf_path, "rb") as f:
        ftps.storbinary(f"STOR /{filename_on_sd}", f)
    ftps.quit()
    print("[+] File upload complete.")

    # 2. MQTT Start Print Command
    print("[*] Sending print dispatch command via MQTT...")
    client = mqtt.Client(mqtt.CallbackAPIVersion.VERSION2, protocol=mqtt.MQTTv311)
    client.username_pw_set("bblp", PRINTER_ACCESS_CODE)
    
    # Bambu requires TLS without strict cert verification (self-signed)
    client.tls_set(cert_reqs=ssl.CERT_NONE)
    client.tls_insecure_set(True)
    
    client.connect(PRINTER_IP, port=8883, keepalive=60)
    client.loop_start()

    # Wait briefly for broker handshake
    time.sleep(1)

    # Standard Bambu payload to execute a print job from local storage
    payload = {
        "print": {
            "sequence_id": "0",
            "command": "project_file",
            "param": "Metadata/plate_1.gcode",
            "subtask_name": "AI Keychain",
            "url": f"file:///sdcard/{filename_on_sd}",
            "bed_type": "textured_plate",
            "timelapse": False,
            "bed_levelling": True,
            "flow_cali": False,
            "vibration_cali": False,
            "use_ams": False
        }
    }

    topic = f"device/{PRINTER_SERIAL}/request"
    client.publish(topic, json.dumps(payload))
    print(f"[+] Dispatched print command to {topic}!")
    
    time.sleep(2)
    client.loop_stop()
    client.disconnect()

```

---

### 4. How the "Last Step" Works

The Bambu P1P runs two local services on your Wi-Fi network:

1. **Implicit FTPS (Port 990):** Allows writing directly to the printer's MicroSD card.
2. **Secure MQTT Broker (Port 8883):** Accepts JSON commands over TLS for machine movement, heating, and executing jobs.

When your agent reaches the final step:

1. It connects to port `990` via Python's standard `ftplib.FTP_TLS`, logs in with username `bblp` and your printer's **Access Code**, and transfers the sliced `.gcode.3mf` directly onto the SD card.
2. It immediately connects to port `8883` over TLS using `paho-mqtt`.
3. It posts the `project_file` command pointing to `file:///sdcard/keychain_job.gcode.3mf`.
4. The P1P responds locally, turns on its nozzle and bed heaters, runs auto-bed leveling, and begins printing without touching the cloud or asking for manual confirmation.
```
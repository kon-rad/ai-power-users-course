# Module 1 — Livestream Intro / Loading Screen Prompt

Generation prompts for the stream's opening. Two use cases:

1. **Intro sting** — a ~5-second animated open that plays once when the stream
   starts.
2. **"Starting soon" holding screen** — a calm ~60-second loop shown before the
   session begins and during breaks.

Both share the same brand, text, and look. Paste the prompt into your video/image
tool (Veo, Runway, Kling, Luma, or Nano Banana / Imagen for a still), then drop the
result into OBS as a scene.

> **Rendered still:** a generated version of the title card (Prompt C, via Gemini)
> is saved at [`assets/intro-screen.jpg`](./assets/intro-screen.jpg) — 16:9, ready
> to drop straight into OBS.

**Format:** 1920×1080 (16:9), YouTube-safe margins. Keep all text inside the
central 80% of the frame.

---

## Exact on-screen text (use verbatim)

Copy these strings exactly — spelling and capitalisation matter.

- **Brand (top / primary):** `AI POWER USERS`
- **Sub-brand (small, above or below):** `Global Builders Club`
- **Session label:** `Day 1 · Module 1 of 15`
- **Module title (hero line):** `Build Your Private AI Second Brain`
- **Holding-screen line (intro only):** `Starting soon`
- **Website (footer):** `globalbuildersclub.com`
- **The four values (row or stack):**
  - `Participate`
  - `Create more than you consume`
  - `The Four Agreements`
  - `Win and help win`

---

## Brand & style

- **Mood:** calm, premium, cosmic-futurist. Confident, not busy.
- **Background:** near-black (`#0A0A0F`) with a soft violet-to-cyan aurora gradient
  drifting slowly; faint film grain; a subtle particle/starfield.
- **Accent colour:** electric violet/purple (`#A855F7`) with cyan (`#22D3EE`)
  secondary — the same palette as the course website.
- **Type:** a clean geometric sans (Space Grotesk / Inter feel) for headings; a
  mono (JetBrains Mono feel) for the session label and website line.
- **Motion:** slow and smooth — gentle parallax, soft glow pulses, easing in/out.
  Nothing flashing or jittery (this plays for a while on stream).
- **Layout:** brand locked to the top third, module title as the centered hero,
  the four values as a quiet row along the lower third, website pinned to the
  bottom. Generous negative space.

---

## Prompt A — 5-second animated intro sting

```
A cinematic 5-second title intro for a live coding class, 1920x1080, 16:9.

Scene: a near-black deep-space background (#0A0A0F) with a slow-drifting violet-
to-cyan aurora gradient, faint film grain, and a subtle particle starfield.

Animation timeline:
- 0.0-1.0s: particles converge and a soft violet glow blooms in the center.
- 1.0-2.5s: the wordmark "AI POWER USERS" fades and scales up in a clean geometric
  sans-serif, glowing electric violet (#A855F7); small "Global Builders Club"
  label fades in just above it in light gray.
- 2.5-4.0s: a thin luminous line sweeps left-to-right; beneath it the hero title
  "Build Your Private AI Second Brain" types/fades in white; a small monospace tag
  "Day 1 · Module 1 of 15" sits above it in cyan.
- 4.0-5.0s: along the lower third, four short pill labels fade in one by one:
  "Participate", "Create more than you consume", "The Four Agreements",
  "Win and help win"; the footer "globalbuildersclub.com" fades in at the bottom
  in monospace.

Style: premium, calm, cosmic-futurist, smooth easing, soft glow, high contrast,
no flicker. Palette: violet #A855F7 and cyan #22D3EE on near-black. Keep all text
crisp, legible, and inside safe margins.
```

## Prompt B — ~60-second "Starting soon" holding loop

```
A calm, seamlessly looping "starting soon" holding screen for a YouTube live
class, 1920x1080, 16:9, ~60 seconds, designed to loop with no visible seam.

Static layout (text stays put; only the background moves):
- Top third: wordmark "AI POWER USERS" in a glowing geometric sans (electric
  violet #A855F7), with "Global Builders Club" in small light-gray letters above
  it, and a monospace "Day 1 · Module 1 of 15" tag below.
- Center: hero title "Build Your Private AI Second Brain" in clean white type,
  with a smaller cyan line "Starting soon" beneath it.
- Lower third: four quiet pill labels in a single row — "Participate",
  "Create more than you consume", "The Four Agreements", "Win and help win".
- Bottom: "globalbuildersclub.com" in monospace, centered.

Motion: only the background animates — a slow violet-to-cyan aurora drifting,
faint particles rising, a gentle glow that breathes in and out. Text is perfectly
still and legible. Film grain overlay. Nothing flashing.

Style: premium, meditative, cosmic-futurist. Palette: violet #A855F7 and cyan
#22D3EE on near-black #0A0A0F. Loop seamlessly.
```

## Prompt C — static loading card (image, fallback)

Use this with a still-image generator (Nano Banana / Imagen / Flux) if you want a
single PNG to sit under a separate music bed in OBS.

```
A premium 1920x1080 title card, 16:9, for a live AI class. Near-black cosmic
background (#0A0A0F) with a soft violet-to-cyan aurora gradient, faint starfield,
and film grain.

Text layout, all crisp and legible inside safe margins:
- Small top label: "Global Builders Club" (light gray)
- Large glowing wordmark: "AI POWER USERS" (electric violet #A855F7, geometric sans)
- Monospace tag: "Day 1 · Module 1 of 15" (cyan)
- Centered hero title: "Build Your Private AI Second Brain" (white)
- A row of four pill labels near the bottom: "Participate",
  "Create more than you consume", "The Four Agreements", "Win and help win"
- Footer, centered, monospace: "globalbuildersclub.com"

Style: calm, cosmic-futurist, high contrast, soft glow, generous negative space.
Palette: violet #A855F7 and cyan #22D3EE on near-black.
```

---

## Reuse for future modules

Swap only these two strings for any other module and keep everything else:

- **Session label** → e.g. `Day 2 · Module 4 of 15`
- **Module title** → e.g. `How Models Actually Work`

The brand, values, palette, and website line stay the same across all 15 modules.

## Production notes

- Render at 1080p (or 4K down to 1080p) and 30fps. Add your music bed in OBS, not
  in the generated clip, so you can reuse the same audio across modules.
- If a tool mangles the longer value line, shorten `Create more than you consume`
  to `Create` on screen and keep the full phrase in narration.
- Check legibility on a phone — most live viewers watch small.

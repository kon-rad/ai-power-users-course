# Module 3.2: Student Guide

**Local Models and Private Inference**
Follow along here. Every command, every config edit, every script.

By the end you will have a local Ollama model Hermes actually uses for tool calls, a
`model_aliases` entry and `/model local` to reach it, a `secondbrain` Hermes profile with
its own launcher script, one encrypted folder, and Venice AI wired in as `private`.

**This module targets a MacBook Pro M4, 16GB unified memory.** More RAM gives more room,
not less need for the context fix below.

---

## Before the session

### You need `/switch-models` from Module 2, and Module 3 complete

```bash
ls ~/.claude/skills/switch-models 2>/dev/null || echo "missing: build it in Module 2 first"
```

### Install Ollama and pull a model before today

```bash
curl -fsSL https://ollama.com/install.sh | sh
ollama --version
ollama pull qwen2.5:7b
```

A 4.7GB download during a live session wastes everyone's time. Do this the night before.

### Create a Venice AI account

Sign up at [venice.ai](https://venice.ai). 0 USD to create the account. Bring a payment
method if you want paid credit live, or use the free tier for today's test calls.

### Bring

- **The machine you tried Qwen on before**, in whatever state it was left. Do not fix it
  ahead of time. Diagnosing the actual failure is the first fifteen minutes.

---

## Step 1: Why it failed

Two documented causes, not a guess.

**Ollama's default context is too small.** Under 24GB of VRAM, Ollama serves 4,096 tokens
by default. Hermes needs at least 64,000 to run agentically. This is not adjustable through
the API Hermes talks to, only server side or through a Modelfile.

**The model's own training context sits below Hermes' floor anyway.** `qwen2.5:7b` and
`qwen2.5:14b` train to 32K tokens. `qwen3:8b` and `qwen3:14b` train to 40K. Both are below
Hermes' 64K minimum. Raising Ollama's served context past that number does not give the
model more real context, it asks the model to extrapolate past its training. For a normal
agentic session this rarely matters. On a very long one, watch for the model losing the
thread.

> **The trap.** If Ollama was left on its default when Qwen was tried, both causes were
> stacking: a 4,096 token ceiling from Ollama, on top of a model that maxes out at 32K to
> 40K even when raised. Neither alone fully explains what happened. Together, they do.

---

## Step 2: Pull and size the model correctly

```bash
ollama list
```

| Model | Download | Native context | Fits 16GB comfortably |
|---|---|---|---|
| `qwen2.5:7b` | 4.7 GB | 32K | Yes |
| `qwen2.5:14b` | 9.0 GB | 32K | Tight, close other apps |
| `qwen3:8b` | 5.2 GB | 40K, thinking and non thinking modes | Yes |
| `qwen3:14b` | 9.3 GB | 40K | Tight |

Start Ollama with context raised past Hermes' floor:

```bash
OLLAMA_CONTEXT_LENGTH=64000 ollama serve
```

Run this in its own terminal tab. It occupies the terminal.

Or bake it into a named model, persistent across restarts:

```bash
cat > /tmp/Modelfile << 'EOF'
FROM qwen2.5:7b
PARAMETER num_ctx 64000
EOF
ollama create qwen2.5-64k -f /tmp/Modelfile
```

Confirm it took:

```bash
ollama ps
```

The `CONTEXT` column should read `64000`.

---

## Step 3: Connect Hermes to it

```bash
hermes model
```

Pick **Custom endpoint**. Base URL `http://localhost:11434/v1`, no API key, model name
`qwen2.5:7b` or `qwen2.5-64k`.

Set the context floor on Hermes' own side too:

```bash
hermes config set model.context_length 64000
```

Test with a real tool call:

```
List every markdown file in this vault modified in the last day, and tell me which one is
longest.
```

> **The trap.** If Hermes still refuses at startup with a context error, check `ollama ps`
> first. A restarted `ollama serve` without the env var silently drops back to the default.

---

## Step 4: `model_aliases`, the routing mechanism

Open `~/.hermes/config.yaml`:

```yaml
model_aliases:
  local:
    model: qwen2.5:7b
    provider: custom
    base_url: http://localhost:11434/v1
```

```
/model local
```

This is the exact mechanism `/switch-models` has been calling since Module 2. It just had
nothing configured for `local` or `private` until today.

---

## Step 5: A profile scoped to this vault

```bash
hermes profile create secondbrain --clone
hermes profile show secondbrain
```

`--clone` copies the active profile's `config.yaml`, `.env`, `SOUL.md`, and skills into
`~/.hermes/profiles/secondbrain/`. Edits there never touch the profile you started from.

> **The trap.** A profile is a full second identity, its own sessions and skill list, not a
> lightweight override. Use one when the config genuinely needs to diverge, not for every
> small tweak.

---

## Step 6: The launcher script

Look at what Hermes already generates:

```bash
hermes profile alias secondbrain
cat ~/.local/bin/secondbrain
```

Write a fuller version that also lands you in the vault:

```bash
#!/bin/sh
cd ~/Documents/secondbrain || exit 1
exec hermes -p secondbrain "$@"
```

```bash
cat > ~/.local/bin/sb << 'EOF'
#!/bin/sh
cd ~/Documents/secondbrain || exit 1
exec hermes -p secondbrain "$@"
EOF
chmod +x ~/.local/bin/sb
```

Run `sb` from anywhere. It should open Hermes already inside this vault, on the
`secondbrain` profile.

---

## Step 7: Encrypt a folder

```bash
hdiutil create -size 2g -type SPARSEBUNDLE -fs APFS -encryption AES-256 -agentpass \
  -volname "Private" ~/private-vault.sparsebundle
```

`-agentpass` prompts through the macOS Keychain agent for a password, on screen, rather
than on the command line where it would land in shell history.

```bash
hdiutil attach ~/private-vault.sparsebundle
```

Mounts at `/Volumes/Private`, password required. Put one real file there.

```bash
hdiutil detach /Volumes/Private
```

> **The trap.** `-size 2g` sets a maximum, not a reservation. A sparsebundle only consumes
> the disk space actually used.

---

## Step 8: What Venice AI actually is

Every model on Venice carries a `model_spec.privacy` field, not a marketing label:

| Label | What it means | Example |
|---|---|---|
| **private** | Zero data retention, Venice's own self hosted open weight models. The request never reaches the original vendor | `deepseek-v4-pro-0813` |
| **anonymized** | Identifying metadata stripped before forwarding to the underlying provider (Anthropic, OpenAI, Google, xAI). That provider still processes the request. Venice states it cannot guarantee full privacy here | `claude-sonnet-5` |

Anonymized removes the account link. It does not remove the content from what the model
vendor sees.

---

## Step 9: Create a key and wire Venice into Hermes

Generate a key at `venice.ai/settings/api`.

```bash
echo 'VENICE_API_KEY=paste-your-key-here' >> ~/.hermes/profiles/secondbrain/.env
```

```yaml
# ~/.hermes/profiles/secondbrain/config.yaml
model_aliases:
  private:
    model: deepseek-v4-pro-0813
    provider: custom
    base_url: https://api.venice.ai/api/v1
    api_key: paste-your-venice-key-here
```

```
/model private
```

> **The trap.** `config.yaml` is `chmod 600`, readable only by this account, which is why
> the key can sit here directly. Never commit this file, and never paste its contents
> anywhere public.

---

## Step 10: Compare private against anonymized

Send the same prompt through both aliases. Read `GET /models?type=text` from the Venice API
and check `model_spec.privacy` on each model, rather than trusting the name alone.

---

## If something breaks

| Symptom | Cause and fix |
|---|---|
| Hermes refuses to start against the local model, a context error | `ollama ps` and check the CONTEXT column. A restarted `ollama serve` without `OLLAMA_CONTEXT_LENGTH` drops back to the default |
| Tool calls come back as plain text JSON instead of executing | Confirm the model actually supports tool calling, `ollama show <model-name>`. Both Qwen2.5 and Qwen3 do, at the sizes in the table above |
| `/model local` fails with connection refused | `ollama serve` is not running, or was started in a terminal that got closed |
| `hdiutil attach` asks for a password every time and never remembers it | Expected. `-agentpass` at creation asks once; the volume itself always requires the password to mount, that is the point |
| Venice requests return 401 | Check the key was pasted into the `secondbrain` profile's `.env` or `config.yaml`, not the main profile's |
| `sb` opens Hermes but not in the vault | Re-check the script's `cd` path matches where this vault actually lives on this machine |

---

## Homework

1. Pull `qwen2.5:14b`, compare against `qwen2.5:7b` on the same real task. Report which one
   earned the extra RAM.
2. Add `fast` and `smart` aliases to `model_aliases` if they are not already there.
3. Restart the machine, confirm the encrypted volume is unreadable without mounting again.
4. Run one prompt through Venice's private tier and its anonymized tier. Compare, and say
   which one you would trust with client data.

---

## What this cost

| Item | Amount |
|---|---|
| Ollama, the local model, the encrypted volume | 0 USD |
| Venice AI account | 0 USD to create |
| Venice AI usage during the session | A few cents at most, free tier or small paid credit covers it |

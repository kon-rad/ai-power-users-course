# Module 3.2: Local Models and Private Inference

**Status:** syllabus draft v1, planning phase.
**Date:** 2026-09-02.
**Platform:** macOS, Apple Silicon. Local model sizing in this module targets 16GB unified
memory specifically (a MacBook Pro M4). Bigger machines have more room, not less need for
the fix.
**Prereq:** Module 2's `/switch-models` skill exists. Module 3 complete.
**Companions:** `student-guide.md` · `agenda.md` · `quiz.md` · `luma-description.md`

---

## Format

**One session, 60 minutes, no break, live on YouTube.**

| Part | Time | Theme |
|---|---|---|
| **Part 1** | 0:02-0:20 | Diagnose and fix the local model that already failed once |
| **Part 2** | 0:20-0:33 | Route to it on purpose: aliases, a profile, a launcher script |
| **Part 3** | 0:33-0:41 | Encrypt a folder nothing else can read |
| **Part 4** | 0:41-0:57 | Venice AI: private versus anonymized, wired in as a provider |

**The spine:** `/switch-models` has had an empty `private` slot since Module 2. Today it
gets filled, twice: once with a model that runs with no network at all, once with a
provider that strips your identity before a frontier model sees the request.

---

## Learning objectives

By the end, a student can:

1. Name the two documented reasons a 7B or 14B Ollama model failed Hermes' tool calling
2. Pull and configure an Ollama model whose context clears Hermes' 64,000 token floor
3. Define `model_aliases` in `config.yaml` and switch with `/model <alias>`
4. Create a Hermes profile scoped to this vault, with its own `config.yaml` and `.env`
5. Write a bash launcher that always opens Hermes into that profile
6. Create a password protected encrypted volume with `hdiutil`
7. State the real difference between a Venice AI private model and an anonymized one

---

## 0:00-0:02 · Open (2 min)

State the artifact: a local model Hermes will actually use for tool calls, a routing
alias to reach it on purpose, an encrypted folder, and Venice AI wired in as a second
private option.

---

# PART 1: Diagnose and fix the local model (18 min)

## 0:02-0:07 · Why it failed (5 min)

Two separate, documented causes, not a guess.

**Cause 1: Ollama's default context is too small.** Under 24GB of VRAM, Ollama serves a
default context of 4,096 tokens. Hermes needs at least 64,000 tokens of context to run
agentically: the system prompt, every tool schema, and the working conversation all have
to fit. This ceiling is not adjustable through the API Hermes talks to. It is set when
Ollama's server starts, or baked into a model with a Modelfile.

**Cause 2: the model's own training context is below Hermes' floor.** `qwen2.5:7b` and
`qwen2.5:14b` are both trained to a 32K token context. `qwen3:8b` and `qwen3:14b` train to
40K. Raising Ollama's served context past either number does not give the model more real
context, it asks the model to extrapolate past what it was trained on. Hermes' 64K floor
sits above both.

Read this against what was actually tried before today. If Ollama was left on its default
and the model was `qwen2.5:7b` or `:14b`, both causes were in play at once.

## 0:07-0:15 · Pull and size the model correctly (8 min)

```bash
ollama --version
curl http://localhost:11434/api/tags
```

If `qwen2.5:7b` was pulled before the session, confirm it:

```bash
ollama list
```

| Model | Download | Native context | Fits 16GB comfortably |
|---|---|---|---|
| `qwen2.5:7b` | 4.7 GB | 32K | Yes |
| `qwen2.5:14b` | 9.0 GB | 32K | Tight, close other apps |
| `qwen3:8b` | 5.2 GB | 40K, thinking and non thinking modes | Yes |
| `qwen3:14b` | 9.3 GB | 40K | Tight |

Start Ollama with its context raised past Hermes' floor, on purpose, and say plainly what
that trade off is: past 32K to 40K of real conversation, the model is extrapolating beyond
its training, not reading a context it actually learned on. For a normal agentic session,
that ceiling is rarely reached. Watch for it on long ones.

```bash
OLLAMA_CONTEXT_LENGTH=64000 ollama serve
```

Or bake it into the model, persistent across restarts:

```bash
cat > /tmp/Modelfile << 'EOF'
FROM qwen2.5:7b
PARAMETER num_ctx 64000
EOF
ollama create qwen2.5-64k -f /tmp/Modelfile
```

Verify it took:

```bash
ollama ps
```

The `CONTEXT` column should read 64000, not 4096 or 2048.

### Watch for

`ollama serve` occupies the terminal. Run it in its own tab, or as a background service,
not the same terminal `hermes` runs in.

## 0:15-0:20 · Connect Hermes to it (5 min)

```bash
hermes model
```

Pick **Custom endpoint**. Base URL `http://localhost:11434/v1`, no API key, model name
`qwen2.5:7b` or `qwen2.5-64k` if the Modelfile route was used.

Set the context length explicitly on the Hermes side too, as a floor Hermes itself will
enforce rather than trusting whatever the endpoint reports:

```bash
hermes config set model.context_length 64000
```

Test it with a real tool call, not a greeting:

```
List every markdown file in this vault modified in the last day, and tell me which one is
longest.
```

### Watch for

If Hermes still refuses at startup with a context error, `ollama ps` first. A restarted
`ollama serve` without the env var loses the override.

---

# PART 2: Route to it on purpose (13 min)

## 0:20-0:25 · `model_aliases` (5 min)

Open `~/.hermes/config.yaml`.

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

That is the whole mechanism `/switch-models` has been calling since Module 2. Today it has
a real destination for the first time.

## 0:25-0:29 · A profile scoped to this vault (4 min)

```bash
hermes profile create secondbrain --clone
```

`--clone` copies the active profile's `config.yaml`, `.env`, `SOUL.md`, and skills into
`~/.hermes/profiles/secondbrain/`. From here, edits to that profile's `config.yaml` never
touch the main one.

```bash
hermes profile show secondbrain
```

### Watch for

A profile is a full second identity: its own sessions, its own skill list, its own state.
It is not a lightweight override. Use one when the config genuinely needs to diverge, not
for every small tweak.

## 0:29-0:33 · The launcher script (4 min)

Hermes already generates one. Look at it before writing your own:

```bash
hermes profile alias secondbrain
cat ~/.local/bin/secondbrain
```

Write a second version that also lands you in the right directory:

```bash
#!/bin/sh
cd ~/Documents/secondbrain || exit 1
exec hermes -p secondbrain "$@"
```

Save it as `~/.local/bin/sb`, `chmod +x`, and confirm `sb` on its own opens Hermes already
in this vault, on the `secondbrain` profile.

---

# PART 3: Encrypt a folder (8 min)

## 0:33-0:41 · A password protected volume, native, no third party tool (8 min)

```bash
hdiutil create -size 2g -type SPARSEBUNDLE -fs APFS -encryption AES-256 -agentpass \
  -volname "Private" ~/private-vault.sparsebundle
```

`-agentpass` prompts through the Keychain agent for a password, visible on screen, rather
than typing it on the command line where it would land in shell history.

```bash
hdiutil attach ~/private-vault.sparsebundle
```

It mounts at `/Volumes/Private` and prompts for the password. Put one real file in it.

```bash
hdiutil detach /Volumes/Private
```

The sparsebundle on disk is unreadable without the password. `ls ~/Documents/` for
comparison: this file sits right next to everything else, and reads as noise without the
key.

### Watch for

`-size 2g` sets the maximum. A sparsebundle only consumes the disk space actually used, not
the full 2GB up front.

---

# PART 4: Venice AI, private versus anonymized (16 min)

## 0:41-0:45 · What Venice actually is (4 min)

Venice AI is a unified API over many third party and self hosted models. Every model
carries one of two privacy labels, not a marketing claim, a field on the model object
itself:

| Label | What it means | Example |
|---|---|---|
| **private** | Zero data retention, runs on Venice's own self hosted open weight models. The request never reaches the original model vendor | `deepseek-v4-pro-0813` |
| **anonymized** | Identifying metadata is stripped before the request is forwarded to the underlying provider (Anthropic, OpenAI, Google, xAI), but that provider still processes it. Venice states it cannot guarantee full privacy on this tier | `claude-sonnet-5` |

Anonymized is not private. It removes the account link, not the content, from what the
model vendor sees.

## 0:45-0:48 · Create an account and an API key (3 min)

Sign up at [venice.ai](https://venice.ai). Free to create an account. Generate a key at
`venice.ai/settings/api`.

```bash
echo 'VENICE_API_KEY=paste-your-key-here' >> ~/.hermes/profiles/secondbrain/.env
```

### Watch for

Confirm this landed in the `secondbrain` profile's `.env`, not the main one, if the goal is
keeping this key scoped to vault work only.

## 0:48-0:53 · Wire Venice into Hermes (5 min)

Base URL: `https://api.venice.ai/api/v1`. Bearer auth.

```yaml
model_aliases:
  private:
    model: deepseek-v4-pro-0813
    provider: custom
    base_url: https://api.venice.ai/api/v1
    api_key: paste-your-venice-key-here
```

`config.yaml` is `chmod 600`, readable only by this account. That is why the key can live
here directly rather than as an env reference, unlike the vault's own `.env` convention for
project secrets.

```
/model private
```

`/switch-models` can now be told, honestly, what `private` means: a self hosted model with
Venice's zero retention guarantee, not a description that was never backed by anything.

## 0:53-0:57 · Compare private against anonymized, live (4 min)

Send the same prompt through both tiers. Read the two responses side by side, and read the
`model_spec.privacy` field back from `GET /models?type=text` to confirm which is which,
rather than trusting the model name alone.

### Watch for

Anonymized models are frontier quality. Private models are Venice's own open weight
catalogue, closer to what Ollama runs locally than to Claude or GPT. Pick the tier the task
actually needs, not the one that sounds safer.

---

## 0:57-1:00 · Review and close (3 min)

State what is now permanent: `/switch-models private` reaches a real destination,
`/switch-models local` reaches Ollama, a `secondbrain` profile and its launcher exist, and
one folder on this machine is unreadable without a password.

State the honest caveat: local and private are not free of trade offs. Local is slower and
less capable than a frontier model. Anonymized is not the same guarantee as private, no
matter how the tier is named elsewhere.

---

# Homework

1. Pull `qwen2.5:14b` and compare it against `qwen2.5:7b` on the same real task. Report
   which one earned the extra RAM.
2. Add `fast` and `smart` aliases if they are not already in `model_aliases`.
3. Put one real file in the encrypted volume, restart the machine, confirm it is unreadable
   without mounting.
4. Run one prompt through Venice's private tier and its anonymized tier. Compare and say
   which one gets client data.

# Peer quiz

Summary only, detail in `quiz.md`: the two causes of the original Qwen failure · what
`-agentpass` does differently from typing a password on the command line · the difference
between `model_aliases` and a Hermes profile · what `model_spec.privacy` actually
guarantees on each Venice tier.

# Prep checklist

## Must verify before the stream

- [ ] `qwen2.5:7b` pulls clean and `ollama ps` shows the raised context after
      `OLLAMA_CONTEXT_LENGTH=64000 ollama serve`, timed on an actual M4 16GB machine
- [ ] `hermes profile create --clone` and `hermes profile alias` still produce the exact
      wrapper script shown here. Confirmed against `coding-best` on 2026-09-02, re-check
      against the Hermes version live on stream day
- [ ] Venice AI's account signup flow, current as of the stream date. Verified 2026-09-02
      against docs.venice.ai
- [ ] The exact current price of `deepseek-v4-pro-0813` and `claude-sonnet-5` on Venice.
      $1.65 / $4.95 and $3.00 / $15.00 per 1M tokens as of 2026-09-02, re-verify before
      quoting a number on stream

## Assets

- [ ] A machine already showing the original Qwen failure, preserved, not fixed in advance
- [ ] A Venice account with a small amount of credit pre loaded, in case live signup stalls
- [ ] A backup `qwen2.5:7b` pull cached locally, in case live download is slow on the day

# Open questions

1. **Is 16GB genuinely enough to keep `qwen2.5:14b` comfortable during a live session with
   Hermes, a browser, and OBS all running at once?** Recommend testing the full real load,
   not just Ollama alone, before promising 14B works live.
2. **Should this module also cover Ollama Cloud as a third `private`-adjacent option?** It
   removes the GPU requirement but the model runs on Ollama's infrastructure, not the
   student's own machine, which changes what "private" means. Recommend a one line mention
   at most, not a full segment, to avoid diluting the local versus Venice distinction.

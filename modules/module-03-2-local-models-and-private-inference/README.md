# Module 3.2: Local Models and Private Inference

**Module 3.2 of AI Power Users · 60 minutes · live on YouTube · run on macOS, Apple Silicon**
**From Argo, myargoquest.com**

Module 2 built `/switch-models`, a skill with four slots: private, fast, smart, coding.
Three of those got wired up. This module gives the fourth one, `private`, somewhere real to
go: a local Ollama model on your own machine, and Venice AI when you want a frontier model
with your identity stripped out of the request.

## What you build

```
PART 1: fix the local model
    why Qwen stalled  ──  Ollama's context default  ──  a model sized for 16GB unified memory

PART 2: route to it on purpose
    model_aliases in config.yaml  ──  a secondbrain profile  ──  a bash launcher

PART 3: a place nothing reads
    hdiutil, AES-256, APFS  ──  mount, work, unmount

PART 4: private without local
    Venice AI  ──  private vs anonymized  ──  wired in as a Hermes custom provider
```

## Learning objectives

By the end of this module you can:

1. Explain why a 7B or 14B Ollama model failed Hermes' agentic tool calling, using the two
   documented causes, not a guess
2. Pull and configure an Ollama model whose context actually clears Hermes' 64,000 token
   floor
3. Define `model_aliases` in `config.yaml` and switch between them with `/model <alias>`
4. Create a Hermes profile scoped to this vault, with its own `config.yaml` and `.env`
5. Write a bash script that always launches Hermes into that profile from this vault
6. Create a password protected encrypted volume with `hdiutil`, no third party tool
7. State the actual difference between a Venice AI private model and an anonymized one, and
   wire Venice in as a custom provider

## The skill this module finishes

| Skill | Cadence | What changes today | Why it matters |
|---|---|---|---|
| **`/switch-models`** | Already built, Module 2 | The `private` slot gets a real destination for the first time: a local Ollama model, or Venice AI, instead of pointing at nothing | A skill with an empty slot is a promise, not a tool. Today the promise gets kept |

## The 4 parts

| # | Part | Output |
|---|---|---|
| 1 | Diagnose and fix the local model | A Qwen model on Ollama that Hermes will actually use for tool calls |
| 2 | Route to it on purpose | `model_aliases`, a `secondbrain` profile, a working launcher script |
| 3 | Encrypt a folder | A mounted, password protected APFS volume |
| 4 | Wire in Venice AI | `private` alias pointed at a zero retention model |

## The ideas that carry the module

**A model's advertised context is a ceiling, not Hermes' floor.** Qwen2.5 7B and 14B train
to 32K tokens. Hermes needs 64K to run agentically. That gap, not model quality, is the
likely cause of the failures already hit.

**Ollama does not give a model its full context by default.** Under 24GB of VRAM, Ollama
serves 4,096 tokens unless told otherwise, and that ceiling cannot be raised through the
API Hermes talks to.

**An alias is a judgment call encoded once.** `/switch-models private` should mean
something specific, not "whatever was configured last." Naming it makes the choice
visible.

**A profile is a second identity, not a second install.** `~/.hermes/profiles/<name>/` gets
its own `config.yaml` and `.env`. One Hermes binary, two separate configurations.

**Anonymized is not private.** Venice strips identifying metadata before forwarding a
request to the underlying provider. The provider still processes it. Only Venice's own
open weight models carry the zero retention guarantee.

## Files in this module

- [`syllabus.md`](./syllabus.md), the full plan: every segment, every command
- [`agenda.md`](./agenda.md), the 60 minute run of show and the contingency
- [`student-guide.md`](./student-guide.md), **follow along here**: every step and command
- [`quiz.md`](./quiz.md), 5 multiple choice plus 4 open ended, peer evaluated
- [`luma-description.md`](./luma-description.md), event copy

## Before the session

**You need Module 2's `/switch-models` skill and Module 3 complete.** If you missed them,
the [Module 2 student guide](../module-02-agent-mastery-and-vibe-coding/student-guide.md)
and
[Module 3 student guide](../module-03-media-models-and-your-own-domain/student-guide.md)
get you current.

**Ollama installed, and one model pulled before the session.** A 4.7 to 9 GB download on
stream wastes the room's time.

```bash
curl -fsSL https://ollama.com/install.sh | sh
ollama pull qwen2.5:7b
```

| Account | Why | Cost |
|---|---|---|
| [Venice AI](https://venice.ai) | Private and anonymized model access | 0 USD to sign up. Bring a payment method if you want paid credit live |

**Bring:** the machine you tried Qwen on before, in the state you left it. The failure is
part of the lesson.

## Homework

1. Pull `qwen2.5:14b` and compare it against `qwen2.5:7b` on the same real task. Report
   which one you kept.
2. Add a `fast` and a `smart` alias to `model_aliases`, pointed at whatever Module 2 chose
   for those slots, if they are not already there.
3. Put one real file inside the encrypted volume, unmount it, restart the machine, and
   confirm you cannot read the file without mounting it again.
4. Run the same prompt through the Venice private model and the Venice anonymized model.
   Compare the two responses and say which one you would trust with client data.

## The honest caveat

A local model on a 16GB Mac will not match a frontier cloud model on judgment, and running
it does not make Hermes free: transcription, image generation, and cloud fallbacks in the
same session still bill normally. What local buys is a floor that works with no network and
no API key, for the tasks that do not need the best model in the world.

# Luma Event: Module 3.2

> House rules: short, concrete, no em dashes, no emojis, links at the end, and the host
> and sponsor footer pasted byte for byte.

## Event title

**The one we are using:**

AI Power Users · Module 3.2: Local Models and Private Inference (Live, Free)

**Two alternates:**

2. AI Power Users · Module 3.2: Make Ollama Actually Work With Hermes (Live, Free)
3. AI Power Users · Module 3.2: Give /switch-models a Private Destination (Live, Free)

## Short blurb (for previews)

Module 2 built `/switch-models` with an empty private slot. This session fills it: a local
Ollama model Hermes actually uses for tool calls, an encrypted folder, and Venice AI wired
in for when private needs to mean something specific. One session, 60 minutes, 0 USD.

## Full description

This is Module 3.2 of AI Power Users, a free hands on live course from Argo. This one
targets a MacBook Pro M4, 16GB unified memory.

Module 2 built `/switch-models` with an empty private slot. This one fills it, twice.

Two halves.

**First, the local model that already failed gets fixed and wired in.** Two documented
causes, not a guess: Ollama's default context, and Qwen's own training ceiling. Then
`model_aliases` and a `secondbrain` profile give `/switch-models local` somewhere real to
go.

**Then private gets a second, honest meaning.**

1. Diagnose the two real reasons Qwen failed before.
2. Pull and configure a model sized for 16GB.
3. Route to it with `model_aliases` and `/model local`.
4. Encrypt a folder with `hdiutil`, no third party tool.
5. Wire Venice AI in as `private`.
6. Compare Venice's private and anonymized tiers, live.

This session costs 0 USD to complete. Ollama, the encrypted volume, and Venice's free tier
cover everything shown.

**By the end you will have:**

- A local Ollama model Hermes actually uses for tool calls
- `model_aliases` and a `secondbrain` Hermes profile
- One password protected encrypted volume
- **`/switch-models private`, pointed at a real, zero retention model**
- A working comparison of Venice's two privacy tiers

**Who it is for:** anyone who built `/switch-models` in Module 2 and never filled its
private slot.

**The honest part:** local and private carry trade offs. A 16GB Mac will not match a
frontier model on judgment, and anonymized is not the same guarantee as private, whatever a
tier is named elsewhere.

**Format:** one live session, 60 minutes, no break. Live on YouTube. macOS, Apple Silicon.

## Links

Course page: https://myargoquest.com/courses/ai-power-users/module-3-2

Setup and download instructions:
https://github.com/kon-rad/ai-power-users-course/blob/main/modules/module-03-2-local-models-and-private-inference/student-guide.md

Everything you need to install beforehand is in the setup instructions. Pull the model the
night before, not during the stream.

> **Both URLs checked 2026-09-02: the course page returns 404 and the GitHub link returns
> 404.** They follow the pattern that resolves for Module 3, so they are the right shape,
> but do not publish this description until each one loads.

## Footer (fixed copy, paste byte for byte)

Paste the host and sponsor block from
`.claude/skills/argo-events/assets/luma-footer.md` verbatim at the end of the description.
It contains zero width characters. Do not retype it. Copy the file.

## Suggested Luma settings

- Cost: Free to attend, 0 USD required
- Capacity: uncapped, livestream
- Location: Online, YouTube Live, link sent to registrants
- Host: Argo (Konrad Gnat)
- Duration: 75 minutes, a 60 minute session plus buffer
- Tags: AI, Agents, Local Models, Privacy, Ollama, Beginner-Friendly

## Notes for the host

- Lead the promo image with a terminal showing `ollama ps` and a real context number, not a
  generic AI graphic. The fix is the deliverable.
- State the 16GB target machine plainly. Someone on 8GB needs a smaller model, someone on
  32GB+ has room to skip straight to `qwen2.5:14b`.
- Do not name a specific Venice model price on stream without re-checking it against
  `docs.venice.ai/overview/pricing` that morning. Prices in this file are dated 2026-09-02.

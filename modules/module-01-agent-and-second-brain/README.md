# Module 1 — Agent & Private AI Second Brain Setup

**Day 1 · Module 1 of 15 · 45 minutes · live on YouTube**

The very first session of the AI Power Users course. We introduce the course,
then install and wire together a complete **private AI second brain**: a set of
free tools that give you a personal, private AI agent living inside your own
notes — with nothing depending on any single company's cloud.

By the end you will have a working setup and your first custom agent skill: a
**daily standup** where your AI reviews your projects, goals, and progress with
you.

## What you'll build

```
          ┌─────────────────────────────────────────────┐
          │   Your Second Brain  (a folder on your disk) │
          │   organised with the PARA method             │
          │                                              │
   speak  │   Obsidian + VS Code ── view/edit notes,      │
  ─────▶  │      ▲                  skills & config        │
  Handy   │      │                                        │
  (STT)   │   Hermes agent  ── runs in the folder,        │
          │      │             reads & writes your notes  │
          │      ▼                                        │
          │   Morpheus  ── the private LLM brain          │
          │   (api.mor.org, OpenAI-compatible)           │
          └─────────────────────────────────────────────┘
              all launched from cmux (your terminal)
```

## The six pieces

| Tool | Role | Free? | Open source? |
|---|---|---|---|
| **Handy** | Local speech-to-text — talk instead of type, 100% offline | Free | Yes |
| **Obsidian** | View, edit, and visualise your notes (Markdown vault) | Free (personal) | No (free) |
| **VS Code** | Inspect and edit the real files behind your agent — notes, skills, and config | Free | Yes (MIT) |
| **Hermes** | The AI agent that lives in your folder and does the work | Free | Yes |
| **Morpheus** | The private, decentralized LLM that powers the agent | Free tier | Yes (network) |
| **cmux** | The terminal you run your agents in (macOS) | Free | Yes |

## Learning objectives

By the end of this module you can:

1. Explain what a "private AI second brain" is and why each of the six pieces exists.
2. Install local speech-to-text, Obsidian, VS Code, Hermes, and cmux.
3. Create a Morpheus account and generate an API key for a private, OpenAI-compatible LLM.
4. Organise a notes folder with the PARA method and open it as an Obsidian vault.
5. Run the Hermes agent inside that folder and connect it to Morpheus.
6. Create and run your first agent skill — a daily standup with your AI.
7. **(Bonus)** Switch Hermes to an open-source model via **OpenRouter** for more speed
   and lower cost — understanding that OpenRouter is **not private**, so it's for when
   speed and cost matter more than privacy (private work stays on Morpheus).

## Files in this module

- [`agenda.md`](./agenda.md) — the 45-minute run-of-show
- [`livestream-script.md`](./livestream-script.md) — timed host script for the stream
- [`livestream-intro-prompt.md`](./livestream-intro-prompt.md) — prompts to generate the intro / "starting soon" screen
- [`livestream-metadata.md`](./livestream-metadata.md) — YouTube live title, description, chapters, tags
- [`luma-description.md`](./luma-description.md) — event copy for Luma
- [`student-guide.md`](./student-guide.md) — **follow along here**: every step, link, and description
- [`quiz.md`](./quiz.md) — 5 multiple-choice + 5 open-ended questions (peer-evaluated)

## Before the session (important)

Some downloads are large. **Start them before class** so you're not waiting:

- Install **Obsidian**, **VS Code**, **Handy**, and (macOS) **cmux**.
- Run the **Hermes** install command.
- Have a laptop (macOS, Windows, or Linux), a microphone, and about 2 GB free.

Full instructions are in [`student-guide.md`](./student-guide.md).

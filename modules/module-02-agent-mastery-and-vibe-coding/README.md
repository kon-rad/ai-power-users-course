# Module 2 — Agent Mastery and Vibe Coding a Pro Website

**Module 2 of AI Power Users · 110 minutes · live on YouTube**
**From Argo — myargoquest.com**

In Module 1 you wrote a skill by hand so you could see the machinery. **You never have to do
that again.**

This session runs on one idea: **you describe, the agent builds.** First your agent writes
three tools for itself. Then it builds a real client's website — from a blank folder to a
live, deployed URL — in one sitting.

## What you'll build

```
PART 1 — Agent mastery
    ┌──────────────────────────────────────────────────────┐
    │  One brief  ──  /models   researches every model     │
    │                            daily, with live pricing   │
    │             ──  /switch   private / fast / smart     │
    │             ──  /spend    what it all cost           │
    │                       │                               │
    │                                                      │
    │              /daily-standup  ← all three, every morning│
    └──────────────────────────────────────────────────────┘

PART 2 — Vibe coding a pro website
    goals ── knowledge base ── layout ── architecture
      │         (info + images)      │           │
      │                                         
      └──────────────────────  plan ── build ── verify
                                              │
                                              
                                    git ── deploy ── LIVE URL
```

## Learning objectives

By the end of this module you can:

1. Explain the difference between a **session** and a **task**, and run **one task per
   session** — resetting between them so model switching never costs you anything
2. Get your agent to **write its own skills** from a spoken brief
3. Explain what a token is, and **measure** what your context files cost per message
4. Tell `SOUL.md` (who your agent is) from `HERMES.md` (how this job works)
5. Turn a business conversation into a structured knowledge base — text **and** images
6. Describe an app's goals, layout, and functionality clearly enough for an agent to build it
7. Judge a tech-stack recommendation without being an engineer — including **saying no to a
   database**
8. Ship a real, live, deployed website with git version control

## The three skills your agent writes

| Skill | Cadence | What it does | Why it matters |
|---|---|---|---|
| **`/models-research`** | **Weekly** + on demand | Researches text, image, video, TTS, speech-to-text and music models. Live pricing from public APIs and independent leaderboards. Writes a dated briefing into your vault | You stop reading "best AI tools" listicles. The data is current, sourced, and yours |
| **`/model-switch`** | On demand | Moves between four profiles: `private` (Morpheus), `fast` (cheap), `smart` (frontier), and `coding` (best **agentic** coding model). **Stops you before sending client data to a public provider** | Automating a command is a shortcut. **Automating a decision is a skill** |
| **`/spend-report`** | **Weekly** + on demand | **Incremental** — keeps a watermark and only ever processes usage since its last run. Reports by project and model, flags spikes, names one change to cut cost | You install the meter before you need it — and it never does the same work twice |

> **Why weekly, not daily.** The model landscape moves in weeks, not hours, and a daily
> research pass burns tokens to tell you the same thing five times. Both skills stay
> available on demand — **cadence is a default, not a cage.** The standup checks whether
> either is 7+ days stale and asks; when nothing is due, it says nothing at all.

> **Why `coding` is its own profile.** The model that writes the nicest standalone function
> is often *not* the one that can run tools, read its own errors, and manage a twelve-file
> project without losing the plot. Those are different leaderboards, and `/models-research` reports
> them separately. This is the profile the website gets built on.

> **`/spend-report` is incremental by design.** It stores a watermark in `.spend-state.json`. Run it
> today after running it yesterday, and it covers only since yesterday — nothing counted
> twice, nothing researched twice. Lifetime totals come from its own stored logs, not from
> re-querying. **A well-built tool remembers what it already did and refuses to repeat
> itself.**

## The case study

A **fence installation company**. Unglamorous, real, and every town has fifty with terrible
websites.

| | |
|---|---|
| Goal | Quote requests. Phone rings and form submissions. Nothing else counts |
| Agency quote | $4,000–8,000, 6–8 weeks |
| Our build | ~66 minutes, a few cents of tokens, $0 hosting |
| Swap for | Roofing, landscaping, pools, decking, plumbing, HVAC |

**Why a fence company:** nobody expects a fence company's website to be beautiful. That gap
between expectation and result is the whole point.

## The ten steps

| # | Step | Output |
|---|---|---|
| 1 | Install the engineering skills | Superpowers + frontend-design |
| 2 | **Set the goals** — what must this achieve, and what would count as failure? | `goals.md` |
| 3 | **Build the knowledge base** — interview the client; the agent *looks at* their photos | `client-brief.md`, `assets/image-inventory.md` |
| 4 | **Design the layout & functionality** — you describe, the agent specs | `layout.md`, image prompts |
| 5 | **Architecture & stack** — then make the AI argue against itself | `architecture.md` |
| 6 | **The plan** — approve it before a line is written | `plan.md`, `HERMES.md` |
| 7 | **Build** — best coding model, real photos | The site |
| 8 | **Verify** — evidence, not assurances | Proof |
| 9 | **Git + deploy** | **A live URL** |
| 10 | What it's worth | The pricing conversation |

## The ideas that carry the module

**Process beats prompt.** A better prompt makes one answer better. A *process* makes every
answer better. Left alone, an agent starts writing code in thirty seconds and builds the
wrong thing beautifully.

**Goals are how a non-engineer wins an argument with an AI.** When the agent offers a gorgeous
animation that pushes the quote button below the fold, `goals.md` is what lets you say no.

**The description is the most important line in a skill.** It's the only part the agent sees
when deciding whether to run. A perfect skill with a vague description never fires. *That's*
what you review — not the code.

**The senior move is removing things, not adding them.** This site probably doesn't need a
database. Working out that it doesn't is worth more than building one.

**One task per session.** A *session* is one continuous conversation; a *task* is one unit of
work. Finish the task, `/reset`, then start the next one. This kills context rot, cuts cost,
and makes model switching free — at a reset there's no accumulated context to re-read, so the
prompt-cache penalty never applies. Reset *between* things, never *during* one: the website
build is a single long task, and you don't reset in the middle of it.

**The best output is often no output.** A skill that reports "no change" every morning trains
you to stop reading it. `/models-research` and `/spend-report` run weekly and stay silent when nothing's due.

**Your context files are a bill.** `hermes prompt-size` tells you what `SOUL.md` costs on
every single message. That's why the 200-line rule exists.

## Files in this module

- [`syllabus.md`](./syllabus.md) — the full syllabus: every segment, every brief
- [`agenda.md`](./agenda.md) — the 110-minute run-of-show
- [`luma-description.md`](./luma-description.md) — event copy
- [`student-guide.md`](./student-guide.md) — **follow along here**: every step, link, and
  copy-pasteable brief
- [`quiz.md`](./quiz.md) — 5 multiple-choice + 4 open-ended (peer-evaluated)

## Before the session

**You need Module 1's setup working** — Hermes running in your notes folder, connected to
Morpheus. If you missed it, the [Module 1 student
guide](../module-01-agent-and-second-brain/student-guide.md) takes about 45 minutes.

**Create these first — all free to start:**

| Account | Why | Cost |
|---|---|---|
| [OpenRouter](https://openrouter.ai) | The `fast` and `smart` model profiles | **Add ~$5 credit** |
| [GitHub](https://github.com) | Version control + deployment source | Free |
| [Vercel](https://vercel.com) | Where the site goes live | Free tier |

**Bring:** a real local business with a bad website. You'll rebuild the session for them as
homework.

> **Note on privacy:** OpenRouter is **not private** — it's the fast, cheap option. Morpheus
> stays your private brain. The `/model-switch` skill you build will stop you before you send
> client data to a public provider, but the judgment is still yours.

## Homework

1. Run `/daily-standup` every morning for a week. Run `/models-research` and `/spend-report` **once** that
   week. Report whether weekly felt like the right cadence.
2. **Practise one task per session** — `/reset` between tasks for a week and report what
   changed: quality, cost, or both.
3. Get `SOUL.md` good, then **cut it under 150 lines.** Report `hermes prompt-size` before
   and after — the cut is the assignment.
4. Use `/model --once` three times. Note when the expensive model was worth it.
5. **Rebuild the site for a real local business in your town.** Goals and discovery properly.
6. Lighthouse mobile performance **above 90**.
7. **Deploy to a real live URL.**
8. Write a one-page proposal for that business. *Optional, high-value: actually send it.*

## The honest caveat

What you build in this session is an **excellent starting point, not a finished client
delivery.** Real client work adds their photos, their copy, their domain, revisions, and
support.

The hour we save is the hour of *building* — which was never the valuable hour. **The
valuable hour is Steps 2, 3 and 4**, where you work out what to build. The agent can't do
that part. It doesn't know the fence business, and it doesn't know the owner.

**That's the job you're selling.**

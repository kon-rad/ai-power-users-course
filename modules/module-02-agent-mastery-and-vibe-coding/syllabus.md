# Module 2 — Agent Mastery and Vibe Coding a Pro Website

**Status:** final.
**Date:** 2026-08-06
**Prereq:** Module 1 complete — Hermes running in a `SecondBrain` folder, connected to
Morpheus, `/daily-standup` working.
---

## Format

**One session · 110 minutes · one break at the midpoint · live on YouTube.**

| Part | Time | Theme |
|---|---|---|
| **Part 1** | 0:00–0:34 | Agent mastery — your agent builds its own tools |
| **Break** | 0:34–0:44 | |
| **Part 2** | 0:44–1:50 | Vibe coding a pro website — client site, live and deployed |

110 minutes sits comfortably inside the course's original 3-hour daily block, so this is
one Luma event, one stream, one YouTube video.

**The spine:** *we never open a code editor to write a skill, and we never write a line of
the website.* We describe, review, and approve. The student's job is **editor, not author.**

---

## Learning objectives

By the end, a student can:

1. Explain the difference between a **session** and a **task**, and run **one task per
   session** — resetting between them so model switching never costs them anything
2. Get their agent to **build its own skills** from a spoken brief
3. Explain what a token is, and measure what their context files cost per message
4. Tell `SOUL.md` (who the agent is) from `HERMES.md` (how this job works)
5. Turn a business conversation into a structured **knowledge base** — text *and* images
6. Describe an app's goals, layout, and functionality clearly enough for an agent to build it
7. Judge a tech-stack recommendation without being an engineer — including saying no to a database
8. Ship a real, live, deployed website with git version control

---

# PART 1 — Agent Mastery (34 min)

## 0:00–0:04 · Open with the payoff

Show the **finished, live fence-company website on a phone**, then the price of the same
thing from an agency: *$4,000–8,000, six to eight weeks.*

> "We're building that in the second half. First we're going to make your agent smart about
> which model to use and what it costs — because that's what makes the second half cheap."

Then the module's premise:

> "In Module 1 you wrote a skill by hand, so you'd see the machinery. **You never have to do
> that again.** Today you describe what you want, and the agent writes it."

---

## 0:04–0:12 · Models, tokens, and the cache trap (8 min)

### Teach (3 min)

- **A token is ~¾ of a word.** You pay for input *and* output; **output usually costs 3–5×
  input.**
- **Every turn re-sends the whole conversation.** Long sessions get expensive *and* dumb.
- **Context rot:** as context fills, recall degrades — a gradient, not a cliff.

### Demo 1 — context rot (90 sec)

Fresh session vs. a **pre-bloated** session, same question. The bloated one is visibly
worse.

> "Same model, same prompt, more noise. The fix is free: start a new session."

Prepare the bloated session in advance. Don't try to bloat one live.

### Command naming — collision check (verified against the Hermes docs)

Skills become slash commands automatically, so a skill named after a built-in is
shadowed or ambiguous. Checked before naming:

| Wanted | Verdict | Shipped as |
|---|---|---|
| `/models` | `/model` is built-in, confusingly close | **`/model-research`** |
| `/switch` | **TAKEN.** Built-in alias for `/sessions` | **`/switch-models`** |
| `/spend` | Free, but `/usage` and `/topup` are built-ins covering nearby ground | **`/spend-tracker`** |

Also built-in and worth avoiding: `/profile`, `/usage`, `/status`, `/context`, `/tools`,
`/sessions`, `/skills`, `/background`, `/steer`, `/stop`, `/help`.

> **The lesson for students:** before you name a skill, run `/help` and check the name
> isn't taken. A shadowed skill fails silently — it just never fires, and you'll waste an
> hour wondering why. Add a second word so the name is unambiguous: `model-research`,
> `switch-models`, `spend-tracker`.


### Demo 2 — switching, natively (2 min)

```
/model                                        → where am I?
/model <name> --provider openrouter           → this session only
/model <name> --provider openrouter --global  → and persist it
/model <name> --once                          → one turn, then auto-restore
/model fast                                   → via a named alias
```

`--once` is the crowd-pleaser: *"use the expensive model for exactly this one hard
question, then drop back automatically."*

Aliases, set once:

```bash
hermes config set model.aliases.fast   openrouter/<cheap-model>
hermes config set model.aliases.smart  openrouter/<frontier-model>
hermes config set model.aliases.coding openrouter/<best-agentic-coding-model>
```

**Four profiles, because "best model" depends entirely on the job:**

| Profile | For | Chosen on |
|---|---|---|
| **`private`** | Client, financial, personal work | Privacy. Morpheus, always |
| **`fast`** | Bulk, boring, summarising, tidying | Price |
| **`smart`** | Judgment, client-facing writing | General capability |
| **`coding`** | Building software, multi-file projects, debugging | **Agentic coding ability — a different ranking entirely** |

> **Why `coding` is its own profile:** the model that writes the nicest standalone function
> is often *not* the model that can run tools, read its own errors, and manage a twelve-file
> project without losing the plot. Those are different leaderboards. **We use this profile in
> the second half to build the website.**

### Demo 3 — the cache trap, and the rule that dissolves it (3 min)

Switch models mid-conversation, send a message, show the token count jump.

> "Switching breaks the prompt cache. The cache is keyed to the model serving the request,
> so your next message re-reads the **entire** conversation at full input price."

**Now define the two words, because they get used loosely and the difference is the whole
lesson:**

| Term | What it is | Boundary |
|---|---|---|
| **Session** | One continuous conversation. From launching `hermes` (or `/reset`) until you reset or quit. **Context accumulates the whole time** | `/reset`, or quit and relaunch |
| **Task** | One unit of work. "Build the spend skill." "Build the fence website." | When the work is done |

### The rule: one task per session

> "Most people run one giant session all day and wonder why the agent gets dumber and the
> bill gets bigger. **Do one task per session. When the task is done, `/reset`.**"

That single habit solves three problems at once:

| Problem | Why one-task-per-session fixes it |
|---|---|
| **Context rot** | Never gets a chance to build up |
| **Cost** | You stop re-sending an irrelevant conversation on every turn |
| **The cache trap** | **Disappears entirely.** At a reset the context is empty, so there's nothing to re-read. Switching models is free at that moment |

> "So the real rule isn't 'switch carefully'. It's: **finish the task, reset, then switch.**
> Do that and you never pay the cache penalty at all."

### The nuance — a task can be long

**Don't reset mid-task.** The website we build after the break is *one task*, even though
it's 66 minutes and dozens of messages — the agent needs to remember the brief, the layout,
what it already built, and your last three corrections. Resetting there would mean
re-explaining everything.

> "Reset **between** things, never **during** one. And inside a long task like the website —
> pick your model at the start and stay on it."

If a genuinely long task outgrows the window, that's what compaction is for — not a reset.

### `/reset` vs quitting (30 sec)

- **`/reset`** — clears the conversation. Fast. Right for most task boundaries.
- **Quit and relaunch** — also re-reads config and skills. **Required after you install a
  skill, change an API key, or add a provider.** Only context length, compression and
  message limits hot-reload.

> "You'll use `/reset` twenty times a day and a full restart maybe twice."

### Safety beat — 60 seconds, do not skip

`fast`, `smart` and `coding` all point at OpenRouter. **OpenRouter is not private.** Module 1 established
Morpheus as the private brain; the moment a second provider appears, say the trade-off out
loud.

> *If you wouldn't paste it into a public chatbot, don't run it on a non-private profile.*

---

## 0:12–0:24 · BUILD BY ASKING: three skills, one brief (12 min)

**The centrepiece.** Read this to the agent, on screen, verbatim.

```
Build me three skills. Ask me anything ambiguous first, then write all three, tell me
where you put them, and run each one once.

────────────────────────────────────────────────────────────
SKILL 1 — `model-research`
Researches the current AI model landscape and writes me a briefing.

CADENCE: WEEKLY, not daily. The model landscape does not move fast enough to justify
a full research pass every morning — that's wasted tokens and wasted attention.
Run it once a week, plus any time I explicitly ask.

RUNS WHEN: I ask what model to use, ask about model costs, ask what's new in models,
or when my weekly briefing is due (7+ days since the last file in the briefings
folder). If the most recent briefing is less than 7 days old and I haven't explicitly
asked for a fresh one, say so, show me that briefing's TL;DR, and do NOT re-research.

COVERS six categories: text/chat · text-to-image · image-to-video · text-to-speech ·
speech-to-text · music generation. Plus image editing.

SOURCES — fetch live every time. Never answer from memory, never reuse yesterday's
numbers:
  - https://openrouter.ai/api/v1/models  (no API key needed). Returns id, name,
    context_length, architecture.output_modalities, and pricing.prompt /
    pricing.completion in USD PER TOKEN — multiply by 1,000,000 for the per-million
    figure people quote.
  - https://artificialanalysis.ai/image/leaderboard/text-to-image
  - https://artificialanalysis.ai/video/leaderboard/image-to-video
  - https://artificialanalysis.ai/text-to-speech/arena
  - Image editing, speech-to-text, music: web search, prefer sources from the last
    60 days, state the date of each source.

FOR TEXT MODELS give four picks: cheapest usable (context >= 100k), best value,
frontier, and any genuinely free models. Also flag the best model for AGENTIC CODING
specifically — that's a different ranking from general intelligence.

OUTPUT to 2-Areas/AI-Models/YYYY-MM-DD-model-briefing.md:
  - TL;DR, max 10 lines — one line per category, model + rough cost
  - A table per category: model, provider, price, limits, one-line why
  - "What changed" vs the most recent previous briefing in that folder. If nothing
    changed, say so in one line. Don't pad.
  - Suggested `fast` and `smart` aliases
  - Every source URL with the date fetched

RULES: if you can't verify it, say so — never guess a price. Never mix units in one
table. Flag any price move over 30% as PRICE ALERT.

────────────────────────────────────────────────────────────
SKILL 2 — `switch-models`
Changes which model I'm using.

Four profiles:
  private — Morpheus. Client, financial, or personal work. Never leaves privacy.
  fast    — cheap OpenRouter model. Bulk work, summarising, tidying, first drafts.
  smart   — frontier OpenRouter model. Judgment calls, client-facing writing.
  coding  — the best AGENTIC CODING model. Building software, multi-file projects,
            debugging. This is a different ranking from general intelligence: I want
            the model that runs tools, reads its own errors, and manages a whole
            project — not the one that writes the prettiest standalone function.

RUNS WHEN: I say switch to fast/smart/private, ask what model I'm on, or ask which
model I should use for a task.

DOES:
  - `/switch-models <profile>` switches using Hermes's native /model command and my
    configured model_aliases. Check the Hermes docs for exact syntax first.
  - `/switch-models` with no argument: tell me what I'm on and suggest a profile based on
    what we're working on right now.
  - If I'm switching mid-task, WARN ME that it breaks the prompt cache and the next
    message re-reads the whole conversation at full input cost. Suggest waiting for
    a task boundary.
  - If I'm switching AWAY from private while we're working on anything that looks
    like client data, financials, or personal notes — STOP and make me confirm.
  - Keep fast/smart/coding in sync with the latest briefing in 2-Areas/AI-Models/.
    If it recommends better ones, offer to update my aliases. The `coding` pick must
    come from the briefing's AGENTIC CODING section, not its general ranking.

────────────────────────────────────────────────────────────
SKILL 3 — `spend-tracker`
Tracks what my AI usage costs. INCREMENTAL — never redoes work it has already done.

CADENCE: WEEKLY, plus on demand whenever I ask.

THE CORE RULE — only ever process what's new:
Keep a watermark in 2-Areas/AI-Costs/.spend-state.json:
      { "last_run": "<ISO timestamp>", "last_covered_through": "<ISO timestamp>",
        "totals_to_date": { "tokens_in": N, "tokens_out": N, "cost_usd": N } }

On every run:
    1. Read the watermark.
    2. Query `hermes insights` ONLY for the period since `last_covered_through`.
Never re-query a period already recorded.
    3. Append the new records to the monthly log.
    4. Update the watermark and the running totals.
    5. Report the NEW period. For lifetime or month-to-date figures, read my own
       stored log files — do not re-query the source.

So if I ran this yesterday and it recorded two days, and I run it again today,
  today's run covers only since yesterday's run. Nothing is counted twice and
  nothing is researched twice.

EDGE CASES:
  - No state file (first ever run): pull the full available history, write the log,
    set the watermark. Say clearly that this was a first full import.
  - Nothing new since last run: say exactly that in one line, with the timestamp of
    the last run. Do NOT re-report the previous period and do NOT invent analysis.
  - State file missing, corrupt, or with a future timestamp: fall back to the latest
    entry in the monthly logs, say you did so, and continue.
  - If a run fails partway, do not advance the watermark — better to re-cover a
    period than to lose one.

DOES:
  - Use Hermes's built-in `hermes insights`. Check the docs for the exact flags and
    output format, and for how to bound it to a date range.
  - Report: the new period since last run · month to date (from my logs) · the trend
    versus the previous comparable period.
  - Break the new period down by project and by model.
  - Flag any session over 2x my recent average as a SPIKE and say what caused it —
    usually a long session or an expensive model doing cheap work.
  - Run `hermes prompt-size` and tell me what my context files cost per message.
  - Give me exactly ONE concrete change to cut cost. If the current model briefing
    has a cheaper model that would do the job, name it.
  - Append to 2-Areas/AI-Costs/YYYY-MM-token-log.md, one file per month. The log is
    the permanent record: date, period covered, tokens in/out, cost, models used,
    projects.

RULES: estimates are estimates — say so, never present them as an invoice. If I've
spent nothing, say that in one line. Don't manufacture analysis.
```

### While it works — host guidance

- **Narrate.** Silence kills a stream.
- **Answer its clarifying questions on camera.** That interaction *is* the lesson — an agent
  resolving ambiguity *before* building is exactly the behaviour we want students to expect.
- **Open the generated `SKILL.md` in VS Code** and read the description line aloud:

> "This line is the most important part of the whole skill — it's the only part the agent
> sees when deciding whether to run it. A perfect skill with a vague description never
> fires. **This is what you review.** Not the code. The description."

- **Run `/model-research`.** Open the dated note in Obsidian. Payoff shot.
- **Show the raw OpenRouter JSON for three seconds.** Students should see this is just
*data*, not magic.

> **Point out what makes `/switch-models` a good skill:** it does nothing Hermes can't already do.
> It adds **judgment** — the cache warning and the privacy stop. **Automating a command is a
> shortcut. Automating a decision is a skill.**

### Verified 2026-08-06

OpenRouter's endpoint: **HTTP 200, no auth, 338 models**, full per-token pricing, 17 free,
11 image-output, 4 audio-output. All three Artificial Analysis leaderboards: 200.
**Re-verify the morning of the stream.**

---

## 0:24–0:31 · SOUL.md vs HERMES.md (7 min)

### Two files, two jobs

| File | Scope | Holds | Example |
|---|---|---|---|
| **`~/.hermes/SOUL.md`** | **Global** — everywhere | Who the agent **is** | "Be direct. No preamble. Say 'I don't know' rather than guess." |
| **`HERMES.md`** | **This project only** | How **this job** works | "Client is a fence company. Stack is Next.js. Copy comes from `client-brief.md`." |

Project priority, first match wins: `.hermes.md` → `AGENTS.md` → `CLAUDE.md` →
`.cursorrules`. `SOUL.md` loads independently on top.

> **The rule:** *if it's true everywhere, SOUL.md. If it's true for this job, HERMES.md.*
> Get it wrong and one giant file bloats every session you ever run.

### Make it measurable (90 sec)

```bash
hermes prompt-size
```

Note the number. Add 100 lines to SOUL.md. Run it again.

> "This is why the 200-line rule exists. It's not style advice — **it's a bill**, and you pay
> it on every single message."

### Write SOUL.md by asking

```
Look at how I've been talking to you, the notes in my second brain, and the writing in
my Projects folder. Draft a SOUL.md that captures how I actually work and how I want
you to behave.

Include: how to talk to me, my writing voice with real examples pulled from my own
notes, my defaults for filing and naming, and explicit permission to tell me when I'm
wrong.

Keep it under 150 lines. Ask me about anything you're unsure of rather than inventing
it. Then show me `hermes prompt-size` before and after.
```

**The two moves that make it good:** real writing samples pulled from the student's own
notes (a template can never do that), and **explicit permission to be uncertain** — the
single highest-value anti-hallucination line a beginner can add.

**Prove it:** same question before and after, side by side.

---

## 0:31–0:34 · Wire it into the standup — weekly, not daily (3 min)

```
Update my daily-standup skill so it knows about the two new skills — but WEEKLY, not
every day. I don't want a full research pass every morning; that's wasted tokens and
wasted attention.

At the start of the standup:
  1. Check when `model-research` last ran (newest file in 2-Areas/AI-Models/) and when
`spend-tracker` last ran (last_run in 2-Areas/AI-Costs/.spend-state.json).
  2. If either is 7+ days old, say so in ONE line and ASK me whether to run it now.
Don't run it without asking — it's a bigger job than a standup.
  3. If both are current, say nothing at all about them. Silence is the correct
     output when there's nothing to report.
  4. If the last model briefing is current AND it recommends better fast/smart models
     than my aliases, mention that in one line.

Then continue with the normal standup.
```

Run `/daily-standup`.

> "Notice what it does when nothing is due: **nothing.** A skill that reports 'no change'
> every single morning trains you to stop reading it. **The best output is often no
> output.**"

### Why weekly is the right cadence

| | |
|---|---|
| **`/model-research`** | The landscape moves in weeks, not hours. A daily research pass burns tokens to tell you the same thing five times |
| **`/spend-tracker`** | Weekly is enough to catch a bad habit before it costs real money — and because the skill is **incremental**, a weekly run covers the whole week without re-processing anything |
| **Both** | Available **on demand** whenever you actually want them. Cadence is a default, not a cage |

**Teaching point:** three small single-purpose skills composed into one routine beats one
giant skill — each piece can be fixed, tested and reused alone. And **`/spend-tracker`'s watermark is
the lesson in miniature:** a well-built tool remembers what it already did and refuses to
repeat itself. That's the difference between automation and busywork.

---

# BREAK — 10 minutes (0:34–0:44)

Tell people exactly what's coming so nobody drifts off:

> "When we come back: a real client, a real website, and a real live URL by the end. Grab
> a coffee."

---

# PART 2 — Vibe Coding a Pro Website (66 min)

**The client:** a fence installation company. Unglamorous, real, every town has fifty with
terrible websites — and **nobody expects a fence company site to be beautiful.** That gap is
the whole emotional payload. Swap for roofing, landscaping, pools, decking.

## Step map

| # | Step | Time | Output |
|---|---|---|---|
| 1 | Install the engineering skills | 4 min | Superpowers + frontend-design |
| 2 | **Set the goals** | 5 min | `goals.md` |
| 3 | **Build the knowledge base** — info + images | 10 min | `client-brief.md`, `assets/` |
| 4 | **Design the layout & functionality** | 9 min | `layout.md`, image prompts |
| 5 | **Architecture, stack & database** | 7 min | `architecture.md` |
| 6 | **The plan + project rules** | 4 min | `plan.md`, `HERMES.md` |
| 7 | **Build** | 18 min | The site |
| 8 | **Verify** | 5 min | Evidence, not assurances |
| 9 | **Git + deploy** | 8 min | **Live URL** |
| 10 | What it's worth | 3 min | The pricing conversation |

---

## Step 1 · Install the engineering skills (4 min)

```
Install the Superpowers skills for software development: brainstorming, writing-plans,
systematic-debugging, and verification-before-completion. They're at
github.com/obra/superpowers and Hermes can install skills from a raw SKILL.md URL.
Also install a frontend-design skill for production-grade UI.

Install them, list what you installed, and confirm each one loads.
```

### Why a process beats a better prompt

> "A better prompt makes one answer better. A **process** makes every answer better."

Left alone, an agent building a website starts writing code in thirty seconds and **builds
the wrong thing, beautifully.** Name that failure mode — students will recognise it all
week. These skills force: understand → plan → get approval → build → prove it works. That's
what $5,000 buys. **The code was never the expensive part.**

**Verify the install a week ahead.** Superpowers is authored for Claude Code and
Hermes-native packaging is an open request in both repos. Have direct raw URLs as fallback.

---

## Step 2 · Set the goals (5 min)

Before anything else. This is the step everyone skips and it's why most sites fail.

```bash
mkdir -p ~/SecondBrain/1-Projects/fence-co-website
cd ~/SecondBrain/1-Projects/fence-co-website
hermes
```

```
We're building a website for a fence installation company. Before anything else, help
me write goals.md.

Ask me: what does this site need to ACHIEVE for the business? How will we know it
worked? What's the one action a visitor should take? What would make this a failure
even if it looks beautiful?

Push back if my goals are vague or unmeasurable.
```

**The answer to have ready:**

| | |
|---|---|
| **Primary goal** | Quote requests. Phone rings and form submissions. That's the entire business case |
| **Success measure** | 10+ quote requests a month, up from ~2 |
| **The one action** | Tap "Get a Free Quote" — or tap the phone number |
| **Failure mode** | A beautiful site nobody contacts. Pretty and silent is worse than ugly and busy |

> "Write that down, because in twenty minutes when the agent offers you a gorgeous
> full-screen animation that pushes the quote button below the fold, **`goals.md` is what
> lets you say no.** Goals are how a non-engineer wins an argument with an AI."

---

## Step 3 · Build the knowledge base (10 min)

Both halves matter: **what the business is**, and **what it looks like.**

### 3a — The business (7 min)

```
Use the brainstorming skill. Interview me as the fence company owner — ask what you
need to design and build a site that gets quote requests. One question at a time. Save
what you learn to client-brief.md as we go.
```

**Have real answers ready.** This is what students under-prepare:

| Question | Answer |
|---|---|
| Business & area | *Northline Fencing, family-run 18 years, 40-mile radius* |
| Services | *Cedar privacy, ornamental aluminium, chain link, farm & ranch, gates, repairs* |
| Customers | *60% homeowners (privacy, dogs, pools), 40% commercial* |
| Current site | *2011 template, not mobile, no photos, phone number inside an image* |
| Competitors | *All identical and cheap-looking* |
| Differentiator | *Licensed, insured, 10-year workmanship warranty, own crew — no subcontractors* |
| Objections | *"Are they fly-by-night?" and "what will this cost?"* |
| Tone | *Established and trustworthy. Not corporate, not cheap* |
| Service towns | *List 8–10 real town names — this is the local SEO* |

### 3b — The images (3 min)

**Images are knowledge base too.** This is the part that separates a real site from a
template.

```bash
mkdir -p assets/photos
# drop 15-20 job photos in — stock is fine for the demo
```

```
Look at every image in assets/photos. For each one, write a line in assets/
image-inventory.md: what it shows, what fence type, whether it's a good hero
candidate, and alt text for accessibility and SEO.

Then tell me what's MISSING — what photos would this business need to shoot to make
this site convert better?
```

> **Two lessons in one step.** The agent can *see* — it reads the photos and writes the alt
> text, which is real SEO work most agencies charge for. And the gap analysis is a genuine
> consulting deliverable: *"here's the shot list for your photographer."* **You just found
> your second invoice.**

### The beat

Open `client-brief.md` and `image-inventory.md` side by side.

> "That's the knowledge base. Every decision from here — colours, copy, which photo goes in
> the hero — comes from **these files**, not from the agent's imagination. **This is why
> your site won't look like every other AI site.**"

---

## Step 4 · Design the layout & functionality (9 min)

**Your step**, and the one most people skip. You describe the site out loud; the agent turns
words into a spec.

```
I'm going to describe the site page by page. Turn it into layout.md — sections in
order, what's in each, what the visitor should do next, and how it changes between
mobile and desktop. Cross-check everything against goals.md, and tell me if anything
I describe works against the goal.

Homepage, top to bottom:
  - Full-width hero: a real cedar fence at golden hour, from image-inventory.md.
Headline about protecting what's yours. Quote button and phone number, both
    visible without scrolling.
  - Trust strip immediately under: licensed · insured · 18 years · 10-year warranty.
Four small badges in a row.
  - Five service cards, photo each: cedar privacy, ornamental aluminium, chain link,
    farm and ranch, gates and repairs.
  - Before-and-after gallery — a slider you drag.
  - Short "why us" with a photo of the actual crew.
  - Quote form: name, phone, fence type, rough length, photo upload.
  - Footer: service area town list, phone, licence number.

Mobile: quote button sticky at the bottom of the screen the whole way down.
```

### Then functionality, explicitly

```
Now write the functionality spec into layout.md. For each interactive thing: what the
user does, what happens, what happens when it fails, and what the business owner sees.

  - Quote form: validation, where the email goes, what the visitor sees after
    submitting, what happens if it fails
  - Photo upload: size limits, formats, what if it's too big
  - Before/after slider: touch and mouse, keyboard accessible
  - Sticky mobile quote button: when it appears, when it hides
  - Phone number: tappable on mobile, click-to-call
```

### Then the images

```
Read client-brief.md and image-inventory.md. Write 5 image-generation prompts for any
hero or section images we're missing, in the style the brief implies: established,
trustworthy, premium, warm cedar tones. Real, not stock-photo-fake.
```

### Why this segment earns nine minutes

> "You just did the expensive part of this job. **Anyone can ask an AI for a website. Almost
> nobody can say what should be on it.** That's what clients pay for, and it's the part the
> agent can't do — it doesn't know the fence business or this owner. **You do.**"

This is the standard 2026 design-agent workflow — *idea → research → wireframe → UI →
assets → handoff* — compressed into one conversation. Name it.

---

## Step 5 · Architecture, stack & database (7 min)

### Ask for a decision, not a menu

```
Read goals.md, client-brief.md, image-inventory.md and layout.md. Recommend the
architecture and stack. Write it to architecture.md.

Constraints:
  - I'm not a programmer. I maintain this or hand it to the client.
  - Fast on mobile — customers are on phones in driveways.
  - Must rank in local search. SEO is not optional.
  - Quote form must email the owner reliably, with the photo attached.
  - Hosting free or near-free.

Give me ONE recommendation, not a menu. For each piece: what it is in one sentence,
why it beats the obvious alternative here, and what it costs.

Then: does this site actually need a database? Make the case both ways and recommend.
```

### Expect roughly

| Layer | Likely | Why |
|---|---|---|
| Framework | **Next.js** | Static generation = fast + SEO-friendly |
| Styling | **Tailwind** | Fast, consistent, agent-friendly |
| Forms | **Resend / Formspree** | Email without running a backend |
| Images | Next.js Image + CDN | Mobile performance |
| Hosting | **Vercel** | Free tier, git-push deploy, free SSL |
| Database | **Probably none** | Quote requests → email |

### The two critical moves

**1. Make it argue against itself.**

```
Now argue against your own recommendation. What would a senior engineer say is wrong
with this for this specific client? What's the simplest thing that would work, and am
I over-building?
```

> **Why:** models tend to agree with your framing. Asking for the counter-case is how a
> non-expert gets a second opinion. **Best 15 seconds in the session.**

**2. The database conversation is the real lesson.**

> "The answer is probably no — and that's worth more to you than a database would be. Every
> piece of tech you add is a thing that breaks, costs money, and needs maintaining. **The
> senior move is removing things, not adding them.** When this client has 200 quotes a month
> and needs to track which ones closed — *then* they need a database. **And that's a second
> invoice.**"

---

## Step 6 · The plan + project rules (4 min)

```
Use the writing-plans skill. Based on goals.md, client-brief.md, layout.md and
architecture.md, write plan.md — steps I can approve one at a time. Flag anything
you're unsure about instead of assuming.

Also write a HERMES.md for this folder so any future session knows the client, the
stack, where the brief and layout live, and the design direction. Keep it short — it
loads on every message.
```

**Read the plan on stream. Then change it** — the most important non-programmer skill in the
course:

```
Two changes. Quote form above the gallery — quote requests are the whole point, and
goals.md says so. And the 10-year warranty has to be visible without scrolling; it's
their main objection.
```

> "Editing a plan takes ten seconds. Editing a built site takes ten minutes. **This is where
> non-programmers get their power back** — you can't review the code, but you can absolutely
> review the plan."

Callback: HERMES.md here, SOUL.md from Part 1. *Project rules vs. who your agent is.*

---

## Step 7 · Build (18 min)

```
/switch-models coding
```

> "This is what the fourth profile is for. The best **agentic** coding model runs tools,
> reads its own errors, and manages a multi-file project — a different leaderboard from
> 'writes a nice function'. **Ask `/model-research` which one that is today** — I won't name one,
> because it'll be wrong by the time you watch this."

And a callback to Part 1: **we're switching at a task boundary.** Fresh session, empty
context, nothing to re-read. The switch costs nothing.

```
Use the frontend-design skill. Execute plan.md.

Design direction from client-brief.md: established, trustworthy, premium. Not cheap,
not corporate. Their competitors all look identical — we don't.

Use the real photos in assets/photos with the alt text from image-inventory.md.

Non-negotiable:
  - Phone number as real tappable text, never inside an image
  - Sticky quote button on mobile
  - Warranty badge visible without scrolling
  - Town names in headings, LocalBusiness schema
  - Works at 375px wide
```

### Host guidance

- **Narrate constantly.**
- **Show the diff.** Module 1's *"never trust 'done'"*.
- **Iterate visibly** — the first result won't be right, and showing that is the point:

```
The hero looks like an insurance company. Warmer, more craftsman. Use the cedar tones
from the brief.
```

- **When something breaks, use it.** Invoke `systematic-debugging` on camera. A failure
  handled well teaches more than a clean run.

### Contingency

**Protect the last 16 minutes for verify + deploy.** If behind at 1:12, cut the gallery and
ship hero + trust strip + services + form. **A shipped rough site beats a perfect plan** —
value #5, literally.

---

## Step 8 · Verify (5 min)

```
Use the verification-before-completion skill. Prove this works:
  - Every link goes somewhere real
  - Quote form submits and the email actually arrives
  - Photo upload works from a phone
  - Works at 375px wide
  - Lighthouse performance and accessibility
Show me evidence for each, not assurances.
```

> "The agent said 'done' twenty minutes ago. This is how you find out whether that was true.
> **Agents are optimistic. Make them prove it.**"

Then **check it yourself** at phone width. Non-negotiable: your eyes on it before the
client's.

---

## Step 9 · Git + deploy + live (8 min)

The segment that turns a demo into a deliverable.

```
Set this up properly and get it live:
  1. Initialise git. Write a .gitignore that keeps secrets and node_modules out.
  2. Check no API keys are in any file you're about to commit. Check before committing.
  3. Commit with a sensible message.
  4. Create a GitHub repo and push.
  5. Deploy to Vercel from that repo.
  6. Set any environment variables the form needs.
  7. Give me the live URL.

Explain each step as you go, and tell me what changes when the client wants their own
domain.
```

### Three beats

- **Git is the undo button for your whole project.** Not a programmer thing — a *not losing
  your work* thing.
- **The secrets check is not optional.** *Push a key to a public repo and it's scraped in
  minutes.* Full treatment in Module 4; the habit starts here.
- **Git push → live site** is the loop that runs the modern web. Change a file, push, live in
  30 seconds.

**End on the live URL, open on a phone, on camera.**

---

## Step 10 · What it's worth (3 min)

| | |
|---|---|
| Agency quote | **$4,000–8,000, 6–8 weeks** |
| What it cost us | **~66 minutes and a few cents of tokens** |
| Hosting | **$0** on Vercel's free tier |
| What to charge | Not "cents plus margin." You sell the **outcome** — more quote requests — not your build time |
| What's actually yours | The client relationship, the goals, the brief, the judgment about what to leave out. The agent has none of those |

> **The honest caveat, said plainly:** what we built is a **very good starting point**, not a
> finished client delivery. Real work adds their photos, their copy, their domain, revisions,
> support. The hour we saved was the hour of *building* — which was never the valuable hour.
> **The valuable hour was Steps 2, 3 and 4**, where we worked out what to build. The agent
> couldn't do that part. **That's the job you're selling.**

---

# Homework

1. Run `/daily-standup` **every morning for a week.** Run `/model-research` and `/spend-tracker` **once**
   during that week. Bring the weekly briefing and say whether weekly felt right.
2. **Practise one task per session.** `/reset` between tasks for a whole week and report
   what changed — quality, cost, or both.
3. Get SOUL.md good, then **cut it under 150 lines.** Report `hermes prompt-size` before and
   after — **the cut is the assignment.**
4. Use `--once` at least three times. Note when the expensive model was worth it.
5. **Rebuild the site for a real local business in your town** — a real one with a bad
   website. Do goals and discovery properly even if you invent the answers.
6. Get **Lighthouse mobile performance above 90.** The hard part, and where the real learning
   is.
7. **Deploy it to a real live URL.**
8. Write a one-page proposal: what's wrong with their current site, what you'd build, what
   you'd charge. **Optional, high-value: actually send it.**

# Peer quiz

**Multiple choice (5):** what a token is · which costs more, input or output · **the
difference between a session and a task** · SOUL.md vs HERMES.md · why you check for secrets
before committing.

**Open, peer-scored (4):**
- *Why does one-task-per-session make the prompt-cache problem disappear rather than just
  reduce it?*
- *Paste `goals.md`. How did it help you say no to something?*
- *What would the site have gotten wrong if you'd skipped Steps 2–4?*
- *Did your agent recommend a database? Was it right?*
- *Paste a skill brief you wrote yourself. Could a peer build the same skill from it?*

**Peer exercise:** open your peer's **live URL** on your phone. Would you request a quote?
One thing that works, one that doesn't.

---

# Prep checklist

## Must verify before the stream
- [ ] **Superpowers installs on Hermes** — authored for Claude Code, Hermes packaging is an
      open issue. Direct raw SKILL.md URLs as fallback. **Highest-risk item.**
- [ ] **New `model_aliases` mid-session** — do they need `/reset`? Adjust the `switch-models` brief
- [ ] Exact `hermes insights` flags and output format
- [ ] OpenRouter endpoint + all three Artificial Analysis URLs (were 200 on 2026-08-06)

## Assets
- [ ] Pre-bloated Hermes session for the context-rot demo
- [ ] All three profiles pre-configured via `hermes model`
- [ ] OpenRouter account with ~$5 credit
- [ ] **The finished fence site, built and deployed in advance** — shown at 0:00 and the
      fallback if the live build stalls
- [ ] 15–20 fence photos in `assets/photos` (stock fine)
- [ ] Client brief answers printed
- [ ] GitHub + Vercel logged in, tested end to end
- [ ] Resend/Formspree with a working test email
- [ ] A known-good build in reserve at each stage
- [ ] All skill briefs in the student guide as copy-pasteable blocks

# Open questions

1. **110 minutes acceptable for one stream?** It fits the course's 3-hour daily block. If it
   must be shorter, cut Step 3b (image inventory) and Step 10 — but 3b is a genuine
   differentiator.
2. **~$5 OpenRouter credit** — flag in the Luma description?
3. **Generate images live in Step 4, or stock?** Suggest: one hero image live, stock for the
   rest.
4. **`/model-research` daily or weekly?** The standup promises daily; the landscape doesn't move
   daily. Consider weekly deep research + daily price check.

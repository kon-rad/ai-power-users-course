# Module 2 — Student Guide

**Agent Mastery and Vibe Coding a Pro Website**
Follow along here. Every step, every link, every brief you can copy and paste.

By the end you will have three skills your agent wrote for itself, a tuned agent
personality, and a real client website live on a real URL.

---

## Before the session

### You need Module 1's setup working

The Hermes agent running inside your notes folder, connected to Morpheus. If you missed
it, the [Module 1 student guide](../module-01-agent-and-second-brain/student-guide.md)
takes about 45 minutes. Do it first — everything here builds on it.

Check it works. Open your terminal:

```bash
cd ~/SecondBrain
hermes
```

Then ask it something about your notes. If it answers, you are ready.

### Three accounts, all free to start

| Account | Why | Cost |
|---|---|---|
| [OpenRouter](https://openrouter.ai) | The `fast`, `smart` and `coding` model profiles | **Add about $5 of credit** |
| [GitHub](https://github.com) | Version control and the deploy source | Free |
| [Vercel](https://vercel.com) | Where your website goes live. Sign in with GitHub | Free tier |

Get your OpenRouter API key from **Keys** in the dashboard and keep it somewhere safe for
ten minutes. We will store it properly in the session.

> **On privacy.** Morpheus stays your private brain. OpenRouter is **not private** — it is
> the fast and cheap option. We will be explicit about which work goes where, and the
> `model-switch` skill you build will stop you before you send client data to a public
> provider.

### Bring a business

A real local business with a bad website — a plumber, landscaper, gym, café, fencing
company. You will rebuild the session for them as homework. Having a real one in mind
makes everything land harder.

---

# PART 1 — Agent Mastery

## Step 1 — Understand what you are paying for

**A token** is a chunk of text, roughly three-quarters of a word. Models see tokens, not
letters — which is why they miscount the r's in "strawberry".

You pay for **input** (everything you send, including your context files and the whole
conversation so far) and **output** (what it writes back). Output usually costs **three to
five times** input.

**Every turn re-sends the entire conversation.** That is why long sessions get expensive.

**Context rot:** as the context fills up, the model's recall degrades. Not a cliff — a
gradient. Your agent genuinely does get worse the longer a session runs.

Try it yourself:

1. In a fresh session, ask your agent to summarise a specific note. Note the quality.
2. In a session you have been using for an hour, ask **exactly the same question.**
3. Compare.

Same model, same prompt, more noise. **The fix is free: start a new session.**

## Step 2 — Switching models

Hermes has this built in. Inside a session:

```
/model                                        show where you are
/model <name> --provider openrouter           switch for this session only
/model <name> --provider openrouter --global  switch and persist it
/model <name> --once                          switch for one turn, then auto-restore
```

`--once` is the useful one: use the expensive model for exactly one hard question, then
drop back automatically.

### Set up four named profiles

Rather than memorising model names, give them aliases. Run these in your terminal
(**not** inside a Hermes session):

```bash
hermes config set model.aliases.fast   openrouter/<cheap-model>
hermes config set model.aliases.smart  openrouter/<frontier-model>
hermes config set model.aliases.coding openrouter/<best-agentic-coding-model>
```

Fill in the model names from the briefing your `models-research` skill produces in Step 3.

| Profile | For | Chosen on |
|---|---|---|
| `private` | Client, financial, personal work | Privacy. Morpheus, always |
| `fast` | Bulk, boring, summarising, tidying | Price |
| `smart` | Judgment, client-facing writing | General capability |
| `coding` | Building software, multi-file projects, debugging | **Agentic coding ability** |

> **Why `coding` is separate.** The model that writes the nicest standalone function is
> often not the one that can run tools, read its own errors, and manage a twelve-file
> project without losing the plot. Different leaderboards.

### The cache trap, and the rule that removes it

Switching models mid-conversation **breaks the prompt cache**. The cache is keyed to the
model serving the request, so your next message re-reads the entire conversation at full
input price.

**A session** is one continuous conversation — from launching `hermes` (or `/reset`) until
you reset or quit. Context accumulates the whole time.
**A task** is one unit of work.

> ## Run one task per session. When the task is done, `/reset`.

That one habit fixes three things at once:

| Problem | Why it fixes it |
|---|---|
| Context rot | Never gets a chance to build up |
| Cost | You stop re-sending an irrelevant conversation every turn |
| The cache trap | **Disappears.** At a reset there is no context to re-read, so switching is free |

**But do not reset mid-task.** The website you build in Part 2 is *one task*, even though
it runs for an hour — the agent needs to remember the brief, the layout, and your last
three corrections. Reset **between** things, never **during** one.

`/reset` clears the conversation. **Quit and relaunch** also re-reads config and skills —
required after you install a skill, change an API key, or add a provider.

## Step 3 — Have your agent build three skills

This is the centrepiece. You are not writing these skills — you are describing them.

> **A note on names.** Skills become slash commands automatically, so a skill named after
> a built-in gets shadowed and silently never fires. `/switch` is already taken (it is an
> alias for `/sessions`), and `/model`, `/profile`, `/usage`, `/status`, `/context`,
> `/tools`, `/skills` are all built in. Run `/help` to see the list before naming anything.
> Ours use a `<noun>-<verb>` scheme.

Paste this whole brief into Hermes:

```
Build me three skills. Ask me anything ambiguous first, then write all three, tell me
where you put them, and run each one once.

────────────────────────────────────────────────────────────
SKILL 1 — `models-research`
Researches the current AI model landscape and writes me a briefing.

CADENCE: WEEKLY, not daily. The model landscape does not move fast enough to justify a
full research pass every morning. Run it once a week, plus any time I explicitly ask.

RUNS WHEN: I ask what model to use, ask about model costs, ask what's new in models, or
when my weekly briefing is due (7+ days since the last file in the briefings folder). If
the most recent briefing is under 7 days old and I haven't asked for a fresh one, say so,
show me that briefing's TL;DR, and do NOT re-research.

COVERS six categories: text/chat, text-to-image, image-to-video, text-to-speech,
speech-to-text, music generation. Plus image editing.

SOURCES — fetch live every time. Never answer from memory, never reuse yesterday's
numbers:
  - https://openrouter.ai/api/v1/models  (no API key needed). Returns id, name,
    context_length, architecture.output_modalities, and pricing.prompt /
    pricing.completion in USD PER TOKEN — multiply by 1,000,000 for the per-million
    figure people quote.
  - https://artificialanalysis.ai/image/leaderboard/text-to-image
  - https://artificialanalysis.ai/video/leaderboard/image-to-video
  - https://artificialanalysis.ai/text-to-speech/arena
  - Image editing, speech-to-text and music: web search, prefer sources from the last
    60 days, state the date of each source.

FOR TEXT MODELS give four picks: cheapest usable (context >= 100k), best value, frontier,
and any genuinely free models. Also flag the best model for AGENTIC CODING specifically —
that is a different ranking from general intelligence.

OUTPUT to 2-Areas/AI-Models/YYYY-MM-DD-model-briefing.md:
  - TL;DR, max 10 lines — one line per category, model + rough cost
  - A table per category: model, provider, price, limits, one-line why
  - "What changed" vs the most recent previous briefing. If nothing changed, say so in
    one line. Don't pad.
  - Suggested fast / smart / coding aliases
  - Every source URL with the date fetched

RULES: if you can't verify it, say so — never guess a price. Never mix units in one table.
Flag any price move over 30% as PRICE ALERT.

────────────────────────────────────────────────────────────
SKILL 2 — `model-switch`
Changes which model I'm using.

Four profiles:
  private — Morpheus. Client, financial, or personal work. Never leaves privacy.
  fast    — cheap OpenRouter model. Bulk work, summarising, tidying, first drafts.
  smart   — frontier OpenRouter model. Judgment calls, client-facing writing.
  coding  — the best AGENTIC CODING model. Building software, multi-file projects,
            debugging. I want the model that runs tools, reads its own errors and
            manages a whole project — not the one that writes the prettiest function.

RUNS WHEN: I say switch to fast/smart/coding/private, ask what model I'm on, or ask which
model I should use for a task.

DOES:
  - `/model-switch <profile>` switches using Hermes's native /model command and my
    configured model_aliases. Check the Hermes docs for exact syntax first.
  - With no argument: tell me what I'm on and suggest a profile based on what we're
    working on right now.
  - If I'm switching mid-task, WARN ME that it breaks the prompt cache and the next
    message re-reads the whole conversation at full input cost. Suggest waiting for a
    task boundary.
  - If I'm switching AWAY from private while we're working on anything that looks like
    client data, financials or personal notes — STOP and make me confirm.
  - Keep fast/smart/coding in sync with the latest briefing in 2-Areas/AI-Models/. The
    coding pick must come from the briefing's AGENTIC CODING section, not its general
    ranking.

────────────────────────────────────────────────────────────
SKILL 3 — `spend-report`
Tracks what my AI usage costs. INCREMENTAL — never redoes work it has already done.

CADENCE: WEEKLY, plus on demand whenever I ask.

THE CORE RULE — only ever process what's new:
  Keep a watermark in 2-Areas/AI-Costs/.spend-state.json:
      { "last_run": "<ISO timestamp>", "last_covered_through": "<ISO timestamp>",
        "totals_to_date": { "tokens_in": N, "tokens_out": N, "cost_usd": N } }

  On every run:
    1. Read the watermark.
    2. Query `hermes insights` ONLY for the period since last_covered_through. Never
       re-query a period already recorded.
    3. Append the new records to the monthly log.
    4. Update the watermark and running totals.
    5. Report the NEW period. For lifetime or month-to-date figures, read my own stored
       log files — do not re-query the source.

EDGE CASES:
  - No state file (first ever run): pull the full available history, write the log, set
    the watermark. Say clearly that this was a first full import.
  - Nothing new since last run: say exactly that in one line with the last run timestamp.
    Do NOT re-report the previous period and do NOT invent analysis.
  - State file missing, corrupt or future-dated: fall back to the latest entry in the
    monthly logs, say you did so, and continue.
  - If a run fails partway, do not advance the watermark.

DOES:
  - Use Hermes's built-in `hermes insights`. Check the docs for exact flags and how to
    bound it to a date range.
  - Report the new period, month to date, and the trend versus the previous period.
  - Break the new period down by project and by model.
  - Flag any session over 2x my recent average as a SPIKE and say what caused it.
  - Run `hermes prompt-size` and tell me what my context files cost per message.
  - Give me exactly ONE concrete change to cut cost. If the current briefing has a
    cheaper model that would do, name it.
  - Append to 2-Areas/AI-Costs/YYYY-MM-token-log.md, one file per month.

RULES: estimates are estimates — say so, never present them as an invoice. If I've spent
nothing, say that in one line. Don't manufacture analysis.
```

When it finishes, **open the generated `SKILL.md` files** in VS Code and read the
description line of each.

> That line is the most important part of the whole skill. It is the only part the agent
> sees when deciding whether to run. A perfect skill with a vague description never fires.
> **That is what you review** — not the code.

Now run them:

```
/models-research
/spend-report
```

Open the dated briefing in Obsidian. You now have a research analyst covering the entire
AI industry, and it costs you nothing.

## Step 4 — SOUL.md and HERMES.md

Two files, two jobs. People conflate them and end up with one bloated file loaded into
every session they ever run.

| File | Scope | Holds |
|---|---|---|
| `~/.hermes/SOUL.md` | **Global** — everywhere | Who your agent **is**: persona, voice, base behaviour |
| `HERMES.md` | **This project only** | How **this job** works: stack, conventions, where things live |

Project file priority, first match wins: `.hermes.md` → `AGENTS.md` → `CLAUDE.md` →
`.cursorrules`. `SOUL.md` loads independently on top.

> **The rule:** if it is true everywhere, `SOUL.md`. If it is true for this job,
> `HERMES.md`.

### Measure what they cost

```bash
hermes prompt-size
```

Note the number. Add a hundred lines to `SOUL.md`. Run it again. The number goes up, and
**you pay that on every single message.** That is why the 200-line rule exists — it is not
style advice, it is a bill.

### Write yours by asking

```
Look at how I've been talking to you, the notes in my second brain, and the writing in my
Projects folder. Draft a SOUL.md that captures how I actually work and how I want you to
behave.

Include: how to talk to me, my writing voice with real examples pulled from my own notes,
my defaults for filing and naming, and explicit permission to tell me when I'm wrong.

Keep it under 150 lines. Ask me about anything you're unsure of rather than inventing it.
Then show me `hermes prompt-size` before and after.
```

Two things make it good: **real writing samples** pulled from your own notes, and
**explicit permission to be uncertain** — "say I don't know rather than guess" is the
single highest-value line a beginner can add.

## Step 5 — Wire it into your standup, weekly

```
Update my daily-standup skill so it knows about the two new skills — but WEEKLY, not every
day. I don't want a full research pass every morning.

At the start of the standup:
  1. Check when models-research last ran (newest file in 2-Areas/AI-Models/) and when
     spend-report last ran (last_run in 2-Areas/AI-Costs/.spend-state.json).
  2. If either is 7+ days old, say so in ONE line and ASK me whether to run it now.
     Don't run it without asking.
  3. If both are current, say nothing at all about them.
  4. If the last briefing is current AND recommends better fast/smart/coding models than
     my aliases, mention that in one line.

Then continue with the normal standup.
```

Run `/daily-standup`. Notice what it does when nothing is due: **nothing.** A skill that
reports "no change" every morning trains you to stop reading it.

---

# PART 2 — Vibe Coding a Pro Website

We build for a **fence installation company**. Swap in roofing, landscaping, pools,
decking — the process is identical.

## Step 1 — Install the engineering skills

```
Install the Superpowers skills for software development: brainstorming, writing-plans,
systematic-debugging, and verification-before-completion. They're at
github.com/obra/superpowers and Hermes can install skills from a raw SKILL.md URL. Also
install a frontend-design skill for production-grade UI.

Install them, list what you installed, and confirm each one loads.
```

> **Why a process beats a better prompt.** A better prompt makes one answer better. A
> process makes every answer better. Left alone, an agent building a website starts
> writing code in thirty seconds and **builds the wrong thing, beautifully.** These skills
> force it to understand, plan, get your approval, build, then prove it works. That is
> what $5,000 buys. The code was never the expensive part.

## Step 2 — Set the goals

```bash
mkdir -p ~/SecondBrain/1-Projects/fence-co-website
cd ~/SecondBrain/1-Projects/fence-co-website
hermes
```

```
We're building a website for a fence installation company. Before anything else, help me
write goals.md.

Ask me: what does this site need to ACHIEVE for the business? How will we know it worked?
What's the one action a visitor should take? What would make this a failure even if it
looks beautiful?

Push back if my goals are vague or unmeasurable.
```

For the fence company: **quote requests.** Ten a month, up from two. The one action is
tapping "Get a Free Quote" or the phone number. The failure mode is a beautiful site
nobody contacts.

> Write it down, because in twenty minutes the agent will offer you a gorgeous animation
> that pushes the quote button below the fold, and **`goals.md` is what lets you say no.**
> Goals are how a non-engineer wins an argument with an AI.

## Step 3 — Build the knowledge base

### The business

```
Use the brainstorming skill. Interview me as the fence company owner — ask what you need
to design and build a site that gets quote requests. One question at a time. Save what you
learn to client-brief.md as we go.
```

Have real answers ready: services, service area, who the customers are, what is wrong with
the current site, what makes them different, what objections they hear, and eight to ten
real town names for local SEO.

### The images

Images are knowledge base too.

```bash
mkdir -p assets/photos
# put 15-20 job photos in here
```

```
Look at every image in assets/photos. For each one write a line in
assets/image-inventory.md: what it shows, what fence type, whether it's a good hero
candidate, and alt text for accessibility and SEO.

Then tell me what's MISSING — what photos would this business need to shoot to make this
site convert better?
```

The agent can see. It writes the alt text — real SEO work agencies charge for — and the
gap analysis is a genuine consulting deliverable: *here is the shot list for your
photographer.* That is your second invoice.

## Step 4 — Layout and functionality

Describe the site out loud. This is your step, and the one most people skip.

```
I'm going to describe the site page by page. Turn it into layout.md — sections in order,
what's in each, what the visitor should do next, and how it changes between mobile and
desktop. Cross-check everything against goals.md and tell me if anything I describe works
against the goal.

[Describe your homepage top to bottom: hero, trust strip, service cards, gallery,
why-us, quote form, footer. Say what is sticky on mobile.]
```

Then the functionality, explicitly:

```
Now write the functionality spec into layout.md. For each interactive thing: what the user
does, what happens, what happens when it fails, and what the business owner sees.
Cover the quote form, photo upload, any slider, the sticky mobile button, and click-to-call.
```

> Anyone can ask an AI for a website. Almost nobody can say what should be on it. **That
> is what clients pay for**, and it is the part the agent cannot do — it does not know the
> fence business or this owner. You do.

## Step 5 — Architecture, stack, database

```
Read goals.md, client-brief.md, image-inventory.md and layout.md. Recommend the
architecture and stack. Write it to architecture.md.

Constraints: I'm not a programmer and I maintain this or hand it to the client. Fast on
mobile. Must rank in local search. Quote form must email the owner reliably with the photo
attached. Hosting free or near-free.

Give me ONE recommendation, not a menu. For each piece: what it is in one sentence, why it
beats the obvious alternative here, and what it costs.

Then: does this site actually need a database? Make the case both ways and recommend.
```

Then the move that matters most:

```
Now argue against your own recommendation. What would a senior engineer say is wrong with
this for this specific client? What's the simplest thing that would work, and am I
over-building?
```

> Models tend to agree with your framing. Asking for the counter-case is how a non-expert
> gets a second opinion. Best fifteen seconds in the session.

On the database, the answer is probably no — and that is worth more to you than a database
would be. Every piece of tech you add is a thing that breaks, costs money, and needs
maintaining. **The senior move is removing things, not adding them.**

## Step 6 — The plan

```
Use the writing-plans skill. Based on goals.md, client-brief.md, layout.md and
architecture.md, write plan.md — steps I can approve one at a time. Flag anything you're
unsure about instead of assuming.

Also write a HERMES.md for this folder so any future session knows the client, the stack,
where the brief and layout live, and the design direction. Keep it short.
```

**Read the plan. Then change it.**

> Editing a plan takes ten seconds. Editing a built site takes ten minutes. This is where
> non-programmers get their power back — you cannot review the code, but you can
> absolutely review the plan.

## Step 7 — Build

```
/model-switch coding
```

You are at a task boundary with a fresh context, so this switch costs nothing.

```
Use the frontend-design skill. Execute plan.md.

Design direction from client-brief.md: established, trustworthy, premium. Not cheap, not
corporate. Their competitors all look identical — we don't.

Use the real photos in assets/photos with the alt text from image-inventory.md.

Non-negotiable:
  - Phone number as real tappable text, never inside an image
  - Sticky quote button on mobile
  - Warranty badge visible without scrolling
  - Town names in headings, LocalBusiness schema
  - Works at 375px wide
```

The first result will not be right. That is normal — iterate:

```
The hero looks like an insurance company. Warmer, more craftsman. Use the cedar tones
from the brief.
```

## Step 8 — Verify

```
Use the verification-before-completion skill. Prove this works:
  - Every link goes somewhere real
  - Quote form submits and the email actually arrives
  - Photo upload works from a phone
  - Works at 375px wide
  - Lighthouse performance and accessibility
Show me evidence for each, not assurances.
```

Then **check it yourself** at phone width. Your eyes on it before the client's.

> The agent said "done" twenty minutes ago. This is how you find out whether that was
> true. Agents are optimistic. Make them prove it.

## Step 9 — Git, deploy, live

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

Three things to take away:

- **Git is the undo button for your whole project.** Not a programmer thing — a not-losing-
  your-work thing.
- **The secrets check is not optional.** Push a key to a public repository and it is
  scraped within minutes. If it happens, rotate the key immediately.
- **Git push to live site** is the loop that runs the modern web. Change a file, push, live
  in thirty seconds.

## Step 10 — What it is worth

| | |
|---|---|
| Agency quote | **$4,000–8,000, 6–8 weeks** |
| What it cost you | **About an hour and a few cents of tokens** |
| Hosting | **$0** on the free tier |
| What to charge | Not "cents plus margin." You sell the **outcome** — more quote requests |

**The honest part.** What you built is an excellent starting point, not a finished client
delivery. Real client work adds their photos, their copy, their domain, revisions and
support.

The hour you saved was the hour of *building* — which was never the valuable hour. **The
valuable hour was Steps 2, 3 and 4**, where you worked out what to build. The agent could
not do that part. It does not know the business, and it does not know the owner.

That is the job you are selling.

---

## Homework

1. Run `/daily-standup` every morning for a week. Run `/models-research` and
   `/spend-report` **once** that week. Report whether weekly felt right.
2. **Practise one task per session** — `/reset` between tasks for a week. Report what
   changed: quality, cost, or both.
3. Get `SOUL.md` good, then **cut it under 150 lines.** Report `hermes prompt-size` before
   and after. The cut is the assignment.
4. Use `/model --once` at least three times. Note when the expensive model was worth it.
5. **Rebuild the site for a real local business in your town.** Goals and discovery
   properly, even if you invent the answers.
6. Get **Lighthouse mobile performance above 90.** This is the hard part.
7. **Deploy it to a real live URL.**
8. Write a one-page proposal: what is wrong with their current site, what you would build,
   what you would charge. *Optional, high value: actually send it.*

Then take the [quiz](./quiz.md) and bring your open-ended answers to the peer lab.

---

## Troubleshooting

**A skill I built never fires.** Its name probably collides with a built-in. Run `/help`
and check. `/switch`, `/model`, `/profile`, `/usage`, `/status`, `/context`, `/tools`,
`/skills` are all taken. Rename it and restart.

**My agent got noticeably worse mid-session.** Context rot. `/reset` and start the task
again in a clean session.

**A new alias is not being picked up.** Config changes beyond context length, compression
and message limits need a `/reset` or a full restart.

**The agent cannot reach a URL.** Some sites block automated fetches. Ask it to say which
source failed and continue with the rest rather than guessing.

**Superpowers will not install.** Install the individual skills directly from their raw
`SKILL.md` URLs on GitHub, or run the build with plain prompting — you lose the process
guardrails but not the session.

**The build is going wrong and I cannot tell why.** Stop. Re-read `plan.md` and `goals.md`
aloud. Nine times out of ten the plan was fine and the agent drifted from it. Say so
directly and point it back at the file.

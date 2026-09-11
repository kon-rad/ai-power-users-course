# Module 4: Student Guide

**Video Clips and Shorts**
Follow along here. Every step, every command, every brief you can copy and paste.

By the end you will have a skill your agent wrote, a `fence-co` caption and title preset,
and one captioned vertical short assembled inside a real ArgoCut project you own.

**This module is run on macOS.** Every command below is the Mac one.

---

## Before the session

### You need Module 3's project

The fence company site, deployed, with `design-brief.md` committed. If you missed it, the
[Module 3 student guide](../module-03-media-models-and-your-own-domain/student-guide.md)
takes about 100 minutes.

```bash
cd ~/SecondBrain/1-Projects/fence-co-website
git status
ls design-brief.md
```

### Build and run ArgoCut once, tonight, not live

A Rust and WASM build is too slow to do on air. Do this before the session.

```bash
git clone https://github.com/kon-rad/argocut.git ~/ArgoCut
cd ~/ArgoCut
cp apps/web/.env.example apps/web/.env.local
```

Open `apps/web/.env.local` and change one line:

```
NEXT_PUBLIC_ARGOCUT_AGENT_API=1
```

Then:

```bash
docker compose up -d db redis serverless-redis-http
cd apps/web && bun run db:migrate
cd ~/ArgoCut && bun install
bun dev:web
```

Open `http://localhost:3000` in Chrome. It should load with no console errors. Leave the
server running, or confirm you can restart it with `bun dev:web` from `~/ArgoCut` tomorrow.

### Install Docker Desktop and the Claude in Chrome extension

| Tool | Why | Cost |
|---|---|---|
| [Docker Desktop](https://www.docker.com/products/docker-desktop/) | ArgoCut's projects and brands live in Postgres | 0 USD |
| Claude in Chrome | Lets your agent see and act inside the tab you are already looking at | 0 USD |

> The exact install link for Claude in Chrome was not confirmed for this guide. Search the
> Chrome Web Store for "Claude in Chrome" and confirm it is the official Anthropic listing
> before installing.

Also confirm ffmpeg from Module 3: `ffmpeg -version`. If missing, `brew install ffmpeg`.

### Bring

- **One raw phone video** of a finished or near finished fence job. A walkthrough or a
  testimonial. Shot 16 by 9, 1080p or better on the shorter side of the frame.

---

## Step 1: Confirm the bench

```bash
cd ~/ArgoCut
curl -s localhost:3000/api/health
```

If nothing answers:

```bash
docker compose up -d db redis serverless-redis-http
bun dev:web
```

Open `http://localhost:3000` in Chrome. Open devtools, and in the console:

```js
window.__argocutAgent?.isReady()
```

> **The trap.** `NEXT_PUBLIC_ARGOCUT_AGENT_API` is baked into the JS bundle at build time.
> A tab open before the server picked up the flag runs stale JS and shows `undefined`
> forever, no matter how many times you reload the flag file. Restart `bun dev:web`, then
> hard refresh the tab.

---

## Step 2: Wire up the tab

Tell your agent:

```
Load select:mcp__claude-in-chrome__tabs_context_mcp,mcp__claude-in-chrome__navigate,mcp__claude-in-chrome__javascript_tool,mcp__claude-in-chrome__file_upload,mcp__claude-in-chrome__tabs_create_mcp

Then find my ArgoCut tab and confirm window.__argocutAgent.isReady() through
javascript_tool. Tell me the tab id you are using.
```

> **The trap.** This is always your own already open tab. If your agent proposes a
> headless or off screen browser, stop it. That is a different tool for a different job.

---

## Step 3: Build `/job-reel`

Paste this whole block. It is long. That is the work.

```
Build me a skill called job-reel.

INPUT: one raw video file of a finished or near finished fence job, a walkthrough or a
testimonial, shot on a phone.

OUTPUT: one vertical 1080x1920 clip on the timeline of an ArgoCut project already open in
my browser tab, captioned in the fence-co style, with a title card, ready for me to review
and export by hand. This skill never exports. Rendering stays my decision, not the agent's.

CADENCE: on demand, whenever I hand it a job video.

TOOLS THIS SKILL USES, all already on this machine:
  - python3 ~/.claude/skills/argocut-edit/scripts/probe_clips.py
  - the caption-video skill's caption_video.py, with --srt-only
  - python3 ~/.claude/skills/argocut-edit/scripts/srt_to_ops.py
  - python3 ~/.claude/skills/argocut-edit/scripts/apply_preset.py
  - window.__argocutAgent, driven through the claude-in-chrome extension, in MY already
    open tab. Never a separate browser, never a dedicated profile.

PIPELINE, in order:

1. PROBE. Run probe_clips.py on the source video. Read duration, fps, resolution, and
   whether it has audio before doing anything else. If the shorter side of the frame is
   under 1080px, say so and stop. It cannot be reframed to 1080x1920 without going soft.

2. TRANSCRIBE, once. Run caption_video.py on the source video with --srt-only. This writes
   an .srt, a full transcript markdown file, and a .words.json sidecar next to the video.
   Every later step reads these files. Never re-run whisper on the same video.

3. PLAN THE CUT. Read the full transcript. Find the strongest 15 to 45 second moment using
   this rubric:
     - Opens with zero setup: a claim, a number, or the state of the job in this shot.
     - Develops one idea. Not a summary of the whole video.
     - Lands on a finished thought, not a trail off into the next topic.
     - Boundaries are sentence indices from the transcript, never raw seconds, so the clip
       can never start or end mid thought.
   Write edit-plan.md next to the source video: the chosen sentence range, the exact start
   and end timestamp in the SOURCE video, and one line on why this moment earns the clip.
   Show me the plan. Do not touch the browser until I approve it.

4. ASSEMBLE, once I approve the plan.
   a. Load the claude-in-chrome tools this needs in one ToolSearch call. Confirm
      window.__argocutAgent.isReady() is true in my open ArgoCut tab. If it is not, stop
      and tell me. Do not guess and do not open a different tab or browser.
   b. If I already have a project open, use it. Otherwise call createProject with a name
      built from today's date and the job, then call configureProject with
      canvasSize: { width: 1080, height: 1920 } so the project is vertical from the start.
   c. Stage the source video onto the hidden file input, call file_upload, then call
      importMedia. Read the result for this one file. If it was skipped, stop and tell me
      why. Never build a timeline against a mediaId you did not get back.
   d. Build ONE apply_edits batch with a single add_clip op. trimStart is the plan's start
      timestamp. trimEnd is the SOURCE video's total duration minus the plan's end
      timestamp, not the end timestamp itself. Name the clip after the job.
   e. Send that batch through applyEdits. If any op is refused, read the failing index and
      the reason, fix that one op, and resend the whole batch. Never apply ops one at a
      time.

5. CAPTION AND TITLE, as a second batch, once the clip exists.
   a. Run srt_to_ops.py against the .srt from step 2, with --timeline-start 0,
      --trim-start set to the plan's start timestamp, and --span set to the clip's own
      duration. This times the captions to what is actually on the timeline, not to the
      full source video.
   b. Call findElements for the text elements that call just created. Run apply_preset.py
      against that list with --presets-file pointing at the project's own
      argocut-brand-presets.json, never the skill's shared copy, --brand fence-co,
      --preset caption. If the fence-co entry does not exist yet, stop and tell me to add
      it first. Do not invent colours or a font.
   c. Add one add_text title card at the start of the clip, two to three seconds, styled
      with the fence-co statement preset from the same file, naming the job.
   d. Send that batch through applyEdits.

6. SAVE. Call save. Report the clip's length, the sentence range it came from, and anything
   you were not sure about, so I can check it myself in the tab that is already open.

RULES:
  - Never export. There is no export tool here, on purpose, and this skill does not add one.
  - Never guess a mediaId, an elementId, or a preset value. Read each one back from the
    tool that created it.
  - A refused apply_edits call rolls back the whole batch. Read the reason before you
    retry. Never resend blind.
  - If ArgoCut is unreachable or window.__argocutAgent is undefined, stop and say so.
```

Answer its questions. When it is done, open `SKILL.md` and read the description line.

---

## Step 4: Build the `fence-co` preset

```bash
mkdir -p ~/SecondBrain/1-Projects/fence-co-website/argocut
cp ~/.claude/skills/argocut-edit/assets/brand-presets.json \
   ~/SecondBrain/1-Projects/fence-co-website/argocut/argocut-brand-presets.json
```

```
Open argocut/argocut-brand-presets.json and design-brief.md. Add a new top level entry
called fence-co, shaped exactly like the existing argo entry: a caption block and a
statement block. Take the colour set, and the type choice if one is stated, from
design-brief.md, not from your own judgement. Show me the new entry before saving.
```

> **The trap.** `fontSize` in this file is not pixels. It renders as
> `fontSize * (canvasHeight / 90)`. A caption needs roughly `3.5` to `4.3`, not `38`.
> `references/text.md` in the argocut-edit skill has the full conversion.

---

## Step 5: Probe and transcribe your real footage

```bash
mkdir -p ~/SecondBrain/1-Projects/fence-co-website/footage
open ~/SecondBrain/1-Projects/fence-co-website/footage
```

Drag your job video in.

```
Run job-reel on footage/<your-job-video>.mp4. Stop after the probe and the transcript. Do
not plan the cut yet, I want to see the raw numbers first.
```

> **The trap.** If the shorter side of the frame is under 1080px, the skill stops. There is
> no fallback photograph the way Module 3 had one. If this happens, use a different clip
> from your phone shot at full resolution, not the one you brought.

---

## Step 6: Plan the cut

```
Continue. Plan the cut and show me edit-plan.md. Do not touch the browser yet.
```

Read it yourself against three questions before you approve it:

1. Does the opening sentence need zero setup?
2. Does it develop one idea, not several?
3. Does the last sentence land, or trail off?

---

## Step 7: Assemble it, live, in your own tab

```
Approved. Assemble it.
```

Watch the browser tab, not the terminal. The project, the clip, the captions and the title
card all appear in the tab you have had open the whole time.

> **The trap.** A refused op names its failing index and its reason. The most common
> failure is a trim that does not match the source's actual duration. Read the reason, fix
> that one field, and let the agent resend the whole batch. Do not ask it to patch the
> timeline by hand from here.

---

## Step 8: Review

Open the timeline. Play the clip. Check the caption sits over the footage and does not run
off either edge.

You now have a skill that turns any future job video into a captioned vertical short, and a
`fence-co` preset every later ArgoCut project can reuse.

**Export is a manual step**, done in the ArgoCut UI, whenever you decide the clip is ready.
Nothing in this module does it for you.

---

## If something breaks

| Symptom | Cause and fix |
|---|---|
| `window.__argocutAgent` is `undefined` | The tab predates the server picking up the flag. Restart `bun dev:web` with `NEXT_PUBLIC_ARGOCUT_AGENT_API=1` set, then hard refresh the tab |
| `docker compose up` fails on port 5432 | Something else on the machine already holds it, often a host Postgres install. Set `ARGOCUT_DB_PORT=5434` in a root `.env` and point `DATABASE_URL` at the same port |
| The skill stops at the probe step | Source resolution is under 1080 on its shorter side. Use different footage, shot at full resolution |
| `apply_edits` refuses with an out of range trim | The plan's timestamps do not match the source video's real duration. Recheck `probe_clips.py`'s output against `edit-plan.md` |
| The caption text is a full screen headline, not a subtitle | `fontSize` was set in pixels instead of the scaled unit. Recheck the `fence-co` entry against `references/text.md` |
| Import silently returns fewer media than you staged | Read the per file result from `importMedia`. A skipped file names its reason, usually an unsupported codec or a storage quota |

---

## Homework

1. Point `/job-reel` at a second job video. Bring `edit-plan.md`.
2. Export the clip by hand, or decide not to, and write one sentence on why.
3. Feed the skill a video with no clean hook, on purpose. Report what it did.
4. Run `describeParams` on the title card element and change one param yourself, directly
   in the timeline.

---

## What this cost

| Item | Amount |
|---|---|
| ArgoCut, Docker, Postgres | 0 USD, runs on your own machine |
| Transcription | 0 USD, local whisper through the caption-video skill |
| Everything else | 0 USD |

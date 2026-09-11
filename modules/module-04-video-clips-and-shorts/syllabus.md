# Module 4: Video Clips and Shorts

**Status:** syllabus draft v1, planning phase.
**Date:** 2026-08-31.
**Platform:** macOS. Every path and command below is the Mac one.
**Prereq:** Module 3 complete. The fence company site deployed, `design-brief.md` in the
repo. ArgoCut cloned, migrated, and confirmed running locally at least once before today.
**Companions:** `student-guide.md` · `agenda.md` · `quiz.md` · `luma-description.md`

---

## Format

**One session, 60 minutes, no break, live on YouTube.**

| Part | Time | Theme |
|---|---|---|
| **Part 1** | 0:00-0:26 | Your agent gets an editing bench. ArgoCut live, the tab wired up, `/job-reel` built |
| **Part 2** | 0:26-1:00 | Use it. A real job video, a cut plan, one atomic assembly, review |

**The spine:** Module 3's agent generated media. Today's agent edits media that already
exists, driven live inside a browser tab instead of called through an API.

**What is different about this module:** the first module that costs 0 USD. ArgoCut,
whisper transcription, and the browser tab are all local and free.

---

## Learning objectives

By the end, a student can:

1. Start ArgoCut with the agent API flag on and confirm `window.__argocutAgent` is live
2. Drive a web app's agent API through their own already open browser tab
3. Plan a cut from a transcript, on sentence boundaries, before touching a timeline
4. Batch a whole assembly into one `apply_edits` call and read a refusal by index
5. Apply a brand's caption and title preset to timeline text without hand setting keys
6. Say why export stays a manual step in this tool

---

# PART 1: Your agent gets an editing bench (26 min)

## 0:00-0:02 · Open (2 min)

State the artifact: one captioned vertical short, assembled live, inside the project
already open in the browser. State the cost: 0 USD required for this session.

---

## 0:02-0:07 · Confirm the bench (5 min)

ArgoCut was built and run once before today, so this segment checks it, it does not build
it.

```bash
cd ~/ArgoCut
curl -s localhost:3000/api/health
```

If nothing answers:

```bash
docker compose up -d db redis serverless-redis-http
bun dev:web
```

Open `http://localhost:3000` in Chrome. In the browser's own devtools console:

```js
window.__argocutAgent?.isReady()
```

`true` means the flag is baked into this tab's bundle. `undefined` means the tab was open
before the server picked up `NEXT_PUBLIC_ARGOCUT_AGENT_API=1`, or the flag is not set.
Hard refresh the tab, or navigate it again, and recheck.

### Watch for

`NEXT_PUBLIC_ARGOCUT_AGENT_API` is a `NEXT_PUBLIC_` variable. It is baked into the JS
bundle at build time, not read at request time. A tab opened before the flag was set is
running stale JS no refresh of the flag alone will fix. Restart `bun dev:web`, then hard
refresh the tab.

---

## 0:07-0:11 · Wire up the tab (4 min)

Load the browser tools:

```
ToolSearch: select:mcp__claude-in-chrome__tabs_context_mcp,mcp__claude-in-chrome__navigate,mcp__claude-in-chrome__javascript_tool,mcp__claude-in-chrome__file_upload,mcp__claude-in-chrome__tabs_create_mcp
```

```
Find my ArgoCut tab and confirm window.__argocutAgent.isReady() through javascript_tool.
Tell me the tab id you are using.
```

### Watch for

This is never a second, agent only browser. If no ArgoCut tab is open, the agent navigates
a tab in your own Chrome, in front of you, or you open one yourself and hand it the tab id.

---

## 0:11-0:26 · BUILD BY ASKING: `/job-reel` (15 min)

Paste this whole block.

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

### Watch for

The skill name is not close to any built in command. No rename needed.

---

# PART 2: Use it (34 min)

## 0:26-0:33 · The fence-co preset (7 min)

ArgoCut's caption and title styling comes from a presets file, keyed by brand. The skill's
own copy at `~/.claude/skills/argocut-edit/assets/brand-presets.json` holds Konrad's
personal and Argo brands. A client's colours do not belong in that shared file.

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

### Watch for

`fontSize` in this file is not pixels. `references/text.md` in the argocut-edit skill has
the conversion. A caption sized for 1080p needs a `fontSize` of roughly 3.5 to 4.3, not 38.

---

## 0:33-0:40 · Probe and transcribe (7 min)

```bash
mkdir -p ~/SecondBrain/1-Projects/fence-co-website/footage
open ~/SecondBrain/1-Projects/fence-co-website/footage
```

Drag the job video in.

```
Run job-reel on footage/<the-job-video>.mp4. Stop after the probe and the transcript. Do
not plan the cut yet, I want to see the raw numbers first.
```

### Watch for

A source under 1080 on its shorter side gets flagged and the skill stops. There is no
photograph fallback here, the way Module 3 had one. Bring real footage shot properly, or
this segment has nothing to work with.

---

## 0:40-0:48 · Plan the cut (8 min)

```
Continue. Plan the cut and show me edit-plan.md. Do not touch the browser yet.
```

Read it against the rubric out loud: does the opening line need zero setup, does it
develop one idea, does the last line land.

### Watch for

If the transcript has no clean hook anywhere, the plan should say so rather than force a
weak candidate. That is the correct failure, not a bug.

---

## 0:48-0:57 · Assemble live (9 min)

```
Approved. Assemble it.
```

Watch the tab, not the terminal. The project appears, the clip lands, the captions and the
title card apply, in the browser the whole time.

### Watch for

A refused op names its index in the batch. Read the reason before resending. The most
common one is an out of range trim, from a plan timestamp that does not match the source's
actual duration.

---

## 0:57-1:00 · Review and close (3 min)

Open the timeline. Play the clip. Check the caption reads over the footage, not off the
edge of the frame.

State what is now permanent: one skill that turns any future job video into a captioned
vertical short, and a `fence-co` preset any later ArgoCut work in this project reuses.

State the boundary: export is a manual step, done in the ArgoCut UI, not by the skill.

---

# Homework

1. Point `/job-reel` at a second job video and bring `edit-plan.md`.
2. Export the clip by hand from the ArgoCut UI, or decide not to, and write one sentence on
   why.
3. Feed the skill a video with no clean hook, on purpose, and report what it did.
4. Run `describeParams` on the title card element and change one param yourself, directly
   in the timeline.

# Peer quiz

Summary only, detail in `quiz.md`: what `window.__argocutAgent` requires to exist on a
page · why `trimEnd` is not the same number as the out point · what happens when an
`apply_edits` op is refused · why the fence-co preset lives in the project, not in the
shared skill folder.

# Prep checklist

## Must verify before the stream

- [ ] ArgoCut boots clean from the documented setup on a machine that has never run it:
      clone, `docker compose up -d db redis serverless-redis-http`,
      `bun run db:migrate`, `bun install`, `bun dev:web`
- [ ] `NEXT_PUBLIC_ARGOCUT_AGENT_API=1` in `.env.local` produces a live
      `window.__argocutAgent` after a hard refresh, timed
- [ ] `probe_clips.py`, `caption_video.py --srt-only`, `srt_to_ops.py`, and
      `apply_preset.py` all run clean against one real test clip end to end
- [ ] The exact Chrome Web Store URL for the Claude in Chrome extension. Not verified as of
      2026-08-31, do not publish a link until checked
- [ ] The current `argo` entry in `brand-presets.json` still matches the shape a `fence-co`
      entry should copy

## Assets

- [ ] A pre transcribed, pre planned backup job video, in case live transcription or the
      cut plan stalls on air
- [ ] The Module 3 `design-brief.md` hex values on screen, ready to paste into the preset
      brief
- [ ] Docker Desktop confirmed running before the stream starts, not checked live

# Open questions

1. **Does every student's machine actually run Postgres in Docker cleanly during a live
   session, or does this whole module have to become a recording of one working machine
   plus a shared watch along?** ArgoCut's storage moved from browser only IndexedDB to a
   Postgres backed server between when `argocut-edit` was written and today. Recommend a
   dry run with two or three real student machines before this airs.
2. **Should `/job-reel` also generate a second, longer cut for the fence company's own
   YouTube channel, not just the short?** Recommend no for this module. One rubric, one
   output, done properly, beats two done thinly. A longer form pass is a later module's
   job if the course goes there.
3. **Is 15 to 45 seconds the right window for a construction trade clip, versus the 30 to
   90 second window the podcast clipping research used?** A finished fence job walkthrough
   has less to say than an interview. Recommend confirming against one real test clip
   before the stream, not assuming the number transfers.

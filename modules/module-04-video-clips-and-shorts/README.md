# Module 4: Video Clips and Shorts

**Module 4 of AI Power Users · 60 minutes · live on YouTube · run on macOS**
**From Argo, myargoquest.com**

Module 3 gave the agent a camera: generated media, a hero video from one photograph. This
module gives it an editing bench: real footage, not generated. Same fence company. A raw
phone video of a finished job becomes one captioned vertical short, assembled live inside
ArgoCut, in the browser tab you are already looking at.

## What you build

```
PART 1: your agent gets an editing bench
    ArgoCut running locally, agent flag on  ──  claude-in-chrome wired to the tab
                                            ──  BUILD BY ASKING: /job-reel
                                            ──  fence-co caption and title preset

PART 2: use it
    raw job footage  ──  probe + transcript  ──  cut plan, sentence indices, shown first
                                            │
                          /job-reel drives ArgoCut, one atomic batch, in your open tab
                                            │
                    captioned vertical short, sitting in the project you are looking at
```

## Learning objectives

By the end of this module you can:

1. Start ArgoCut with the agent API flag on and confirm `window.__argocutAgent` is live
2. Drive a web app's agent API through your own already open browser tab, not a separate
   agent browser
3. Plan a cut from a transcript, on sentence boundaries, before touching a timeline
4. Batch a whole assembly into one `apply_edits` call and read a refusal by index
5. Apply a brand's caption and title preset to timeline text without hand setting keys
6. Say why export stays a manual step and what would have to change for that to move

## The skill your agent writes

| Skill | Cadence | What it does | Why it matters |
|---|---|---|---|
| **`/job-reel`** | On demand | Probes and transcribes a raw job video, plans a cut against a rubric, writes `edit-plan.md`, then drives ArgoCut in your open tab: creates or configures a vertical project, imports the source, assembles the clip in one batch, captions and titles it from the fence-co preset, saves. Never exports | This is the whole module. One brief, one skill, one artifact, built the same way Module 3 built its two skills |

## The 8 steps

| # | Step | Output |
|---|---|---|
| 1 | Confirm the bench | ArgoCut answers `/api/health`, `window.__argocutAgent.isReady()` is true |
| 2 | Wire up the tab | claude-in-chrome tools loaded, the ArgoCut tab found |
| 3 | **Build `/job-reel`** from one brief | The skill, written and readable |
| 4 | The fence-co preset | A `brand-presets.json` copy in the project, one new entry from `design-brief.md` |
| 5 | Probe and transcribe | `clips.json`, `.srt`, `.words.json` for the real job video |
| 6 | **Plan the cut** | `edit-plan.md`, sentence indices, shown before the browser |
| 7 | **Assemble live** | One `apply_edits` batch: clip, caption, title, in the open tab |
| 8 | Review and save | The clip on the timeline, in the project you have been looking at the whole time |

## The ideas that carry the module

**The browser is the runtime.** ArgoCut has no server side project object the agent can
write to directly. It reaches into the same tab you are already looking at.

**One batch, one undo step.** `apply_edits` sends the whole assembly as one call, so Cmd-Z
rolls back the whole edit, not a third of it.

**Cut on the thought, not the sentence.** Boundaries come from sentence indices in a
transcript, never from raw seconds, so a clip cannot start or end mid idea.

**A refusal rolls back and names the index.** A rejected op does not fail silently. It
throws, undoes the batch, and says which op and why.

**Export stays a human decision.** The skill has no export tool by design. An agent can
assemble a client facing video. Whether it ships is still someone's call.

## Files in this module

- [`syllabus.md`](./syllabus.md), the full plan: every segment, every brief
- [`agenda.md`](./agenda.md), the 60 minute run of show and the contingency
- [`student-guide.md`](./student-guide.md), **follow along here**: every step, command and
  brief
- [`quiz.md`](./quiz.md), 5 multiple choice plus 4 open ended, peer evaluated
- [`luma-description.md`](./luma-description.md), event copy
- [`clipping-skill-prompt.md`](./clipping-skill-prompt.md), the research behind the cut
  rubric and the crop maths this module's skill reuses

## Before the session

**You need Module 3's project**, deployed, with `design-brief.md` in the repo. If you
missed it, the
[Module 3 student guide](../module-03-media-models-and-your-own-domain/student-guide.md)
takes about 100 minutes.

**ArgoCut, built and run locally at least once before today.** A Rust and WASM build is too
slow to do live.

```bash
git clone https://github.com/kon-rad/argocut.git ~/ArgoCut
cd ~/ArgoCut
cp apps/web/.env.example apps/web/.env.local
```

Edit `apps/web/.env.local` and set `NEXT_PUBLIC_ARGOCUT_AGENT_API=1`.

```bash
docker compose up -d db redis serverless-redis-http
cd apps/web && bun run db:migrate
cd ~/ArgoCut && bun install
bun dev:web
```

Confirm `http://localhost:3000` loads before you close the laptop tonight.

| Tool | Why | Cost |
|---|---|---|
| [Docker Desktop](https://www.docker.com/products/docker-desktop/) | ArgoCut's projects and brands live in Postgres | 0 USD |
| Claude in Chrome extension | Lets the agent drive the tab you are looking at | 0 USD |

Also install ffmpeg if Module 3 did not: `brew install ffmpeg`.

**Bring:** one raw phone video of a finished or near finished fence job, a walkthrough or a
testimonial, 16 by 9, 1080p or better. A 720p source cannot be reframed to a vertical short
without going soft, the same resolution honesty from Module 3.

## Homework

1. Point `/job-reel` at a second job video and bring `edit-plan.md`.
2. Export the clip by hand from the ArgoCut UI, or decide not to, and write one sentence on
   why.
3. Feed the skill a video with no clean hook, on purpose, and report what it did.
4. Run `describe_params` on the title card element and change one param yourself, directly
   in the timeline.

## The honest caveat

There is no export tool, and that is not a gap to be filled later. Rendering a client
facing video and rendering an agent's opinion of one are different acts. The module builds
the first all the way to a reviewable clip and stops there on purpose.

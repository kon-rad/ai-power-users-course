# Module 4: Agenda (Run of Show)

**Module 4 of AI Power Users · 60 minutes · no break · live on YouTube**
**Platform: macOS.** Theme: real footage, not generated media, assembled live inside a
browser tab.

The two time sinks: Docker and Postgres not answering at 0:02, and a cut plan with no clean
hook at 0:40. **Protect 0:48 to 0:57, the live assembly.** If the clock slips, cut earlier
segments, not the assembly.

## Part 1: Your agent gets an editing bench (26 min)

| Time | Segment | What happens | Attendee action |
|---|---|---|---|
| 0:00-0:02 | **Open** | State the artifact and the cost, 0 USD | Watch |
| 0:02-0:07 | **Confirm the bench** | `curl localhost:3000/api/health`, `window.__argocutAgent.isReady()` in devtools, hard refresh if stale | Confirm their own instance |
| 0:07-0:11 | **Wire up the tab** | Load claude-in-chrome tools, agent finds or opens the ArgoCut tab | Confirm the tab id |
| 0:11-0:26 | **Build `/job-reel`** | Paste the brief, agent writes the skill, answer its questions | Paste the brief |

## Part 2: Use it (34 min)

| Time | Segment | What happens | Attendee action |
|---|---|---|---|
| 0:26-0:33 | **The fence-co preset** | Copy `brand-presets.json` into the project, add a `fence-co` entry from `design-brief.md` | Build their own preset |
| 0:33-0:40 | **Probe and transcribe** | Real job video in, probe, transcribe, stop before planning | Bring their own footage |
| 0:40-0:48 | **Plan the cut** | `/job-reel` writes `edit-plan.md`, read against the rubric before approving | Approve or reject the plan |
| 0:48-0:57 | **Assemble live** | One `apply_edits` batch: clip, captions, title, in the open tab | Watch their own tab |
| 0:57-1:00 | **Review and close** | Play the clip, state the permanent artifact, state the export boundary | Review their own clip |

## Materials on screen

- Student guide, pinned in chat
- A terminal running `curl localhost:3000/api/health`
- The ArgoCut tab itself, at every segment after 0:07
- Module 3's `design-brief.md`, open, for the preset segment

## Homework (briefed at 0:57)

1. A second job video through `/job-reel`. Bring `edit-plan.md`.
2. Export by hand, or decide not to, and say why in one sentence.
3. A video with no clean hook, on purpose. Report what the skill did.
4. Change one param on the title card by hand, in the timeline.

## Contingency

- **At 0:07 and `window.__argocutAgent` is still undefined:** restart `bun dev:web` with
  the flag confirmed in `.env.local`, then hard refresh. If it still fails, switch that
  student to the pre transcribed backup clip and pair them with a working machine for the
  live assembly.
- **At 0:26 and behind:** skip live authoring of the `fence-co` preset. Paste a pre built
  entry and move on. Note it as homework to build their own.
- **At 0:40 and the transcript has no clean hook:** use the pre transcribed backup job
  video for that student, and keep their own footage as a homework retry.
- **If Docker or Postgres will not come up on a student's machine:** that student follows
  along on the shared screen this session, and does the build as homework once it is
  fixed. Do not spend live minutes debugging one machine's Docker install.
- **Never cut:** the `trimEnd` versus out point distinction, the one atomic batch rule, and
  the no export statement at close.

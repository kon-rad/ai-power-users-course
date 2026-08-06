# Module 2 — Agenda (Run of Show)

**Module 2 of AI Power Users · 110 minutes · live on YouTube**
Theme: your agent writes its own tools, then vibe codes a real client website from a
blank folder to a live URL.

> Host note: the two biggest time-sinks are the live build and anything that has to
> install. Everything installable is in the student guide as a "before you arrive" step.
> **Protect the last 16 minutes for verify and deploy** — a live URL is the deliverable,
> and a session that runs out of time before shipping has failed at the one thing it
> promised.

## Part 1 — Agent Mastery (34 min)

| Time | Segment | What happens | Attendee action |
|---|---|---|---|
| 0:00–0:04 | **Open with the payoff** | Show the finished, live fence site on a phone. Agency price: $4,000–8,000, 6–8 weeks. Then the premise: you describe, the agent builds | Watch |
| 0:04–0:12 | **Models, tokens, the cache trap** | What a token is · context rot demo (fresh vs. bloated session) · `/model` flags and aliases · the four profiles · session vs. task | Set up aliases |
| 0:12–0:24 | **Build three skills from one brief** | Read the brief; the agent writes `model-research`, `switch-models`, `spend-tracker`, then runs each once | Paste the brief, answer its questions |
| 0:24–0:31 | **SOUL.md vs HERMES.md** | Global identity vs. project rules. Measure both with `hermes prompt-size` | Write SOUL.md |
| 0:31–0:34 | **Wire into the standup** | Weekly cadence, not daily. Standup asks only when something is 7+ days stale | Update the standup skill |

## Break (10 min) — 0:34–0:44

> "When we come back: a real client, a real website, and a real live URL by the end."

## Part 2 — Vibe Coding a Pro Website (66 min)

| Time | Segment | What happens | Attendee action |
|---|---|---|---|
| 0:44–0:48 | **1. Install the engineering skills** | Ask the agent to install Superpowers + frontend-design. Why process beats prompt | Run the install ask |
| 0:48–0:53 | **2. Set the goals** | `goals.md` — what must this achieve, what counts as failure | Answer as the client |
| 0:53–1:03 | **3. Build the knowledge base** | Client interview into `client-brief.md`; agent reads the photos and writes `image-inventory.md` plus a shot list | Drop photos in, answer questions |
| 1:03–1:12 | **4. Layout and functionality** | Describe the site out loud; agent writes `layout.md` and the functionality spec, then image prompts | Describe your site |
| 1:12–1:19 | **5. Architecture, stack, database** | One recommendation, then make the AI argue against itself. Does this even need a database? | Review and decide |
| 1:19–1:23 | **6. The plan + project rules** | `plan.md` you approve, `HERMES.md` for the folder. Change the plan on stream | Approve or amend |
| 1:23–1:41 | **7. Build** | `/switch-models coding`, then `frontend-design` executes the plan with the real photos | Follow along |
| 1:41–1:46 | **8. Verify** | Evidence, not assurances. Mobile at 375px, form submits, Lighthouse | Check your own site |
| 1:46–1:54 | **9. Git, deploy, live** | git init, secrets check, GitHub, Vercel, live URL on a phone | Deploy yours |
| 1:54–1:57 | **10. What it's worth** | Pricing, and the honest caveat | Note the homework |

## Materials on screen

- Student guide (this repo) — pinned in chat
- openrouter.ai — the `fast`, `smart`, `coding` profiles
- github.com — version control and deploy source
- vercel.com — where the site goes live
- github.com/obra/superpowers — the engineering skills

## Homework (briefed at 1:54)

1. Run `/daily-standup` every morning for a week; `/model-research` and `/spend-tracker`
   once that week. Report whether weekly felt right.
2. Practise one task per session — `/reset` between tasks. Report what changed.
3. Get `SOUL.md` good, then cut it under 150 lines. `hermes prompt-size` before and after.
4. Use `/model --once` three times.
5. Rebuild the site for a real local business in your town.
6. Lighthouse mobile performance above 90.
7. Deploy it to a real live URL.
8. Write a one-page proposal for that business. Optional: actually send it.

## Contingency (if the build runs long)

- **At 1:35 and behind:** cut the before/after gallery. Ship hero, trust strip, services,
  quote form. A shipped rough site beats a perfect plan.
- **If Superpowers will not install:** fall back to direct raw `SKILL.md` URLs, or run the
  build with plain prompting and name what the process would have added.
- **If the live build stalls entirely:** switch to the pre-built site, walk the same ten
  steps against it, and keep the git and deploy segments live. Those are the two that make
  it a deliverable rather than a demo.

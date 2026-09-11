# Module 3: Agenda (Run of Show)

**Module 3 of AI Power Users · 60 minutes · no break · live on YouTube**
**Platform: macOS.** Theme: your agent gets a camera, and the site gets a design, a moving
hero, and an address you own.

> Host note: the two time sinks are the live generation, which is queue dependent and out
> of your control, and the design segment, which will expand to fill any space you give it.
> **Protect 0:53 to 0:58 for the domain and the deploy.** A live domain is the deliverable.
> If the clock slips, cut from the design segment, never from the ship.

## Part 1: Your agent gets a camera (31 min)

| Time | Segment | What happens | Attendee action |
|---|---|---|---|
| 0:00-0:03 | **Open with the payoff** | Module 2 site and the finished site side by side. Static hero versus moving hero, preview URL versus real domain. Say the cost of the session out loud: about 2 USD | Watch |
| 0:03-0:08 | **macOS ground rules, reopen the project** | Mac equivalents table, `brew install ffmpeg`, then get the agent caught up from `HERMES.md` | Install ffmpeg, reopen the project |
| 0:08-0:13 | **The key and where it lives** | fal account, spend limit, key into `.env`, `.env` into `.gitignore` first, `git status` proves it. Agent scans the repo for credentials | Store the key, run the scan |
| 0:13-0:25 | **Build two skills from one brief** | Read the brief on screen. Agent writes `media-models` and `media-gen`, then runs each once. Read the description lines aloud. Point at the cost gate | Paste the brief, answer its questions |
| 0:25-0:31 | **Read a catalogue properly** | `/media-models` writes the briefing. Mixed units, wrong family, resolution and duration caps. Work out the cost of one five second 1080p clip | Run it, read your own briefing |

## Part 2: Use them (29 min)

| Time | Segment | What happens | Attendee action |
|---|---|---|---|
| 0:31-0:38 | **The hero video** | Say what image to video really is before generating. One photo, slow push in with a lateral drift, two variants, pick one. ffmpeg to a web ready mp4 plus poster frame | Animate your own photo |
| 0:38-0:46 | **Design by example** | Screenshot three to five admired sites, `Cmd+Shift+4`. Agent extracts type, space, colour, hierarchy and motion into `design-brief.md`. Rules, not adjectives. Where the copying line is | Capture yours, run the brief |
| 0:46-0:53 | **Rebuild** | `/switch-models coding`, apply `design-brief.md`, hero video with poster, muted, playsinline, reduced motion respected. Check 375px and reduced motion live | Rebuild yours |
| 0:53-0:58 | **Domain and deploy** | Buy live, attach to the project, apex and www, certificate check, update the hardcoded preview URL, push. Auto renew turned on where the class can see it | Buy and attach yours |
| 0:58-1:00 | **Close** | What it cost, what is permanent, the honest caveat about when a video beats a photograph. Homework | Note the homework |

## Materials on screen

- Student guide, pinned in chat
- fal.ai, the model gallery and the dashboard keys page
- The Module 2 site on its `vercel.app` preview URL
- vercel.com, the project's domain settings
- Three to five design reference sites, tabs open in advance

## Homework (briefed at 0:58)

1. Run `/media-models` once. Name the cheapest model that would do your hero and what you
   give up.
2. Three hero clips from three photographs. Bring the worst one and say what went wrong.
3. A `design-brief.md` from five screenshots, for a business that is not a fence company.
4. Buy a domain. Put something on it.
5. Lighthouse mobile performance above 90 with the video in the hero.
6. Total your media spend for the week and price the same work for a client.

## Contingency

- **At 0:31 and behind:** skip the second video variant. Generate one, use it, and say on
  camera that you would normally generate two.
- **If the generation fails or comes back warped twice:** cut to the pre generated clip in
  reserve, keep the segment moving, and come back to the failed take at 0:58 as the honest
  caveat. A visible failure is good teaching, a stalled stream is not.
- **At 0:46 and behind:** cut the design segment to the `design-brief.md` output only.
  Apply it to the hero alone rather than the whole site.
- **If the domain purchase stalls on payment or verification:** switch to a domain bought
  in advance, attach that one live, and narrate the purchase flow from a screenshot. The
  attach and the certificate check are the teaching, not the checkout.
- **Never cut:** the `.gitignore` before `.env` order, the cost estimate gate, and the live
  https check on the domain.

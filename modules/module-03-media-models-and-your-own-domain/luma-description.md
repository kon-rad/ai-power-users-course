# Luma Event: Module 3

> House rules: short, concrete, no em dashes, no emojis, links at the end, and the host
> and sponsor footer pasted byte for byte.

## Event title

**The one we are using:**

AI Power Users · Module 3: AI Media Models, Website Improvement, and Professional Deployment (Live)

**Two alternates:**

2. AI Power Users · Module 3: Media Models, a Real Redesign, and a Domain You Own (Live)
3. AI Power Users · Module 3: Cheapest Model That Does the Job, Then Ship the Site (Live)

## Short blurb (for previews)

Your agent learns which media models it can call and what each one costs, then makes a hero
video, redesigns your site, and puts it on a domain you own. One session, 60 minutes.
About 2 USD of generation credit.

## Full description

This is Module 3 of AI Power Users, a free hands on live course from Argo. This one runs on
macOS.

In Module 2 your agent wrote its own tools and then built a client website from a blank
folder to a live URL. Today it gets a camera, and the site stops looking like a template.

Two halves.

**First, we build a skill that finds the latest and greatest models and what they cost.**
From one brief, your agent writes two skills:

**/media-models** fetches the current catalogue of image and video models it can actually
call on fal, the newest open ones included. Not a leaderboard. The exact endpoint id, the
exact input fields, the live price with the unit stated, the resolution and duration caps.
Three picks per family: cheapest usable, best value, best quality. Then it computes the one
number that decides everything, what a single five second 1080p clip costs from each.

**/media-gen** generates the image or the video and files it properly. It works out which
model family the job needs, reads the newest briefing to pick the endpoint, estimates the
cost and tells you the number before it runs, and stops for confirmation above a threshold
you set. Afterwards it records the prompt, the endpoint, the settings and what you actually
spent, next to the file.

The point is not knowing which model is best today. That changes monthly, and a skill with
a model name hardcoded in it is wrong by the time you finish writing it. The point is that
your agent looks it up, converts every price to the same job before comparing, and uses the
cheapest model that does the work.

**Then we make the website look professional and put it on a real address.**

1. Turn one photograph of finished work into a five second hero video with a slow camera
   move.
2. Screenshot three to five sites you admire, and have the agent extract the rules that make
   them work: type scale, spacing scale, colour set, what the eye lands on first.
3. Rebuild the site with that design and the video hero, without wrecking mobile load time
   and without ignoring reduced motion settings.
4. Buy a domain, attach it, and confirm https is live. On camera, on a phone.

**By the end you will have:**

- Two agent built skills that work on every future project, not just this one
- A hero video made from your own photograph, web ready, under 2 MB, with a poster frame
- A `design-brief.md` with a type scale, a spacing scale and a colour set, applied
- The site from Module 2, redesigned, on a domain you own, on https

**Who it is for:** anyone who finished Module 2, or anyone with a working agent and a site
they want to finish. No coding experience required.

**The honest part, two things:**

- A video hero is not automatically better than a photograph. It is better when the motion
  shows something a still cannot. Otherwise a sharp photo beats it, loads faster, and never
  warps a fence post.
- The depth in the clip is inferred from one photograph, not modelled. It is a guess. Slow
  camera moves are how you keep the guess honest, because the slower the move, the less the
  model has to invent.

**What it costs:** this is the first module that costs money. About 2 USD of media
generation credit on fal, and roughly 10 to 20 USD a year if you buy a domain during the
session. Hosting stays free. Add the credit before you arrive, not during the session.

**Format:** one live session, 60 minutes, no break. Live on YouTube.

## Links

Course page: https://myargoquest.com/courses/ai-power-users/module-3

Setup and download instructions:
https://github.com/kon-rad/ai-power-users-course/blob/main/modules/module-03-media-models-and-your-own-domain/student-guide.md

Everything you need to install beforehand is in the setup instructions. Start there.

> **Both URLs returned 404 when checked on 2026-08-15.** They follow the pattern that
> resolves for Module 2, so they are the right shape, but do not publish the description
> until each one loads.

## Footer (fixed copy, paste byte for byte)

Paste the host and sponsor block from
`.claude/skills/argo-events/assets/luma-footer.md` verbatim at the end of the description.
It contains zero width characters. Do not retype it. Copy the file.

## Suggested Luma settings

- Cost: Free to attend, about 2 USD of fal credit required
- Capacity: uncapped, livestream
- Location: Online, YouTube Live, link sent to registrants
- Host: Argo (Konrad Gnat)
- Duration: 75 minutes, a 60 minute session plus buffer
- Tags: AI, Agents, Video, Design, Web Development, No-Code, Beginner-Friendly

## Notes for the host

- Lead the promo image with a frame from the hero video on a phone, not a terminal. The
  deliverable sells the session.
- The 2 USD credit requirement has to be visible before someone registers, not discovered in
  the session. Every module after this one needs the same credit.
- Do not name a model in the description. Whatever is named will be wrong by the stream
  date, and the module's whole point is that you look it up instead of memorising it.
- macOS is stated in the first line of the description on purpose. Module 0 was Windows and
  people will ask.

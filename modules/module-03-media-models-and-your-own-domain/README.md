# Module 3: Media Models and Your Own Domain

**Module 3 of AI Power Users · 60 minutes · live on YouTube · run on macOS**
**From Argo, myargoquest.com**

Module 2 built a working client website and left it on a preview URL with a static hero.
This session gives your agent access to image and video models, turns one photograph into a
moving hero, gives the site a design direction taken from work you admire, and puts the
whole thing on a domain you own.

This is the first module that costs money. Budget about 2 USD of media generation, plus
roughly 10 to 20 USD a year for the domain.

## What you build

```
PART 1: your agent gets a camera
    fal key in .env  ──  one brief  ──  /media-models   what can I call, and what does it cost
                                    ──  /media-gen      generate it, file it, log what it cost

PART 2: use them
    one photograph  ──  image to video  ──  hero clip + poster frame
    5 screenshots   ──  design-brief.md ──  rules, not adjectives
                                        │
                            rebuild ──  domain ──  LIVE ON HTTPS
```

## Learning objectives

By the end of this module you can:

1. Store an API key on macOS so your agent can use it and git never sees it
2. Tell text to image, text to video and image to video apart, and pick the right one
3. Work out what one generation will cost **before** you run it
4. Turn one photograph into a hero clip, and explain why the depth is inferred rather
   than real
5. Get an agent to extract a design direction from screenshots, and know where the
   copying line is
6. Put a video in a hero without wrecking mobile load time or ignoring reduced motion
7. Buy a domain, attach it to a deployment, and confirm HTTPS is live

## The two skills your agent writes

| Skill | Cadence | What it does | Why it matters |
|---|---|---|---|
| **`/media-models`** | **Monthly** plus on demand | Reads the fal catalogue and writes a briefing: endpoint ids, exact prices with units, input fields, resolution and duration caps, and the cost of one five second 1080p clip from each pick | Your Module 2 `/model-research` skill reads leaderboards and tells you what is **good**. This one tells you what you can **call**. A leaderboard cannot be pasted into code |
| **`/media-gen`** | On demand | Picks the family, picks the endpoint from the latest briefing, estimates the cost and shows you the number before spending it, generates two variants, converts video for the web, and logs what it actually cost | The cost gate is four lines of the brief and it is the whole difference between a tool and a liability |

> **Why no model name is hardcoded.** Media models change monthly. A skill with a model
> name written into it is wrong by the time you finish writing it. The skill is told where
> to look, not what to use.

## The eight steps

| # | Step | Output |
|---|---|---|
| 1 | macOS ground rules, reopen the project | The agent caught up from `HERMES.md` |
| 2 | The fal key, stored properly | `.env`, `.gitignore`, a clean `git status` |
| 3 | **Build the two skills** from one brief | `/media-models`, `/media-gen` |
| 4 | Run the discovery | A dated media model briefing in your vault |
| 5 | **The hero video** from one photograph | A five second clip, a poster frame, under 2 MB |
| 6 | **Design by example** from screenshots | `design-brief.md` |
| 7 | Rebuild with the direction and the video | The site |
| 8 | **Buy the domain and deploy** | **A live https URL you own** |

## The ideas that carry the module

**Know the price before you spend it.** Text models are cheap enough to be careless with.
Media models are not. The habit that matters is not picking the best model, it is being
able to work out what a job costs before running it.

**The family decides the result.** If you already have the photograph, you want image to
video. Hand the same job to a text to video model and it will invent a different fence.
Most bad media output is the right prompt sent to the wrong family.

**The depth is a guess.** Image to video infers depth from one photograph and re-renders
the frames. Ask for a slow camera move and the model has less to invent, which is why slow
moves look expensive and fast ones look cheap.

**Rules, not adjectives.** "Modern and clean" gets you the average of everything the model
has ever seen. A type scale, a spacing value and one accent colour get you a decision you
can check against the built page.

**Steal the mechanism, not the layout.** Extracting principles from work you admire is what
designers have always done. Reproducing someone's site is not. The test is whether a
visitor could tell where you got it.

**A domain is rented, not bought.** Turn on auto renew the day you buy it.

## Files in this module

- [`syllabus.md`](./syllabus.md), the full plan: every segment, every brief, host guidance
- [`agenda.md`](./agenda.md), the 60 minute run of show and the contingency
- [`student-guide.md`](./student-guide.md), **follow along here**: every step, link and
  copy pasteable brief
- [`quiz.md`](./quiz.md), 5 multiple choice plus 4 open ended, peer evaluated
- [`luma-description.md`](./luma-description.md), event copy scaffold

## Before the session

**You need Module 2's project**, built, committed and deployed. If you missed it, the
[Module 2 student guide](../module-02-agent-mastery-and-vibe-coding/student-guide.md)
takes about 110 minutes.

| Account | Why | Cost |
|---|---|---|
| [fal.ai](https://fal.ai) | Image and video generation | **Add about 2 USD of credit** and set a spend limit |

Also install ffmpeg: `brew install ffmpeg`.

**Bring:** two or three high resolution photographs of the business, three to five websites
you admire with the tabs open, and a payment method if you want to buy your domain live.

## Homework

1. Run `/media-models` once. Name the cheapest model that would do your hero, and what you
   give up by using it.
2. Generate three hero clips from three different photographs. **Bring the worst one** and
   say what the model got wrong.
3. Build a `design-brief.md` from five screenshots for a business that is not a fence
   company, and apply it.
4. Buy a domain. Any domain. Put something on it.
5. Get Lighthouse mobile performance above 90 **with the video in the hero**.
6. Total your media spend for the week from your `spend-tracker` log, and price the same
   work for a client.

## The honest caveat

A video hero is not automatically better than a photograph. It is better when the motion
shows something a still cannot, which for a fence is depth, length, and how light sits on
the boards. If the clip does not do that, a sharp photograph beats it, loads faster, and
never warps a post.

The same applies to the domain. It makes the site look like a business, and it changes
nothing about whether the site gets quote requests. That was decided in Module 2, in
`goals.md`.

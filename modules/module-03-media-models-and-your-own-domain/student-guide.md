# Module 3: Student Guide

**Media Models and Your Own Domain**
Follow along here. Every step, every link, every brief you can copy and paste.

By the end you will have two new skills your agent wrote, a hero video made from one of
your own photographs, a design direction taken from sites you admire, and the site live on
a domain you own.

**This module is run on macOS.** Every command and key binding below is the Mac one.

---

## Before the session

### You need Module 2's project

The fence company site, built, committed, and deployed. If you missed it, the
[Module 2 student guide](../module-02-agent-mastery-and-vibe-coding/student-guide.md) takes
about 110 minutes. Do it first.

Check it still works:

```bash
cd ~/SecondBrain/1-Projects/fence-co-website
git status
hermes
```

### One account, and it costs money

| Account | Why | Cost |
|---|---|---|
| [fal.ai](https://fal.ai) | Image and video generation | **Add about 2 USD of credit.** Set a spend limit while you are in there |

This is the first module with a real cost. Media models bill per image and per second of
video, and there is no free tier that produces a usable clip. Two dollars covers the
session with room to redo the things that come out wrong.

You already have GitHub and Vercel from Module 2.

### Install ffmpeg

```bash
brew install ffmpeg
ffmpeg -version
```

If `brew` is not found, install [Homebrew](https://brew.sh) first, then run the above.

### Bring

- **Two or three high resolution photographs** of the business you are building for.
  One of them should be a good hero candidate: wide, well lit, with something in the
  foreground and something behind it.
- **Three to five websites you genuinely admire.** Any industry except your client's.
  Have the tabs open.
- **A payment method**, if you want to buy your domain during the session. Expect roughly
  10 to 20 USD a year for a `.com`.

---

## Step 1: The macOS bits you need today

| Job | Keys or command |
|---|---|
| Open this folder in Finder | `open .` |
| Open it in VS Code | `code .` |
| Show hidden files in Finder | `Cmd+Shift+.` |
| Screenshot a region | `Cmd+Shift+4` |
| Screenshot a window | `Cmd+Shift+4`, then `Space`, then click |
| Spotlight | `Cmd+Space` |

Screenshots save to your Desktop. You will need that in Step 6.

Reopen the project and get the agent caught up:

```bash
cd ~/SecondBrain/1-Projects/fence-co-website
hermes
```

```
Read HERMES.md, goals.md and layout.md. Tell me in five lines where this project got to
and what is not finished.
```

---

## Step 2: Your fal key, stored properly

1. Sign in at [fal.ai](https://fal.ai), add credit, and **set a spend limit**.
2. Create an API key in the dashboard. Copy it now. It is shown once.
3. Store it:

```bash
cd ~/SecondBrain/1-Projects/fence-co-website
echo '.env' >> .gitignore
echo 'FAL_KEY=paste-your-key-here' >> .env
git status
```

Open `.env` in VS Code and replace the placeholder with your real key.

> **The trap.** `git status` must **not** list `.env`. If it does, your key is one commit
> away from being public. Fix that before you do anything else. Git tracks a file from the
> moment it is first committed, so `.gitignore` has to come first.

Then have the agent check the whole project:

```
Scan this whole project for anything that looks like a credential, an API key, or a
token, including in files that are already committed. Tell me what you find and what you
would do about it. Do not fix anything yet.
```

---

## Step 3: Build the two skills

Paste this whole block into your agent. It is long. That is the point: the brief is the
work.

```
Build me two skills. Ask me anything ambiguous first, then write both, tell me where you
put them, and run each one once.

────────────────────────────────────────────────────────────
SKILL 1 - `media-models`
Finds out what image and video models I can actually call on fal, and what they cost.

This is NOT the same job as my existing `model-research` skill. That one reads
leaderboards and tells me which models are GOOD. This one reads a catalogue and tells me
what I can CALL: the exact endpoint id, the exact input fields, and the exact price. A
leaderboard cannot be pasted into code. An endpoint id can.

CADENCE: monthly, plus any time I ask, plus any time `media-gen` cannot find an endpoint
for something I asked for.

COVERS four families, kept separate because they are not interchangeable:
  - text to image
  - image editing, meaning an image plus an instruction in, a changed image out
  - text to video
  - image to video, meaning a still photo in, a moving clip out

SOURCES, fetched live every time. Never answer from memory and never reuse last month's
prices:
  - The fal model gallery at https://fal.ai/models, which can be filtered by category
  - Each candidate model's own API page at https://fal.ai/models/<endpoint-id>/api, which
    carries the input schema and the price
  - If fal publishes a machine readable catalogue endpoint, use it and record the URL in
    the briefing. If you cannot verify that one exists, say so in one line and read the
    pages instead. Do not invent an API.

FOR EACH family give me three picks: cheapest usable, best value, and best quality. For
each pick record:
  - endpoint id, exactly as it must be typed
  - price, with the UNIT stated: per image, per megapixel, per second of video, per run
  - the required input fields and their names
  - maximum resolution and, for video, maximum duration
  - whether it produces audio
  - one line on what it is actually good at

THEN compute the number I care about: what does ONE five second 1080p clip cost from each
video pick. Show the arithmetic. That is the number that decides what I use.

OUTPUT to 2-Areas/AI-Models/YYYY-MM-DD-media-model-briefing.md:
  - TL;DR, one line per family, model plus price
  - A table per family
  - "What changed" against the most recent previous media briefing in that folder. If
    nothing changed, one line saying so
  - Every source URL with the date fetched

RULES: never guess a price and never mix units inside one table. If a model's page does
not state a price, write UNKNOWN and keep it out of the picks. Flag any price move over
30 percent as PRICE ALERT.

────────────────────────────────────────────────────────────
SKILL 2 - `media-gen`
Generates an image or a video and files it properly.

RUNS WHEN: I ask for an image, a video, a clip, a hero, a thumbnail, or ask to animate a
photo I already have.

BEFORE IT RUNS ANYTHING, EVERY TIME:
  1. Work out which family the job is: text to image, image editing, text to video, or
     image to video. If I hand you a photo and ask for movement, it is image to video,
     never text to video. Say which family you chose and why, in one line.
  2. Read the newest media briefing in 2-Areas/AI-Models/ and pick the endpoint. If the
     newest briefing is over 30 days old, say so and offer to run `media-models` first.
  3. Estimate the cost of this specific run and TELL ME THE NUMBER BEFORE RUNNING.
  4. If the estimate is over 0.50 USD, stop and make me confirm.
  5. Read FAL_KEY from .env. Never print it, never write it into a file, never pass it on
     a command line where it lands in shell history.

PROMPT CRAFT, encoded in the skill so I do not have to remember it:
  - For text to image: brief it like a photographer. Subject, setting, lighting, lens,
    palette, mood. Sentences, not a pile of keywords. State the aspect ratio explicitly.
  - For image to video: the picture already contains the scene, so DO NOT re-describe the
    scene. Describe the CAMERA and what moves: the move, its direction, its speed, and
    what must stay still. Ask for a slow move by default. Fast moves in short clips look
    cheap and expose the model's guesses.
  - Generate 2 variants for anything going on a page, and show me both before I pick.

AFTER IT RUNS:
  - Save into the project's assets folder with a descriptive kebab case filename, never
    output.mp4 or image1.png.
  - For video, use ffmpeg to produce a web ready version: H.264 mp4, no audio track,
    faststart enabled, and a poster frame extracted as a jpg. Report the file sizes.
  - Write a sidecar note next to the file recording: the prompt, the endpoint id, the
    settings, the actual cost, the date, and the source image if there was one.
  - Append the cost to my spend log so `spend-tracker` picks it up.

RULES: never claim a generation succeeded without opening the file and checking it exists
and is not zero bytes. If a run fails, report the error verbatim and do not silently
retry more than once. Tell me what I actually spent, not what I estimated.
```

Answer its questions. When it is done, open the two `SKILL.md` files it wrote and read the
description line at the top of each. That line is the only part your agent sees when
deciding whether to run the skill.

---

## Step 4: Find the models

```
/media-models
```

Open the briefing it writes and read it properly. Three things trip people up:

| Trap | What to do |
|---|---|
| **Mixed units** | One model is priced per image, another per megapixel, another per second. Convert everything to the same job before comparing: one 1080p image, one five second clip |
| **Wrong family** | If you already have the photo, you want **image to video**. A text to video model will invent a different fence |
| **Caps** | A model can look cheap and max out at 4 seconds or 720p. Read the cap before the price |

The number that decides everything: **what does one five second 1080p clip cost.** Your
briefing has it for each pick. Use that number, not one you read in a blog post.

---

## Step 5: Turn a photograph into a hero video

Put your photos in place first:

```bash
mkdir -p assets/photos
open assets/photos
```

Drag your photographs in.

```
Use media-gen. Animate assets/photos/<your-hero-photo>.jpg into a five second hero clip.

Image to video, not text to video. The fence in that photo is the client's real work and
it must stay exactly as it is.

Camera: a slow push in, very slight, with a small lateral drift to the right so the posts
separate from the background and the eye reads depth. No zoom snap, no orbit, no crash in.
Nothing in the scene changes: no people appear, no gate opens, no leaves blow, the fence
does not change colour or shape.

Loopable if the model supports it. No audio.

Show me the cost estimate first. Generate two variants and let me pick.
```

> **What is actually happening.** The model is not building a 3D model of your fence. It
> infers depth from one photograph and re-renders each frame as the camera moves. It looks
> three dimensional and it is a guess. The slower the move, the less it has to invent, and
> the fewer places the guess goes wrong.

Watch each variant **on loop for ten seconds** before choosing. Warped posts and pickets
that grow show up on the second pass, not the first.

Then make it web ready:

```
Take the variant I picked and make it web ready:
  - H.264 mp4, no audio, faststart enabled, under 2 MB if you can get there without it
    looking bad. Tell me the size before and after.
  - Extract a poster frame as a jpg, same aspect ratio.
  - Put both in public/ and tell me the paths.
```

---

## Step 6: Take a design direction from sites you admire

Screenshot three to five sites you like. `Cmd+Shift+4`, then `Space`, then click the
window. They land on your Desktop.

> Pick sites from **any industry except your client's**. Copy a fence company and you will
> build the site they already have. You are stealing the mechanism, not the layout.

```bash
mkdir -p assets/design-refs
mv ~/Desktop/Screenshot*.png assets/design-refs/
```

```
Look at every screenshot in assets/design-refs/. For each one, write down what is
actually doing the work, not what it looks like. Specifically:

  - Type: how many sizes are in use, how big is the jump between them, what is the
    heading to body ratio, is it one family or two
  - Space: how much empty space surrounds the main element, what is the vertical rhythm
    between sections, how wide is the text column
  - Colour: how many colours are really used, what is the background, where does the one
    accent colour appear and how rarely
  - Hierarchy: what does the eye land on first, second, third, and what makes that happen
  - Motion: what moves, when, and how slowly

Then write design-brief.md with:
  1. The three or four rules these examples have in common. Rules, not adjectives.
  2. What of this fits my client and their visitors, and what does not. Be specific about
     what you are rejecting and why.
  3. A concrete spec for our site: type scale in pixels, spacing scale, the colour set
     with hex values, and where the accent colour is allowed to appear.

Cross check every rule against goals.md. If any of it works against getting quote
requests, say so and drop it.
```

> **Why this works.** "Modern and clean" gets you the average of everything the model has
> ever seen. "Headings are three times the body size, sections are 96 pixels apart, one
> accent colour used twice per page" gets you a decision you can check.

---

## Step 7: Rebuild

New task, so start a new session first. Module 2's rule: reset between tasks, never during
one.

```
/switch-models coding
```

```
Use the frontend-design skill. Apply design-brief.md across the whole site, and rebuild
the hero to use the video.

The hero video:
  - autoplay, muted, loop, playsinline, with the poster jpg set so something is on screen
    before the video loads
  - the poster image stays visible and the video never loads if the visitor has reduced
    motion turned on
  - the quote button and the phone number stay readable over the video at every width,
    which probably means an overlay
  - the video is decoration. If it fails to load, the hero still works

Everything else from Module 2 stays and is non negotiable:
  - phone number as real tappable text, never inside an image
  - sticky quote button on mobile
  - warranty visible without scrolling
  - works at 375 pixels wide

Then tell me what this did to page weight and to the largest contentful paint.
```

**Check three things yourself before believing any of it:**

1. **375 pixels wide.** Responsive mode in your browser.
2. **Reduced motion.** System Settings, Accessibility, Display, Reduce motion. Reload. The
   video should not play.
3. **The file size.** A 12 MB hero video is a broken site for someone standing in a
   driveway on cellular data, which is your actual visitor.

If the video costs you real load time on mobile, serve the poster image on phones and the
video on desktop. `goals.md` wins the argument: quote requests, not decoration.

---

## Step 8: Buy the domain and go live

### Buying

| Route | Trade off |
|---|---|
| **Buy inside Vercel** | Fewest moving parts. Nameservers, renewals and the certificate are handled for you. Your registrar and your host become the same company |
| **Buy at an independent registrar** | More control and easier to move later. One extra step, and a wait for DNS to propagate |

Expect roughly 10 to 20 USD a year for a `.com`. Other extensions vary widely, including
the renewal price, which is often not the price you paid in year one. **Read the renewal
price before you buy.**

> **Turn on auto renew.** A domain is rented, not bought. A client's site going dark
> because a card expired is the most embarrassing failure in this business.

### Going live

```
Get this live on the domain:
  1. Add the domain to the Vercel project.
  2. Set up both the apex and the www version, and redirect one to the other. Tell me
     which one you made canonical and why.
  3. Confirm the certificate is issued and https works with no warning.
  4. Update anything in the site that has the old preview URL hardcoded: the
     LocalBusiness schema, the canonical tag, the sitemap, any absolute links.
  5. Commit and push.

Then prove it. Open the live https URL and show me the response, not a summary.
```

Open it on your phone. That is the deliverable.

---

## If something breaks

| Symptom | Cause and fix |
|---|---|
| **`Unauthorized` or `401` from fal** | The key is not reaching the process. Check `.env` is in the project folder you are running from, that the line is `FAL_KEY=...` with no quotes and no spaces around the `=`, and that you replaced the placeholder |
| **The clip is warped, or a post bends** | Predictable. The model inferred depth wrongly. Ask for a slower and smaller camera move, or pick a photo with more separation between foreground and background. Do not fight it with a longer prompt |
| **The video does not autoplay on iPhone** | It needs `muted` **and** `playsinline`. Without both, iOS refuses to autoplay inline |
| **The site got slower** | The video is too big. Re-encode at a higher CRF, drop to 720p for the hero, or serve the poster image only on mobile |
| **The domain shows a certificate warning** | The certificate is still being issued, or the nameservers have not propagated. Wait, then recheck. If it is still wrong after an hour, the nameservers are wrong |
| **The agent says it generated a file that is not there** | It happens. Check the folder yourself. That is why the skill brief tells it to open the file and verify the size |

---

## Homework

1. Run `/media-models` once. Name the cheapest model that would do your hero, and what you
   give up by using it.
2. Generate three hero clips from three different photographs. **Bring the worst one** and
   say what the model got wrong.
3. Build a `design-brief.md` from five screenshots for a business that is not a fence
   company, and apply it.
4. Buy a domain. Any domain. Put something on it.
5. Get Lighthouse mobile performance above 90 **with the video in the hero**.
6. Add up what you spent on media generation this week from your `spend-tracker` log, and
   work out what you would charge a client for the same work.

---

## What this cost

| Item | Amount |
|---|---|
| Media generation during the session | About 2 USD, including the variants you throw away |
| Domain | Roughly 10 to 20 USD a year for a `.com`, renewal price checked before buying |
| Hosting | Still 0 USD on the Vercel free tier |
| Everything else | 0 USD |

# Module 3: Media Models and Your Own Domain

**Status:** syllabus draft v1, planning phase.
**Date:** 2026-08-14
**Platform:** macOS. Every path, command and key binding in this module is the Mac one.
**Prereq:** Module 2 complete. The fence company site built, committed, and deployed to a
Vercel preview URL.
**Companions:** `student-guide.md` · `agenda.md` · `quiz.md` · `luma-description.md`

---

## Format

**One session, 60 minutes, no break, live on YouTube.**

| Part | Time | Theme |
|---|---|---|
| **Part 1** | 0:00-0:31 | Your agent gets a camera. Connect to fal, find the models, build the skills |
| **Part 2** | 0:31-1:00 | Use them. Hero video, a design upgrade, a domain, and the site live on it |

**The spine:** in Module 2 the agent learned to research models it could talk to. Today it
learns to run the ones that produce pictures and video, and the site stops looking like a
template and starts looking like a business with a domain.

**What is different about this module:** the student pays for something for the first time.
Media models are not free and there is no free tier that generates a usable video. Budget
about 2 USD of fal credit for the session. Say this in the first two minutes.

---

## Learning objectives

By the end, a student can:

1. Store an API key on macOS so it is available to the agent and never reaches a git commit
2. Tell the three media model families apart: text to image, text to video, image to video,
   and say which one a given job needs
3. Read a media model listing and work out what one run will cost before running it
4. Turn one still photograph into a short hero video with a camera move, and explain why
   the result is inferred depth rather than a 3D model
5. Get an agent to extract a design direction from screenshots of sites they admire, and
   say what may and may not be copied
6. Put a video in a hero section without wrecking mobile load time or ignoring reduced
   motion settings
7. Buy a domain, point it at a deployment, and confirm HTTPS is live

---

# PART 1: Your agent gets a camera (31 min)

## 0:00-0:03 · Open with the payoff (3 min)

Two browser windows side by side on screen. Left: the Module 2 site, static hero photo, on
its `vercel.app` URL. Right: the finished version, hero video moving, on a real domain.

> "Same site, one hour apart. The photo moved, the design got a point of view, and the
> address stopped saying vercel dot app."

Then the cost line, said once and plainly:

> "This is the first module that costs money. Media models bill per image and per second of
> video. Two dollars of credit covers everything we do today, with room to redo things that
> come out wrong. That number is on the screen for the whole session."

---

## 0:03-0:08 · macOS ground rules and reopening the project (5 min)

Module 0 was Windows. This cohort is on macOS, so the muscle memory changes. Run this fast
and do not teach the terminal from scratch.

| Job | macOS |
|---|---|
| Open a folder in Finder from the terminal | `open .` |
| Open the folder in VS Code | `code .` |
| Copy the output of a command | `pbcopy`, as in `cat .env.example \| pbcopy` |
| Show hidden files in Finder | `Cmd+Shift+.` |
| Screenshot a region | `Cmd+Shift+4` |
| Screenshot a whole window | `Cmd+Shift+4`, then `Space`, then click |
| Screenshot tools and recording | `Cmd+Shift+5` |
| Spotlight | `Cmd+Space` |
| Home folder | `~`, which is `/Users/you` |

Screenshots land on the Desktop by default. That matters at 0:38, because we are about to
take several.

**Install the one missing tool:**

```bash
brew install ffmpeg
ffmpeg -version
```

> "Homebrew is the Mac package manager. `ffmpeg` is the piece of software that every video
> tool on earth is quietly built on. Your agent will drive it, you will never learn its
> flags, and that is the correct division of labour."

**Reopen the project, one task per session, exactly as Module 2 taught:**

```bash
cd ~/SecondBrain/1-Projects/fence-co-website
hermes
```

```
Read HERMES.md, goals.md and layout.md. Tell me in five lines where this project got to
and what is not finished.
```

> "That is what `HERMES.md` was for. A week later, a fresh session, and the agent is caught
> up in one message."

---

## 0:08-0:13 · The key, and where it lives (5 min)

Do this properly once. Every module after this one reuses the pattern.

1. Create an account at **fal.ai**, add credit, open the dashboard, create an API key.
2. Copy it once. It is shown once.
3. Store it:

```bash
cd ~/SecondBrain/1-Projects/fence-co-website
echo 'FAL_KEY=paste-your-key-here' >> .env
echo '.env' >> .gitignore
git status
```

`git status` must not list `.env`. If it does, stop and fix it before anything else.

| Rule | Why |
|---|---|
| The key lives in `.env`, never in a file you commit | Public repos are scraped continuously. A leaked key is spent by someone else within minutes |
| `.env` goes in `.gitignore` before the key goes in `.env` | Order matters. Git tracks a file from the moment it is first committed |
| Commit a `.env.example` with the names and no values | So the next person, or the client, knows what is required |
| Set a spend limit in the fal dashboard | The one control that turns a bad loop into an annoyance instead of a bill |

> "Module 2's rule was do not send client data to a public provider. This module's rule is
> do not send your key anywhere at all. Different failure, same habit: know where the thing
> is before you need it."

**The check that is worth 60 seconds on camera:**

```
Before we do anything else: scan this whole project for anything that looks like a
credential, an API key, or a token, including in files that are already committed. Tell
me what you find and what you would do about it. Do not fix anything yet.
```

---

## 0:13-0:25 · BUILD BY ASKING: two skills, one brief (12 min)

**The centrepiece.** Read this to the agent on screen, verbatim.

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

### While it works, host guidance

- **Narrate.** Silence kills a stream.
- **Answer its clarifying questions on camera.** That interaction is the lesson.
- **Open the generated `SKILL.md` and read the description line aloud.** Callback to Module
  2: the description is the only part the agent sees when deciding whether to fire.
- **Point at the cost gate.** It is four lines of the brief and it is the whole difference
  between a tool and a liability.

> "Notice what we did not do. We did not tell it which model to use. Models change monthly
> and a skill with a model name hardcoded in it is wrong by the time you have finished
> writing it. We told it where to look instead."

---

## 0:25-0:31 · Run the discovery, and read a catalogue properly (6 min)

```
/media-models
```

Open the briefing in Obsidian while it writes. Then teach the three things students get
wrong when they read a media model listing.

| Trap | What it looks like | What to do |
|---|---|---|
| **Mixed units** | One model priced per image, another per megapixel, a third per second | Convert everything to the same job before comparing. One 1080p image. One five second clip |
| **The family mismatch** | Asking a text to video model to animate a photo you already have | If you have the picture, you want image to video. A text to video model will invent a different fence |
| **Resolution and duration caps** | A model that looks cheap and maxes out at 4 seconds or 720p | Read the cap before the price |

### The number that matters

Have the agent read out the cost of one five second 1080p clip for each video pick. As of
**2026-08-14**, image to video pricing on fal sits in the region of **0.07 to 0.14 USD per
second** for the mid tier models and **0.10 to 0.20 USD per second** for the top tier ones,
so a five second clip lands roughly between **0.35 and 1.00 USD**. **Re-verify the morning
of the stream and read the briefing's number, not this one.**

> "This is the actual skill. Not knowing which model is best today, because that changes.
> Knowing how to work out what a job costs before you run it, because that does not."

---

# PART 2: Use them (29 min)

## 0:31-0:38 · The hero video, from one photograph (7 min)

**The job:** one still photo of a finished fence becomes a five second clip with a slow
camera move, so the hero has depth and motion instead of being a flat picture.

### Say what it actually is, before generating anything

> "The model is not building a 3D model of that fence. It infers depth from a single
> photograph and re-renders the frames as the camera moves. It looks three dimensional and
> it is a guess. That is why we ask for a slow move: the slower the move, the less new
> information it has to invent, and the fewer places its guess can be wrong."

### The brief

```
Use media-gen. Animate assets/photos/<the-hero-photo>.jpg into a five second hero clip.

Image to video, not text to video. The fence in that photo is the client's real work and
it must stay exactly as it is.

Camera: a slow push in, very slight, with a small lateral drift to the right so the posts
separate from the background and the eye reads depth. No zoom snap, no orbit, no crash in.
Nothing in the scene changes: no people appear, no gate opens, no leaves blow, the fence
does not change colour or shape.

Loopable if the model supports it. No audio.

Show me the cost estimate first. Generate two variants and let me pick.
```

### Then the web version, driven by the agent

```
Take the variant I picked and make it web ready:
  - H.264 mp4, no audio, faststart enabled, under 2 MB if you can get there without it
    looking bad. Tell me the size before and after.
  - Extract a poster frame as a jpg, same aspect ratio.
  - Put both in public/ and tell me the paths.
```

### Host guidance

- **Show the failure.** If a variant warps a post or grows a picket, keep it on screen and
  name it. That artefact is the honest limit of the technique and it teaches more than the
  good take does.
- **Watch it on loop for ten seconds.** Anything wrong shows up on the second pass.
- **Say the file size out loud.** A 12 MB hero video is a broken site on a phone in a
  driveway, which is exactly the visitor `goals.md` describes.

---

## 0:38-0:46 · Design by example (8 min)

The segment that changes how their sites look. Module 2 shipped a competent site. It still
looked like a template, because the agent had no reference except its own defaults.

### Collect the references, live

Three to five screenshots of sites the student genuinely admires. On macOS, `Cmd+Shift+4`,
then space, then click the window. They land on the Desktop.

> "Pick sites from anywhere except your client's industry. Copy a fence company's site and
> you will build the same site they already have. The point is to steal the mechanism, not
> the layout."

```bash
mkdir -p assets/design-refs
mv ~/Desktop/Screenshot*.png assets/design-refs/
```

### The brief

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
  2. What of this fits a fence company in Chicago whose visitors are on phones in a
     driveway, and what does not. Be specific about what you are rejecting and why.
  3. A concrete spec for our site: type scale in pixels, spacing scale, the colour set
     with hex values, and where the accent colour is allowed to appear.

Cross check every rule against goals.md. If any of it works against getting quote
requests, say so and drop it.
```

### The two lines that make this segment

> "Adjectives do not survive contact with a build. Telling an agent to make it modern and
> clean gets you the average of everything it has ever seen. Telling it the heading is
> three times the body size, sections are 96 pixels apart, and there is exactly one accent
> colour gets you a decision you can check."

> "On copying: extracting principles from work you admire is what every designer has always
> done. Reproducing someone's site is not. The test is whether a visitor could tell where
> you got it. If they could, you copied."

---

## 0:46-0:53 · Rebuild the hero and apply the direction (7 min)

```
/switch-models coding
```

Fresh session, task boundary, no cache penalty. Module 2's rule, applied without comment.

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

Everything else from Module 2 is non negotiable and stays:
  - phone number as real tappable text, never inside an image
  - sticky quote button on mobile
  - warranty visible without scrolling
  - works at 375 pixels wide

Then tell me what this did to page weight and to the largest contentful paint.
```

### Host guidance

- **Check reduced motion live.** System Settings, Accessibility, Display, Reduce motion.
  Reload. The video should not play. This is a 20 second demo and almost nobody does it.
- **Check it at 375 pixels** in responsive mode before saying it is done.
- **If the video hurts mobile performance, cut it on mobile.** Poster image on phones,
  video on desktop. Say why: the goal is quote requests from people on phones, and a hero
  that costs three seconds of load time works against the goal. `goals.md` wins arguments.

---

## 0:53-0:58 · Buy the domain and go live (5 min)

The segment that turns the project into a business asset. Protect these five minutes.

### Buy it

Two honest routes:

| Route | What happens | Trade off |
|---|---|---|
| **Buy inside Vercel** | Search and buy from the dashboard. Nameservers are configured automatically, renewals and the SSL certificate are handled | Fewer moving parts, and the fastest thing to do live. Your registrar and your host are the same company |
| **Buy at an independent registrar** | Buy, then point the nameservers or add the records Vercel gives you | More control and portability, one more step, and a DNS wait |

Expect a `.com` to cost roughly **10 to 20 USD per year**. The exact figure is whatever the
registrar shows on the day, and it is not the same for every extension. **Do not quote a
price on stream without reading it off the screen.**

> "Renewal is the part people get wrong. A domain is rented, not bought. Turn on auto renew
> in front of the class. A client's site going dark because a card expired is the single
> most embarrassing failure in this business."

### Attach it and prove it

```
Get this live on the domain:
  1. Add the domain to the Vercel project.
  2. Set up both the apex and the www version, and redirect one to the other. Tell me
     which one you made canonical and why.
  3. Confirm the certificate is issued and https works with no warning.
  4. Update anything in the site that has the old preview URL hardcoded: the LocalBusiness
     schema, the canonical tag, the sitemap, any absolute links.
  5. Commit and push.

Then prove it. Open the live https URL and show me the response, not a summary.
```

**End on the domain, open on a phone, on camera.**

---

## 0:58-1:00 · Close (2 min)

| | |
|---|---|
| What today added | A hero that moves, a design with a stated reason, and an address you own |
| What it cost | About 2 USD of media generation, and 10 to 20 USD a year for the domain |
| What is now permanent | Two skills. Every future project gets images and video without re-learning any of this |

**The honest caveat, said plainly:**

> "A video hero is not automatically better than a photograph. It is better when the motion
> shows something a still cannot, which for a fence is depth, length, and how the light
> sits on the boards. If the clip does not do that, a sharp photograph beats it, loads
> faster, and never warps a post. Use the tool when it earns its file size."

---

# Homework

1. Run `/media-models` once. Bring the briefing and name the cheapest model that would do
   your hero, and what you give up by using it.
2. Generate three hero clips from three different photographs. **Bring the worst one** and
   say what the model got wrong.
3. Build a `design-brief.md` from five screenshots for a business that is not a fence
   company, and apply it.
4. Buy a domain. Any domain. Put something on it.
5. Get Lighthouse mobile performance above 90 **with the video in the hero**. This is the
   hard one, and it is where the real learning is.
6. Add up what you spent on media generation this week from your `spend-tracker` log, and
   work out what you would charge a client for the same work.

# Peer quiz

**Multiple choice (5):** which model family animates a photo you already have · what unit
video models are priced in · why `.env` goes in `.gitignore` before the key goes in `.env`
· what `playsinline` and `muted` are for · what happens to a domain you do not renew.

**Open, peer scored (4):**
- *Your image to video clip got something wrong. What, and why is that failure predictable?*
- *Paste three rules from your `design-brief.md`. Why are they rules and not adjectives?*
- *What did one five second clip cost you, and how did you know before you ran it?*
- *Apex or www, which did you make canonical, and what breaks if you do neither?*

**Peer exercise:** open a peer's domain on your phone on cellular data, not wifi. Time how
long until you can read the headline. Report the number.

---

# Prep checklist

## Must verify before the stream

- [ ] **fal endpoint ids and prices for all four families.** They move. Re-run
      `/media-models` the morning of the stream and read from that briefing
- [ ] **Whether fal publishes a machine readable model catalogue endpoint.** The skill
      brief tells the agent to say so if it cannot verify one. Confirm which branch it
      takes so it is not a surprise on camera
- [ ] Vercel domain purchase flow end to end, including what the confirmation screen looks
      like, so nothing personal is on screen when it appears
- [ ] `brew install ffmpeg` on a clean Mac, timed. If it is slow, move it into the student
      guide as a before you arrive step
- [ ] The reduced motion toggle path in the current macOS version

## Assets

- [ ] fal account with about 10 USD of credit and a spend limit set
- [ ] The Module 2 fence site, committed and deployed, in a known good state
- [ ] The host's real fence photos from `Projects/Fence4U/assets` staged in
      `assets/photos`, high resolution, one obvious hero candidate
- [ ] **A pre generated hero clip in reserve.** If the live generation fails or comes back
      warped twice, cut to this one and keep the segment moving
- [ ] Five design reference screenshots already captured, in case the live capture stalls
- [ ] A domain shortlist checked for availability that morning, and a payment method ready
- [ ] Both briefs in the student guide as copy pasteable blocks

# Open questions

1. **Does the domain get bought live, or pre bought and attached live?** Buying live is the
   better television and puts a payment screen on stream. Recommend buying live with a
   dedicated card and the browser window cropped.
2. **Two dollars of fal credit as an attendance barrier.** Flag it in the Luma description
   or absorb it by showing the generation and letting students watch? Recommend flagging
   it. Every module after this one needs the same credit.
3. **Is 60 minutes enough for both the media half and the design half?** The design segment
   is the one that would grow. If this overruns twice, split design by example into its own
   module and give this one the whole hour for media.
4. **Should `media-gen` also cover text to speech and music?** It would fit the same shape.
   Recommend no for now. Two families done properly beats four done thinly.

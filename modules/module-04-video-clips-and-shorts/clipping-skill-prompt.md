# Podcast Clipping: Research and Build Prompt
### The reusable brief for turning a 60 minute episode into vertical shorts

**Researched:** 2026-08-15
**Goal:** Work out the best way to build a skill that takes a 60 minute podcast video, transcribes it, finds the strongest 30 to 90 second moment, and renders a vertical 9:16 short that is captioned, watermarked, and framed on whoever is speaking.
**Scope:** Local macOS tooling plus the Gemini API. Built and tested as the `clipping-skill` at `~/.claude/skills/clipping-skill/`.

---

## TL;DR, the five things that matter

1. **The clip selection is the whole product, and it is a reading task, not a scoring model.** Every automated clipper sells a virality score. What actually decides whether a clip works is whether it opens on a hook, develops one idea, and lands. An agent reading a timestamped transcript against an explicit rubric does this better than a number, and you can argue with it.
2. **Boundaries must be sentences, never seconds.** Whisper gives word level timestamps. Group them into sentences, choose the clip by sentence index, and a clip can never start or end mid thought. This one constraint removes most of what makes automated clips feel broken.
3. **Active speaker detection does not need a dedicated model here.** TalkNet-ASD and LR-ASD are the published open source options and both want PyTorch plus pretrained weights. Gemini takes video and audio in one call, which is the same signal, and it costs about 300 tokens per second of video. Run it on the 60 second clip, not the 60 minute episode: 20k tokens versus roughly 1.1M.
4. **This is the finding most likely to change the plan: the source resolution decides whether a close up is even possible.** A full height 9:16 crop of a 1080p frame is already a 1.78x enlargement to reach 1080 wide, before any zoom. A 4K master reframes beautifully. A 720p screen recording cannot be reframed at all. Build the sharpness budget in as an explicit number and let it refuse.
5. **Cut between speakers, do not pan.** The slow automated glide between two seated people is the most recognisable tell of a machine made clip. A hard cut on a speaker change is what a human editor does, it is free, and it needs a minimum turn length so a 0.8 second "right, yeah" does not trigger a camera move.

---

## 1. The pipeline

```
episode.mp4
   │
   ├─ 1. transcribe          caption-video --srt-only     word level timings
   ├─ 2. sentence index      sentences.py --list          numbered, timestamped
   ├─ 3. choose the clip     you read it, against a rubric
   ├─ 4. lock boundaries     sentences.py --window        exact start and end
   └─ 5. render              clip.py
                              ├─ who speaks when      Gemini, video plus audio
                              ├─ where is their body  Gemini, one frame per turn
                              ├─ crop maths           plain Python
                              ├─ segment and concat   ffmpeg
                              ├─ watermark            ffmpeg overlay
                              └─ captions             caption-video
```

Steps 1 and 2 run once per episode. Steps 3 to 5 run once per clip.

## 2. The decisions, and what each one costs

| Decision | Chosen | Rejected | What it costs |
|---|---|---|---|
| Clip selection | Agent reads a sentence index against a written rubric | An LLM virality score, 0 to 99 | No single number to sort by. You have to read the shortlist. |
| Boundaries | Sentence indices from word timestamps | Fixed length windows, or raw seconds | Clip length is decided by the idea, so you cannot promise exactly 60 seconds. |
| Speaker detection | Gemini on the clip window | TalkNet-ASD or LR-ASD locally | Per clip API cost, and a network dependency. |
| Body location | Gemini box on one still frame per turn | Per frame tracking with YOLO plus MediaPipe | One framing per turn, not continuous tracking. Fine for a static podcast set. |
| Crop geometry | Computed in Python from the box | Asking the model for a crop rectangle | More code. In exchange it is reproducible and every number is adjustable. |
| Camera moves | Hard cut on speaker change | Smoothed pan and zoom path | No cinematic drift. This is the point. |
| Render | One segment per shot, concatenated | Single pass with `sendcmd` driving the crop | More encodes. Needed because changing crop width mid stream changes the output frame size. |

## 3. The framing rule

"Full body close up" resolves to this: the speaker fills the phone screen rather than floating in a wide room.

| Parameter | Default | Meaning |
|---|---|---|
| `--fill` | 0.82 | Fraction of the crop height the body occupies |
| `--headroom` | 0.09 | Space above the head, as a fraction of the crop |
| `--body-fraction` | 0.75 | When the body is visible past the knees, keep this much of it, roughly head to thigh |
| `--max-upscale` | 2.5 | How far a crop may be enlarged to reach 1080 wide |
| `--min-turn` | 2.0s | Turns shorter than this never trigger a reframe |

Measured against six source shapes on 2026-08-15:

| Source | Subject | Crop | Upscale | Body fills |
|---|---|---|---|---|
| 3840x2160 | standing, full body | 856x1520 | 1.26x | 91% |
| 3840x2160 | small and distant | 432x768 | 2.50x | 82% |
| 1920x1080 | guest in a two shot | 608x1080 | 1.78x | 80% |
| 1280x720 | webcam inset | 404x720 | 2.67x | 42% |

The last row is the honest caveat. The budget refuses to zoom into 720p, so a screen recording with a small webcam inset produces a wide, soft clip. Record the source at 1080p minimum, and 4K if the clip matters.

## 4. The clip selection prompt

Paste this with the output of `sentences.py --list`. This is the prompt that does the work.

```
Below is a full podcast transcript as a numbered sentence index. Each line is:

    [index] H:MM:SS-H:MM:SS (duration) text

'<<' marks a sentence preceded by a second or more of silence.

Find the strongest self-contained moments for vertical short-form video.

Rules for every candidate:
- Between 30 and 90 seconds long. Verify by subtracting the timestamps.
- Starts and ends on a sentence boundary. Give me sentence INDICES, not seconds.
- The first sentence must work as a hook with zero setup: a claim, a number,
  a contradiction, or the opening of a story.
- The last sentence must land: a conclusion, a punchline, or a reversal. Not a
  drift into the next topic.
- One idea, developed. Not a summary of several.
- No opening pronoun whose referent sits outside the window.
- If the moment only makes sense with the host's question, include the question.

Return 5 candidates, best first, as a table:

| # | Sentences | Start | End | Secs | Why it works | What it costs |

'Why it works' is one sentence naming which of these it is: story with a turn,
number with a consequence, disagreement, crisp definition, reversal, confession.
'What it costs' is the honest weakness: needs context, weak landing, slow
opening, niche.

Then, below the table, name which single candidate you would post first and
say what makes it beat the others. Do not hedge across several.
```

## 5. The build prompt

The brief that produced the skill. Paste it into an agent to rebuild, port, or adapt it to another brand.

```
Build a Claude Code skill called clipping-skill that turns a long podcast or
interview video into finished vertical 9:16 short-form clips.

Input: one video file, typically 60 minutes, 16:9, one or two people seated.
Output: a 1080x1920 mp4 of a 30 to 90 second moment, captioned, watermarked,
and framed on whoever is speaking.

Build it as three scripts plus a SKILL.md and two reference files.

Script 1, sentences.py
  Read the word-level transcript sidecar that the caption-video skill writes
  (<video>.words.json, a flat list of {text, start, end}).
  Group words into sentences. A sentence ends on terminal punctuation OR on a
  silence gap longer than 0.65s, because whisper drops full stops constantly in
  conversational speech. Do not treat abbreviations like "Dr." as sentence ends.
  Modes: --list prints every sentence numbered and timestamped, marking any
  sentence preceded by 1s+ of silence as a clean entry point. --window FIRST
  LAST reports the exact span for a sentence range. --snap START END widens
  rough seconds out to whole sentences. Every mode also has --json.
  Report the duration against the 30 to 90 second short-form window.

Script 2, track_speaker.py
  Produce a crop plan for a [start, end] window: a list of shots, each with a
  pixel-exact crop rectangle in the source frame.
  Pass 1: re-encode the window to a 480px 8fps proxy, upload it to the Gemini
  File API, and ask for a speaker-turn timeline plus a one-line visual
  description of each person. The speaker is whoever's LIPS ARE MOVING, not
  whoever is loudest. Use "none" when nobody on screen is speaking. Require the
  turns to tile the window with no gaps.
  Pass 2: for each turn, pull one full-resolution frame at the turn midpoint and
  ask Gemini for that person's body box, head top, and how far down the body is
  visible. Bounding boxes are [ymin, xmin, ymax, xmax] normalised to 0-1000.
  Then compute the crop in Python, never in the model:
   - if the body is visible past the knees, trim the bottom to 0.75 of
     head-to-bottom, giving a medium shot
   - crop height = body height / fill, fill defaults to 0.82
   - width follows the 9:16 aspect; if the person is broader, widen by at most
     1.25x, then stop and let an elbow clip
   - clamp to the upscale budget: a crop may be enlarged at most --max-upscale
     (2.5) to reach the output width. Derive this from the SOURCE resolution so
     4K masters get more room to push in than 1080p ones
   - position the head at --headroom (0.09) of the crop height below the top,
     centred horizontally on the person
   - if the crop was forced LARGER than the framing wanted, by the upscale
     budget or a small subject, centre the body in it instead of applying
     headroom, otherwise the person gets pinned to the bottom edge with dead
     air above
  Merge turns shorter than --min-turn (2.0s) into their neighbour, and merge
  consecutive shots whose crops land within 3% of each other.
  Every failure path degrades to one static centre crop and exits 0. Missing
  API key, missing package, model error, unparseable response: all of them
  still produce a usable plan.

Script 3, clip.py
  Render the plan. Cut the window once accurately at constant frame rate.
  Extract that window's audio once. Render one VIDEO-ONLY segment per shot with
  its own crop, all scaled to 1080x1920 with identical encoder settings.
  Concatenate with the concat demuxer and -c copy. Mux the single continuous
  audio track back on, folding the logo overlay into the same pass. Then call
  caption-video to burn karaoke captions.
  Audio is extracted once and laid back on rather than cut per segment, so the
  speech stays gapless no matter how many times the framing cuts.
  Flags: --dry-run to print the plan and stop, --plan to reuse or hand-edit a
  plan with no API calls, --static for one framing throughout, --no-vision for
  a pure centre crop, --logo-corner, --no-captions, --no-watermark.

Captions: use the caption-video skill with a vertical config. Big type, 16
character wrap, two lines, and margin_v 400 so the block sits at roughly y=1180
to y=1520 on a 1920 canvas. That clears the TikTok and Reels bottom bar and the
right-hand action rail.

Watermark: default to TOP-LEFT, not bottom-left. The bottom-left corner of a
TikTok or Reel carries the username, caption and music ticker, so a logo there
gets covered.

Camera behaviour: when the speaker changes, CUT. Never pan or glide. The slow
automated pan between two seated people is the most recognisable tell of a
machine-made clip.

Document two reference files: one for the clip selection rubric, one for the
crop maths, the cut-don't-pan rule, safe zones, and every failure mode with its
degradation path. State the trade-off next to every default.
```

## 6. Running it

```bash
# once per episode
python3 ~/.claude/skills/caption-video/scripts/caption_video.py episode.mp4 --srt-only --model small
python3 ~/.claude/skills/clipping-skill/scripts/sentences.py episode.words.json --list

# once per clip
python3 ~/.claude/skills/clipping-skill/scripts/sentences.py episode.words.json --window 143 152
python3 ~/.claude/skills/clipping-skill/scripts/clip.py episode.mp4 \
    --start 742.1 --end 806.4 --out clips/the-hook.mp4
```

Watch the first clip before rendering the rest.

## What is confirmed vs inferred

**Confirmed, verified against a primary source or a live run on 2026-08-15**

- Gemini samples video at 1 frame per second by default and costs about 258 tokens per frame plus 32 tokens per second of audio, roughly 300 tokens per second of video. Inline video is capped under 100MB and about a minute; the File API takes 2GB free or 20GB paid. Google AI for Developers docs.
- Gemini image bounding boxes are `[ymin, xmin, ymax, xmax]` normalised to 0-1000. Google AI for Developers docs.
- Bounding boxes over video with timestamps are not a documented capability. This is why the skill uses a still frame per turn instead.
- `gemini-3.7-flash` and `gemini-2.5-flash` are both live on the Generative Language API, listed from the models endpoint on this machine.
- ffmpeg's `crop` filter accepts runtime commands on `w`, `h`, `x` and `y`, confirmed by `ffmpeg -h filter=crop` on ffmpeg 7.1.1.
- The framing numbers in section 3 come from running the crop function against those six source shapes.
- The full pipeline was rendered end to end: a 35 second single-shot clip with captions and watermark, and a 24 second three-shot clip exercising the concat and audio mux path. Both produced 1080x1920 output at the exact requested duration.
- TalkNet-ASD (ACM MM 2021) and LR-ASD (IJCV 2025) are the published open source active speaker detection models. LR-ASD reports 94.1% mAP on AVA-ActiveSpeaker. Neither has PyTorch installed on this machine.

**Inferred or secondhand**

- Safe zone numbers, roughly 320px clear at the bottom and 120px on the right for TikTok, come from creator guides rather than a platform specification. Platforms change these without notice.
- The clip selection rubric is a judgement, assembled from what commercial clippers claim to weigh, hook strength, pacing and topic shifts. It has not been A/B tested against posted performance.
- Opus Clip's virality score is described by its own documentation. The internal method is not published.

**Open questions, not researched**

- Current Gemini per-token pricing was not checked, so per-clip cost in USD is unknown. Check before a large batch.
- Whether cutting on speaker change actually outperforms a smooth pan on retention. This was chosen on editing craft, not data.
- Whether a local TalkNet or LR-ASD pass would beat Gemini on turn accuracy for overlapping speech, where two people talk at once.

## Sources

**Active speaker detection**
- [TalkNet-ASD](https://github.com/TaoRuijie/TalkNet-ASD)
- [LR-ASD](https://github.com/Junhua-Liao/LR-ASD)
- [LR-ASD, IJCV 2025](https://link.springer.com/content/pdf/10.1007/s11263-025-02399-2.pdf)

**Gemini API**
- [Video understanding](https://ai.google.dev/gemini-api/docs/video-understanding)
- [Image understanding and bounding boxes](https://ai.google.dev/gemini-api/docs/image-understanding)

**Reframing and clip selection**
- [auto-vertical-reframe, YOLO plus MediaPipe pipeline](https://github.com/KazKozDev/auto-vertical-reframe)
- [Opus Clip virality score](https://help.opus.pro/docs/article/virality-score)

**Safe zones**
- [Aspect ratios and safe zones for Shorts, Reels and TikTok](https://syllaby.io/blog/aspect-ratios-safe-zones-shorts-reels-tiktok/)
- [Safe zone guide 2026](https://kreatli.com/guides/safe-zone-guide)

## Related notes

- The skill lives at `~/.claude/skills/clipping-skill/`, with the rubric in `references/clip-selection.md` and the crop maths in `references/reframing.md`.
- Upstream of this: `podcast-factory` produces a `<slug>-clips.md` shortlist from an episode. When that file exists, read it and skip straight to locking boundaries.
- Sibling skill: `vlogs-clipping` does the same job for single-speaker vlogs under the Konrad Gnat brand, with one static speaker-centred crop and no turn detection.

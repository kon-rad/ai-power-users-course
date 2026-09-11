# Module 1 V2 Image Prompts

**Two images.** A square 1:1 cover for Luma, and a wide 16:9 "starting soon" holding screen
for the YouTube livestream.

**Style:** late 1990s Japanese cel anime. The reference points are Cowboy Bebop for the
warmth and the character work, Akira and The Animatrix for the neon night city and the hard
rim light, and a kawaii register for the robot specifically. This is a deliberate step away
from the golden hour tropical terrace of Modules 1 to 3. The theme of this module is a
machine that keeps working after you go to bed, so the scene is **night**, and the one warm
light in it is the desk.

**Continuity that is kept:** the same avatar character, the same white robot introduced in
Module 1, the Argo hexagon emblem, and the same rendered text conventions as Module 2.

---

## Three rules

1. **Pass the Argo logo in as a reference image, do not describe it in words alone.** The
   scene prompt asks for the glowing hexagon emblem and the generator is given
   `logo-icon.png` as a reference so the geometry is right. The **wordmark** is never drawn.
   It is composited afterwards as a watermark into the calm corner the prompt reserves.
2. **Text is rendered by the model, following the Module 2 convention.** Module 0 rendered
   text separately in a Python compositing step. Module 2 asked the image model for it and
   the result was cleaner. **Newer wins.** Every string below must be spelled exactly.
3. **Two characters, both clearly visible.** The avatar is the host and the robot is the
   sidekick. Module 0's house rule that the avatar is never the star was written for the
   solar punk event covers. Module 2 overrode it for course images and that override stands.

---

## The cast, described once and reused by both prompts

Paste this block into both prompts. It is what keeps the series looking like one set.

```
THE HOST, the anime avatar: a man in his late thirties, short dark hair under a
BACKWARDS black baseball cap, light stubble, blue green eyes, calm half smile,
wearing a plain black polo. Late 1990s cel anime character art: clean confident
linework, flat shadow shapes rather than soft gradients, a hard cyan rim light down
one side of his face from the monitor and a warm amber key light from the desk lamp
on the other. He is seated at the desk, three quarter view, one hand resting on a
laptop keyboard, the other holding a phone up so the screen faces slightly toward
the viewer.

THE ROBOT, small and kawaii and clearly a real desk object rather than a mascot: a
compact white matte plastic desktop robot, roughly the size of a table lamp, sitting
on the desk near the laptop. A smooth rounded egg shaped head and body. Two LARGE
round black camera lens eyes joined by a single dark horizontal bar across the face,
which gives it a friendly goggle look. Two very thin flexible antennae rising from
the top of the head on small coil springs, each with a tiny coloured charm tied to
it. A short black articulated neck joint with visible cable routing between head and
body. The white body is covered in small overlapping vinyl stickers, the way a
laptop lid is. No arms, no legs. It is tilted slightly toward the laptop screen,
watching it, and one antenna is bent as if it just moved.
```

---

# Image 1: Luma cover, square 1:1

## Scene prompt

```
Create a 1:1 square event cover illustration for an online class, in the style of a
late 1990s Japanese cel animated film. Hand painted background, visible cel shading
with hard edged shadow shapes, clean confident linework, rich saturated colour,
strong film grain, subtle chromatic aberration at the frame edges. Think Cowboy
Bebop character warmth with Akira and Animatrix night city lighting.

SETTING: a small apartment room at 2am, seen from slightly above and behind the
desk. Rain streaks on a large window that fills the upper right, and beyond it a
dense neon night city, hand painted, out of focus: magenta and cyan signage, vertical
Japanese style signboards reduced to abstract glowing shapes with no readable
lettering, a distant elevated road, aircraft lights. The room itself is dark except
for three light sources: a warm amber desk lamp, the cyan glow of a laptop screen,
and one thin strip of green from a rack of small machines.

[PASTE THE CAST BLOCK HERE]

THE DESK, lower half of the frame: an open modern laptop, screen glowing cyan,
showing a simple two pane layout. The left pane is a list of plain text notes in a
sidebar tree. The right pane is a terminal, monospaced amber text on near black, with
one blinking cursor. Beside the laptop, a ceramic mug with steam, a pair of over ear
headphones lying flat, and a small stack of paper notes.

THE SERVER, upper left, floating in the dark air like painted light rather than a
science fiction hologram: a single small rack mounted server unit rendered as a
glowing wireframe box, cool green and teal, with a thin luminous line arcing from it
down across the room to the phone in the host's hand. The line is made of tiny
drifting characters, text in flight. This is the image's one piece of magic and it
should read as a diagram drawn in light, not as a special effect.

THE PHONE: the screen shows a chat interface, two speech bubbles, one right aligned
and one left aligned, no readable text inside them, glowing softly blue.

BRANDING: a softly glowing golden hexagon emblem, a six sided ring made of angular
faceted gold segments around a dark hexagonal centre, exactly like the reference
image provided, floats small in the air just above the laptop as a holographic mark.
Keep it small and secondary. It is a mark in the scene, not a logo pasted on top.

PALETTE: deep indigo and near black for the room, magenta and cyan from the window,
warm amber and ochre from the desk lamp, cool green from the server graphic. High
contrast. Deep blacks.

COMPOSITION FOR SQUARE: vertical stacking. Title text occupies the top 22 percent of
the frame against the darkest part of the night sky. The window and the floating
server graphic sit in the upper middle. The host, the robot and the laptop fill the
centre and lower middle. Leave the LOWER RIGHT CORNER visually calm and uncluttered,
plain dark wall or soft shadow, no objects and no detail. A watermark is placed there
afterwards.
```

## Text to render, square

```
TEXT RENDERING, critical. Render every string exactly as written, spelled correctly,
with no extra words and no invented lettering anywhere else in the image. Typeface: a
bold rounded friendly sans serif for the title, in warm cream ivory (#F7EFDC) with a
soft dark outline and a gentle drop shadow so it stays readable over the night sky.
All text sits inside the central 88 percent of the frame. Do NOT add any other
lettering, watermark, logo text, signage or interface labels anywhere in the image.

Large title across two lines at the top:
  "Your Own AI Agent"
  "and Second Brain"

Smaller subtitle line directly beneath the title, single line:
  "Build it, then put it on a server that never sleeps"

A single small caption line along the very bottom edge, smaller and lighter:
  "AI POWER USERS · MODULE 1 V2 · LIVE ON YOUTUBE · Hosted by Konrad Gnat · Argo"
```

---

# Image 2: YouTube livestream holding screen, wide 16:9

Same world, recomposed horizontally, with more room at the sides. This screen is reused
across the session and during any break, so **no date is rendered on it, ever.**

## Scene prompt

```
Create a 16:9 widescreen "starting soon" holding screen for a live YouTube class
stream, 1920x1080 feel, in the style of a late 1990s Japanese cel animated film. Hand
painted background, visible cel shading with hard edged shadow shapes, clean
confident linework, rich saturated colour, strong film grain. Cowboy Bebop character
warmth, Akira and Animatrix night city lighting.

SETTING: a small apartment room at 2am, cinematic horizontal composition, read left
to right. The desk and the two characters occupy the centre and centre left. A large
rain streaked window fills the right third, and beyond it a dense neon night city,
hand painted and out of focus: magenta and cyan signage reduced to abstract glowing
shapes with no readable lettering, a distant elevated road, aircraft lights. The room
is dark except for a warm amber desk lamp, the cyan glow of the laptop screen, and a
thin strip of green light.

[PASTE THE CAST BLOCK HERE]

CRITICAL, THE HOST MUST BE FULLY VISIBLE. He is the main subject. Show him from the
chest up, seated centre left, face clearly visible and turned toward the viewer,
occupying roughly the middle third of the frame, at least as large and as prominent
as the robot. Do NOT crop him, hide him behind the laptop, or replace him with the
robot. Two characters: the host on the left of the pair, the robot on the desk to his
right.

THE DESK: an open modern laptop between them, angled so the glowing cyan screen is
readable but does not cover the host's face or chest. The screen shows a simple two
pane layout, a sidebar tree of plain text notes on the left and a terminal of
monospaced amber text on near black on the right, with one blinking cursor. A ceramic
mug with steam and a pair of over ear headphones rest nearby.

THE SERVER, far left of the frame, floating in the dark air like painted light rather
than a science fiction hologram: a single small rack mounted server unit rendered as
a glowing wireframe box in cool green and teal, with a thin luminous line arcing from
it rightward across the whole frame to the phone in the host's hand. The line is made
of tiny drifting characters, text in flight. It should read as a diagram drawn in
light, not a special effect.

THE PHONE: held up in the host's other hand, screen angled toward the viewer, showing
a chat interface with two speech bubbles, one right aligned and one left aligned, no
readable text inside them, glowing softly blue.

BRANDING: a softly glowing golden hexagon emblem, a six sided ring made of angular
faceted gold segments around a dark hexagonal centre, exactly like the reference
image provided, floats small in the air above the laptop as a holographic mark. Small
and secondary.

PALETTE: deep indigo and near black for the room, magenta and cyan from the window,
warm amber and ochre from the desk lamp, cool green from the server graphic. High
contrast, deep blacks.

COMPOSITION FOR WIDESCREEN: title text runs across the top. Leave the LOWER LEFT
CORNER visually calm and uncluttered, plain dark wall or soft shadow, no objects and
no detail, because a watermark goes there. YouTube safe margins: nothing important
within 8 percent of any edge.
```

## Text to render, landscape

```
TEXT RENDERING, critical. Render every string exactly as written, spelled correctly,
with no extra words and no invented lettering anywhere else in the image. Typeface: a
bold rounded friendly sans serif for the title, in warm cream ivory (#F7EFDC) with a
soft dark outline and a gentle drop shadow. All text sits inside the central 88
percent of the frame. Do NOT add any other lettering, watermark, logo text, signage
or interface labels anywhere in the image.

Large title across the top, two lines, centred:
  "Your Own AI Agent"
  "and Second Brain"

Directly beneath the title, a single smaller line:
  "AI POWER USERS · MODULE 1 V2 · macOS and Windows 11"

A prominent short line lower in the frame, in warm amber, clearly separate from the
title, reading exactly:
  "STARTING SOON"

A single small caption line along the very bottom edge:
  "LIVE ON YOUTUBE · Hosted by Konrad Gnat · Argo · Private AI Journal"

Do not render any date anywhere in the image.
```

---

## Reference images to pass to the generator

Pass all three, in this order. The Module 2 generator passed two and the robot came out
generic, which is the one thing worth changing.

| # | File | What it fixes |
|---|---|---|
| 1 | `Areas/argo/ai-power-users/images/module-02-luma-square.jpg` | Character art style and the avatar's face, so the series reads as one set |
| 2 | `Areas/argo/assets/friendly-robot.png` | **The actual robot.** The sticker covered white body, the eye bar, the spring antennae. Words alone produce a generic humanoid |
| 3 | The Argo hexagon logo icon | The emblem geometry |

> **The logo icon path in `gen_module2.py` is stale.** It points at
> `/Users/konradgnat/dev/notes/PROJECTS/argo/design/logo-icon.png`, and that tree moved. The
> watermark assets now live in `Areas/argo/assets/`. **Find the current icon before running
> anything**, and fix the constant rather than letting it fail silently. This is the same
> class of bug recorded in `planning/module-00-image-prompts.md`.

## Watermark

Composited after generation, never drawn by the model.

- Asset: `Areas/argo/assets/argo-watermark-ink@2x.png`
- Square: **lower right**
- Landscape: **lower left**
- Treatment: cream `#FAF4E6` fill plus a blurred dark drop shadow at 88 percent opacity.
  This is the treatment Module 2 landed on after a flat low opacity mark turned out to be
  invisible against both dark and light areas. On a night scene the shadow matters less, but
  keep it so the two modules match.

## Generating

Adapt `Areas/argo/ai-power-users/images/gen_module2.py`. Change the two prompt constants,
the `JOBS` list, the reference image list, and `OUT`.

| Setting | Value |
|---|---|
| Model | `gemini-3-pro-image-preview` |
| Size | `2K` |
| Aspect | `1:1` for the Luma cover, `16:9` for the holding screen |
| Output | `Areas/argo/ai-power-users/images/` |

Save the pre watermark originals as `*-raw.jpg`. Re-compositing needs them.

Expected files:

| File | Size | Use |
|---|---|---|
| `module-01-v2-luma-square.jpg` | 2048 x 2048 | Full resolution square master |
| `module-01-v2-luma-square-1080.jpg` | 1080 x 1080 | **The Luma event cover** |
| `module-01-v2-youtube-loading.jpg` | 2752 x 1536 | Full resolution landscape master |
| `module-01-v2-youtube-loading-1920.jpg` | 1920 x 1080 | **The OBS holding screen** |

> If a run returns no image at all it was a blocked response rather than an error. Reword
> the neon city description, which is the part most likely to trip a filter, and retry.

## Check before shipping

- [ ] Every rendered string is spelled correctly, including "Konrad Gnat" and "AI POWER USERS"
- [ ] No date anywhere on the landscape image
- [ ] No invented lettering in the neon signage, the laptop screen, or the phone
- [ ] The robot has the eye bar and the two spring antennae, not a generic humanoid face
- [ ] The host's cap is on backwards
- [ ] The watermark corner is genuinely empty in the raw render before compositing
- [ ] The square reads at thumbnail size on a phone. Luma cards are small
- [ ] The title is legible over the night sky. Night backgrounds are where cream text goes
      grey

# Module 1 V2 Web Page: Draft

**Status:** **DRAFT ONLY. Not built, not deployed.** Awaiting approval.
**Date:** 2026-08-18
**Target route:** `/courses/ai-power-users/module-1-v2` on myargoquest.com
**Precedent:** the live `/courses/ai-power-users/module-1` page, whose structure this mirrors
**Precedent for the plan format:** `planning/module-02-web-page-plan.md`

---

## What already exists, verified with curl on 2026-08-18

| URL | Status |
|---|---|
| `https://myargoquest.com/courses/ai-power-users` | 200 |
| `https://myargoquest.com/courses/ai-power-users/module-0` | 200 |
| `https://myargoquest.com/courses/ai-power-users/module-1` | 200 |
| `https://myargoquest.com/courses/ai-power-users/module-2` | 200 |
| `https://myargoquest.com/courses/ai-power-users/module-1-v2` | **404, this page** |

So the route pattern is settled and this page is a new sibling, not a new pattern.

## Access model

**Public.** Same as every other module page. This page's job is Luma registrations and
YouTube views, and gating it would defeat that.

## The one decision to make before building

**Does this page replace `/module-1`, or sit beside it?**

| Option | Consequence |
|---|---|
| **New route, both pages live** | Old links keep working. Two pages describe a session that only runs one way now, and the older one is wrong |
| **Replace `/module-1` in place, redirect nothing** | One truth. Anyone holding a link to the old Module 1 lands on the new one, which is what they want anyway |
| **New route, and `/module-1` redirects to it** | **Recommended.** One truth, and no dead links. Costs one redirect rule |

Recommendation: **build `/module-1-v2`, then 301 `/module-1` to it.** Keep `/module-0` where
it is, since Module 0's Windows material is now folded into this session and the page is
still an accurate record of a session that was actually run.

---

## Page structure

Mirrors the live Module 1 page, with three sections that are new.

| # | Section | Content |
|---|---|---|
| 1 | **Hero** | Badges: `Module 1 V2` · `Live on YouTube` · `macOS and Windows 11` · `No coding required`. Headline: **Setting Up Your Personal AI Agent and Second Brain.** Sub hook: the agent does not stop when you close your laptop. Three CTAs: **Register on Luma** · **Watch live** · **Follow the guide** |
| 2 | **The deliverable** | **New, and it is the section that sells the page.** A phone in frame showing the Telegram chat, beside a terminal window on the server the reply came from. Lead with the outcome, not the tool list |
| 3 | **What you build** | The diagram from the README, laptop on the left, rented server on the right, phone at the bottom. Rendered as an SVG or a styled block, not an image, so it works on mobile and in dark mode |
| 4 | **The four tools** | Four cards: Handy, Obsidian, VS Code, Hermes. Each with role, cost, open source status, and a link. **Four, not six.** Module 1's page listed six and two of them are gone |
| 5 | **The two accounts** | Two cards: OpenRouter and DigitalOcean, each with what it is for and **what it costs**, stated as a number. This is the first module in the series that costs money and the number belongs above the fold, not in a footnote |
| 6 | **Choosing a model** | **New.** The four filters in order, as a numbered list: tool support, context, price, speed. One callout explaining that an agent without tool support chats and never touches a file. **Do not name a model on this page.** It will be wrong within weeks and the page is not versioned |
| 7 | **The skill** | One card for `/standup`: the command, the cadence, what it does, why it matters. Plus the line about checking a skill name against the built in slash commands |
| 8 | **The six steps** | Numbered list from the README's six steps table |
| 9 | **Learning objectives** | The ten from the syllabus |
| 10 | **The 120 minute session** | The run of show as a table, four parts, fifteen rows. Horizontal scroll container on mobile |
| 11 | **Before you arrive** | The three installs, the Whisper model download warning, the two accounts, and the hardware line. Links out to the student guide for the detail |
| 12 | **What is private and what is not** | **New, and non negotiable.** The five row table from the student guide. It is what makes every other claim on the page credible, and no competing course has it |
| 13 | **Knowledge check** | 5 multiple choice plus the 4 open ended prompts, from `quiz.md` |
| 14 | **Homework** | The six items |
| 15 | **Course navigation** | Previous and next module cards, and the position in the series |
| 16 | **Footer CTA** | Register on Luma, repeated |

## Sections deliberately not included

- A pricing or upsell block. The course is free to attend and the page should not imply
  otherwise
- An email capture. Luma is the capture
- Testimonials. There are none yet, and inventing them is out of the question

---

## Hero copy

> **Badges:** Module 1 V2 · Live on YouTube · macOS and Windows 11 · No coding required

> **Headline:** Setting Up Your Personal AI Agent and Second Brain

> **Sub hook:** Two hours, live, from nothing installed to an AI agent running on a server
> you own that answers you on Telegram. The entry point to the course. No prior module
> required, and the first ten minutes are the terminal from scratch.

> **CTAs:** Register on Luma · Watch live on YouTube · Follow the student guide

## The deliverable section copy

> **You leave with a bot in your pocket.**
>
> Not a subscription. Not an app someone else runs. A Linux server you rent for six dollars
> a month, running an open source agent you installed, holding notes that live in a folder
> you own, reachable from your phone by text message. You reboot it live during the session,
> wait forty seconds, and message it again without touching anything.

## The private and not private section copy

> **Being precise about this is worth more than believing all of it is private.**

| Piece | Private |
|---|---|
| Speech to text, your voice | **Yes.** Never leaves your machine, works offline |
| Your notes on disk | **Yes.** Plain files in a folder you own |
| The model | **No.** Prompts pass through the provider |
| The server | **Yours, not private.** A rented computer on the public internet |
| The Telegram chat | **No.** Bot chats are not end to end encrypted |

> Running a model on your own hardware is a later session in this course, and it is a real
> answer rather than a consolation prize.

---

## Metadata

```ts
export const metadata: Metadata = {
  title:
    "Module 1 V2 · Setting Up Your Personal AI Agent and Second Brain | AI Power Users",
  description:
    "Two hours, live, from nothing installed to an AI agent running on a server you own " +
    "that answers you on Telegram. Speech to text, a PARA vault, your first agent skill, " +
    "and a bot on your phone. macOS and Windows 11. No coding required.",
};
```

**Open Graph image:** the phone showing the Telegram chat, cropped from the Luma square
cover so the page, the event and the thumbnail are visibly the same session. **Not a
terminal screenshot.**

---

## Copy source of truth

| Page section | Comes from |
|---|---|
| Hero, deliverable, honest caveats | `luma-description.md` |
| Tools, accounts, model filters, steps, objectives, run of show | `syllabus.md` |
| The ideas and the diagram | `README.md` |
| Quiz | `quiz.md` |
| Before you arrive | `student-guide.md` |

**Write the copy once.** If the page and the syllabus drift, the page is wrong. The syllabus
wins.

---

## Data to add

Following the shape the existing module pages already use:

```ts
export const MODULE_1_V2_AGENDA: AgendaRow[]    // 15 run of show rows, four parts
export const MODULE_1_V2_TOOLS: ToolCard[]      // Handy, Obsidian, VS Code, Hermes
export const MODULE_1_V2_ACCOUNTS: Prereq[]     // OpenRouter, DigitalOcean, with costs
export const MODULE_1_V2_STEPS: string[]        // the six steps
export const MODULE_1_V2_FILTERS: string[]      // the four model filters, in order
export const MODULE_1_V2_PRIVACY: PrivacyRow[]  // the five row private table
export const MODULE_1_V2_GUIDE_URL: string      // the GitHub student guide
export const MODULE_1_V2_LUMA_URL: string       // this event, not Module 1's
```

> **Per module event URLs.** `module-02-web-page-plan.md` flagged that the site had one
> global `LUMA_URL` and `YOUTUBE_URL` with a TODO on them. If that is still true, this page
> will point its Register button at the wrong event. **Check before building.** It is a two
> line fix and a silent, expensive failure if missed.

---

## Verification checklist

1. `next build` passes and lint is clean
2. `/courses/ai-power-users/module-1-v2` renders for logged out visitors
3. **The Luma and YouTube CTAs point at this module's URLs**, not Module 1's
4. The `/module-1` redirect lands here, and no other page links to a now dead route
5. Previous and next navigation works in both directions
6. Renders correctly in dark mode
7. Mobile at 375px, including the run of show table and the private table, both in
   horizontal scroll containers
8. Every outbound tool link returns 200
9. The Open Graph image renders in a link preview on at least one messaging app
10. The quiz interaction works

---

## Sequencing

**Nothing ships until the module content is locked and the module has run once.**

1. Syllabus, student guide, Luma copy, quiz drafted. **Done, this folder**
2. **Konrad reviews and locks the syllabus**
3. Push the module folder to GitHub, so the student guide URL resolves
4. Create the Luma event, get its URL
5. Schedule the YouTube stream, get its URL
6. **Run the module once**, then correct the run of show table from what actually happened
7. Build this page with the corrected times and the real screenshots
8. Add the `/module-1` redirect

> **Blocking dependency:** section 2, the deliverable, needs a real photograph of a real
> phone showing a real reply from a real server. A mockup there would undercut the one claim
> the page makes. Take that photograph during the dry run, not during the live session.

---

## Open decisions

1. **Redirect or coexist**, covered above. Recommending build new, then redirect `/module-1`.
2. **Does Module 0's page stay?** Recommending yes. Its Windows material is folded into this
   session, but the page is an accurate record of a session that ran, and it still serves
   anyone who wants the File Explorer and shortcuts material on its own.
3. **How is the module numbered in the course navigation?** "Module 1 V2" reads clearly on
   its own page and awkwardly in a list of Module 0, 1, 2, 3. Either renumber the series or
   accept the awkwardness. **This is a course structure decision, not a page decision**, and
   it should be made before the page is built rather than discovered in the navigation
   component.
4. **Whether the six dollar server cost belongs in the hero badges.** It is the single most
   likely reason someone bounces, and also the single most likely reason someone feels
   ambushed at minute four if it is buried. Recommending it goes in the accounts section
   with a number, and is named in the sub hook, but not in a badge.

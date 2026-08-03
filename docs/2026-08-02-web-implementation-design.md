# AI Power Users Class — Web Sub-Section Design

**Date:** 2026-08-02
**Status:** Approved
**App:** `apps/global-builders-club/web` (Next.js 16, App Router)

## Summary

Add an "AI Power Users" course as a mini-LMS sub-section of the Global Builders
Club website. A public overview and values page introduce the course; four
login-gated lesson pages (Day 1, Day 2, Day 3, Capstone) deliver the teaching
content. Day 1 is written in full as the template; Days 2–3 ship as structured
skeletons (objectives, section headings, fully-specified exercises) to expand
later. The capstone brief and peer-evaluation rubric are written in full.

Source material adapted from `PROJECTS/GlobalBuildersClub/ai-power-users-class/`
(`syllabus.md`, `brainstorm.md`). The six course **values** are authored fresh.

## Decisions (locked)

- **Page type:** Full course delivery (mini-LMS), not just a landing page.
- **Content model:** Hand-built TSX pages (metadata + structured lists live in a
  shared data module; lesson prose lives in the page components).
- **Access:** Login-gated via the site's existing SIWE auth. Overview + values
  public; the four lesson pages gated **server-side** (cookie read at render — the
  lesson body is never sent to logged-out visitors).
- **Content depth now:** Day 1 full prose; Days 2–3 structured skeletons; capstone
  + values full.

## The Six Values (culture layer)

Each renders as an icon card (name + one-line meaning + "how it shows up here").

1. **Win and help win** — Your success and your peers' are the same goal.
   *Graded partly on the feedback you give.*
2. **The Four Agreements** — Be impeccable with your word · Don't take it
   personally · Don't make assumptions · Always do your best. *Ground rules for
   every feedback circle.*
3. **Criticise in private, praise in public** — Corrections one-to-one;
   recognition to the whole room. *Peer evaluation uses private written notes.*
4. **Create more than you consume** — Ship things; don't just watch demos.
   *Every session ends with something you built.*
5. **Done over perfect** — A shipped rough capstone beats a perfect plan.
   *Capstone graded on a working demo, not polish.*
6. **Participate** — The class is a practice, not a lecture. *Labs, mini-teaches,
   and the feedback circle are all hands-on.*

## Information Architecture

```
/class            (public)  Overview
/class/values     (public)  The six values in full
/class/day-1      (gated)   Foundations, Models & Prompting — FULL PROSE
/class/day-2      (gated)   Multimodal, Retrieval & Automation — skeleton
/class/day-3      (gated)   Privacy, Self-Hosting & Capstone — skeleton
/class/capstone   (gated)   Full brief + peer-evaluation rubric
```

Navigation: add `{ href: "/class", label: "AI Class" }` to `Navigation.tsx`.

## Access Gating

New helper `src/lib/auth/serverSession.ts` → `getServerSession()` reads and
validates the `session` cookie via `next/headers` `cookies()` (same base64-JSON +
`expiresAt` check already used in `src/app/shop/purchases/page.tsx` and
`src/lib/auth/session.ts`).

Each lesson page is an `async` server component with
`export const dynamic = "force-dynamic"`. Flow:

```
const session = await getServerSession();
if (!session) return <LessonGate />;   // wallet connect + sign in; no lesson body
// ...render lesson
```

`LessonGate` is a client component wrapping the existing `ConnectButton`
(`useAuth().signIn`). This is a genuine gate: unauthenticated requests never
receive lesson HTML.

## Components (`src/components/class/`)

| Component | Kind | Purpose |
|---|---|---|
| `ClassHero` | client | Overview hero (ScrollReveal) |
| `ValueCard` / `ValuesGrid` | server | Value cards |
| `LessonLayout` | server | Day header, objectives, section wrapper, prev/next |
| `LessonSection` | server | Titled teaching block |
| `Objectives` | server | Learning-objectives checklist |
| `Exercise` | server | Typed callout: `lab` / `peer` / `homework` / `essay` |
| `RubricTable` | server | Capstone peer-evaluation rubric |
| `LessonGate` | client | Auth wall (ConnectButton) |
| `CourseNav` | server | Day list + prev/next (takes `currentSlug`) |

Visual language matches `/robotics` and `/about`: dark theme, purple-500 accents,
`bg-card border border-border rounded-xl`, lucide icons,
`ScrollReveal`/`StaggerContainer` reveals.

## Data Module (`src/lib/class/course.ts`)

Shared, prose-free structured data consumed by overview + lesson pages:

- `VALUES` — the six values.
- `DAYS` — `[{ slug, day, title, theme, duration, objectives[], summary }]`.
- `EXERCISES` — per-day labs / peer exercises / homework / essays.
- `RUBRIC` — six peer-evaluation criteria.
- `GRADING` — weighting table (labs 20 · homework 15 · essays 15 · peer 15 ·
  capstone 25 · peer-feedback 10).
- `TOOLKIT` — take-home resources.
- `CAPSTONE` — brief, the "3 of 6 capabilities" rule, deliverables, examples.

Lesson *prose* (Day 1) lives as JSX in the page component. Day 2–3 sections
render headings + short outline text + their `Exercise` cards from `EXERCISES`.

## Content Plan

- **Day 1 — full prose:** history of AI · what an LLM is · the agent loop ·
  tokens/context/temperature · model ecosystem (open/closed, families, modalities,
  reasoning) · prompt engineering + system prompts. Inline examples throughout.
- **Day 2 — skeleton:** image/video/audio models · speech-to-text · multimodal ·
  multi-language · face-swap ethics · RAG · agents & automation · MCP. Objectives +
  headings + exercises specified.
- **Day 3 — skeleton:** API-key/credential leaks · centralized data collection ·
  self-hosting (Ollama/LM Studio) · private & decentralized AI · cost management ·
  staying current. Objectives + headings + exercises specified.
- **Capstone — full:** brief, pick-3-of-6 rule, deliverable, examples, rubric.
- **Values — full** on `/class/values`.

## Out of Scope (future work)

- Day 2–3 prose expansion.
- Progress tracking / persistence / completion state.
- Enrollment, payment, or cohort management.

## Verification

No test framework in the app. Manual + tooling checklist:

1. `next build` passes; `eslint` clean.
2. Logged-out user visiting `/class/day-1|2|3|capstone` sees the gate and **no**
   lesson body in the HTML response.
3. Logged-in user sees full lesson content.
4. `/class` and `/class/values` render for everyone.
5. All inter-page links, `CourseNav` prev/next, and the new nav item work.
6. Pages render correctly in the site's dark theme.

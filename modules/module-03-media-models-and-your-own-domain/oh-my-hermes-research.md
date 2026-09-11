# oh-my-hermes (OMH) — the skill pack that sits on top of Hermes Agent
### What it offers, how it installs, and whether it belongs in AI Power Users

**Researched:** 2026-08-15
**Goal:** Decide whether `oh-my-hermes` is worth teaching, and to whom — with an accurate
picture of what its 100-odd skills actually do and what they cost in context.
**Scope:** The `rlaope/oh-my-hermes` repository at `main` on 2026-08-15 (release v1.0.6),
the upstream `NousResearch/hermes-agent` docs it depends on, and the "Hermes Agent
Masterclass" field note by [@nykdotdev](https://x.com/nykdotdev), 2026-07-06.

---

## TL;DR — the five things that matter

1. **OMH is a discipline layer, not a capability layer.** It does not give Hermes new
   powers. It gives Hermes a *vocabulary for how sure it is* — a four-rung evidence ladder
   that separates "a plan is ready" from "an executor said it finished" from "a test
   actually passed." That single idea is the whole product, and it is the one thing worth
   teaching even to students who never install it.

2. **The context cost is the finding most likely to change the plan.** Every installed
   skill's `SKILL.md` is carried into Hermes' routing context *on every turn*, not only
   when that workflow runs. `omh setup` installs a **core** profile (~9 skills) for exactly
   this reason; `--full` installs ~89 and emits a machine-readable
   `context_cost_warning`. A student who runs `--full` on day one makes their agent
   measurably dumber for every unrelated conversation — the context-rot lesson from
   [[research-2026-ai-best-practices]], live and self-inflicted.

3. **The eight `ulw-` workflows are the actual product surface.** Context, interview,
   research, plan, work, loop, QA, perf. Everything else — 94 more skills — is catalogue
   depth most people will never route to. Teach the eight; mention the rest exists.

4. **It is two months old, third-party, and largely agent-authored.** Created 2026-06-03,
   980 stars, MIT, maintained by one person (`@rlaope`) with two AI agents credited as
   collaborators. Upstream Hermes has 230,864 stars and is Nous Research's own. Those are
   different risk profiles and a beginner course should say so out loud.

5. **The nyk article and the repo agree on the core discipline and disagree on the
   numbers.** Both land on the same operating model: memory is *what the agent knows about
   you*, skills are *what it knows how to do*, cron is *detection*, and approval gates
   anything with money, reputation, or production on the line. But the article's "60+
   tools, 20+ messaging surfaces" does not match the Hermes docs' "40+ tools" and six
   gateway platforms — see [What's confirmed vs. inferred](#whats-confirmed-vs-inferred).

---

## 1. The two layers, kept straight

| | **Hermes Agent** | **oh-my-hermes (OMH)** |
|---|---|---|
| Who | Nous Research | `@rlaope` / Team Art & Engineering |
| What | The agent runtime: TUI, tools, memory, skills, cron, subagents, messaging gateway | A skill pack + CLI that governs *how* Hermes takes on work |
| Repo age | Created 2025-07-22 | Created 2026-06-03 |
| Stars / forks | 230,864 / 45,800 | 980 / 93 |
| Licence | MIT | MIT |
| Latest | rolling | v1.0.6, 2026-08-14 |
| Command | `hermes` | `omh` |
| Replaces the other? | — | **No.** Hermes stays the chat surface; OMH never executes code itself |

OMH's own framing: *"OMH is the operating layer above Hermes-native skills: it frames the
problem, picks the workflow and evidence gates, and runs native skills as capabilities
inside that governed path."* It is deliberately not an executor. Every handoff it prepares
is marked `prepared_not_observed` until something else records that the work ran.

The course already runs on Hermes — see [[course-overview]] and the `HERMES.md` step in
[[module-03-media-models-and-your-own-domain/README|this module's README]]. OMH is the
optional layer above it, not a replacement for anything students have already installed.

---

## 2. What it offers — the eight ultra-skills

You do not type these as commands. You say the trigger word in chat and Hermes routes.

| Workflow | What it does | Underlying skill | Role | Gate |
|---|---|---|---|---|
| `ulw-context` | Aligns the words a repo uses before planning; captures confirmed terms, interviews the next decision frontier | `context` | planner | clarity-gated |
| `ulw-interview` | One question at a time until scope, non-goals and acceptance criteria exist | `deep-interview` | planner | clarity-gated |
| `ulw-research` | Real code plus live web, citations kept, contested claims verified, output is a decision dossier | `research` | researcher | source-gated |
| `ulw-plan` | Consensus plan: options compared, risks named, done-criteria agreed, review gate before execution | `ralplan` | planner | reviewed-plan-gated |
| `ulw-work` | Splits an **accepted** plan into disjoint parallel lanes that never edit the same file | `ultrawork` | handoff-guide | handoff-gated |
| `ulw-loop` | interviewer → planner → researcher → builder → reviewer, cycling until a real gate passes | `loop` | planner | loop-gated |
| `ulw-qa` | Attacks the build with hostile scenarios and fixes what breaks | `ultraqa` | reviewer | scenario-gated |
| `ulw-perf` | Measures where it is actually slow, leaking or expensive, then fixes one hot path behind a regression budget | `ultraperf` | tracker | measurement-gated |

The interesting design detail is the **"Do not use when"** block every skill carries. `ulw-plan`
refuses ambiguous requests and points at `ulw-interview`. `ulw-perf` refuses when the metric
and baseline are already declared and points at `performance-goal`. The catalogue is built to
route *away* from itself — which is the opposite of how most skill packs behave.

### The rest of the catalogue

102 skill directories ship in the repo at `main`; the docs describe a `--full` install as
"~89 skills, growing over time." They are grouped for users into six **capability families**:

| Family | What it covers |
|---|---|
| Plan and decide | Clarify goals, prepare plans, make loop and decision paths explicit |
| Learn and gather | Sources, papers, signal triage, source-backed briefs |
| Retain knowledge | Reviewed project knowledge, wiki and memory review — without claiming writes happened |
| Create materials and visuals | Files, reports, packages, image-card prompts |
| Delegate coding and ship | Scoped handoffs and typed workflow charts across model, runtime, wrapper, tool and agent targets |
| Operate and observe | Setup health, automation, workflow learning, memory review, status, repair |

Above the skills sit **playbooks** — situation-level pipelines (`request-to-handoff`,
`safe-feature-change`, `feedback-triage`, `idea-to-deploy`, `deploy-and-monitor`,
`release-readiness-review`, `research-department`, `materials-processing`, and ~13 more).
A playbook answers *"what should the whole experience do next, and who owns each stage?"*
where a skill answers *"which workflow guidance should Hermes use?"*

And beside them sit nine **roles** — guide, researcher, planner, operator, memory-keeper,
handoff-guide, builder, tracker, reviewer. Roles are explicitly **responsibility labels for
chat, not runtime agents.** Nothing is spawned. They exist so Hermes can say who owns the
next action without implying a worker started.

---

## 3. The evidence ladder — the idea worth stealing even if you never install it

| You see | It means |
|---|---|
| `Plan · not run` | A prompt or plan is ready. **Nothing has run.** |
| `Code · running` | An executor is running now and OMH is watching it |
| `Code · reported done` | The executor said it finished. **Nobody checked.** |
| `Test · verified` | A test, review or CI gate actually passed |

The repo's own note on this: *"the distinction that matters is the second row from the
bottom: an executor saying it is done is not the same as anything having been checked, and
most tools spell both 'complete'."*

This is the same three-state discipline the nyk article insists on — **drafted → approved
or scheduled → executed and verified** — and it is the single most teachable idea in either
source. Concretely: do not call something posted until you have the post URL; do not call
something deployed until the smoke test passes; do not call something paid until the
transaction is verified.

It maps directly onto this module. `/media-gen` estimating a cost is not the same as having
spent it; a hero clip rendered is not the same as a hero clip under 2 MB; a domain bought is
not the same as HTTPS live. Same ladder, no install required.

---

## 4. Mixture of models

OMH treats **model choice** and **coding ownership** as two separate decisions. It ships
editable, ordered fallback chains per category:

| Category alias | Editable recommendation order |
|---|---|
| `ultrabrain` | GPT-5.6 Sol |
| `deep` | GPT-5.6 Terra |
| `unspecified-high` | Kimi K3, then Claude Opus 5 |
| `unspecified-low` | GLM 5.2, then GLM 5.2 Ultrafast |
| `quick` | GLM 5.2 Ultrafast, then Kimi K3 |
| `writing` | Kimi K3, then Qwen3-Coder, then Gemini 3.1 Pro |
| `visual-engineering` | Claude Fable 5, then Kimi K3 |
| `artistry` | Gemini 3.1 Pro, then Claude Fable 5, then Kimi K3 |

The honesty is the point: the docs call these *"editable preferences, not benchmark
results"*, and the guided setup will not claim a model is available just because a config
file mentions it. The flow is staged — **inspect → confirm active → preview → apply →
verify** — and an apply requires a digest of the exact preview you approved. Prior session
metadata only counts as `observed_before`; a model becomes `confirmed_active` only when the
user explicitly confirms it.

Students ask "which model should I use" every single session. This table is a good answer
shape even for people who never install OMH: **pick per job category, keep a fallback,
write it down, and don't confuse a preference with a benchmark.**

---

## 5. Install, and the cost of installing too much

| Path | Command |
|---|---|
| Homebrew | `brew install rlaope/tap/omh` |
| Bun (recommended by the repo) | `bun install -g oh-my-hermes` |
| npm | `npm install -g oh-my-hermes` |
| macOS / Linux universal | `curl -fsSL https://raw.githubusercontent.com/rlaope/oh-my-hermes/main/install.sh \| sh` |
| Windows PowerShell 5.1+ | `irm https://raw.githubusercontent.com/rlaope/oh-my-hermes/main/install.ps1 \| iex` |
| Hermes skill tap | `hermes skills tap add rlaope/oh-my-hermes` |

Then three commands, and only three, are meant for normal users:

```sh
omh setup     # install managed skills, register them with Hermes
omh update    # upgrade the CLI via its owning package manager, then refresh skills
omh doctor    # check registration, point at the next repair action
```

Everything else in the CLI is explicitly *"deterministic backend infrastructure for Hermes
Agent, wrappers, coding agents, automation, and maintainers."* If a tutorial has a student
typing `omh capabilities export --json`, the tutorial is wrong.

### Core vs full — read this before recommending it to anyone

| Profile | What installs | Why |
|---|---|---|
| **`core`** (default) | Router + `doctor`, `skill`, `cancel`, `agent-ops-review`, plus `plan`, `gateway-intent-card`, `executor-runtime-readiness`, `ops-observability-card` | Bounded per-turn context weight; enough for a first chat/plan/status/handoff session |
| **`full`** (`--full`) | ~89 skills | Every workflow's guidance available immediately — at permanent context cost |

The repo is unusually straight about the trade-off: *"Every skill OMH installs is skill
guidance that Hermes carries into its routing context on every turn, not just when that
workflow is used."* A `--full` install returns a `context_cost_warning` naming the installed
count, the core count and the delta, and `omh docs skill-context-cost` will show the actual
always-loaded bytes — including how much is boilerplate repeated verbatim across skills.

Two more things worth knowing before anyone tries it:

- **Installs are non-destructive.** `omh setup` / `install` / `update` never delete an
  installed skill directory, so reinstalling with `core` after a `--full` does **not**
  shrink anything. Reconciling back down is a documented separate procedure.
- **`omh uninstall --all` before removing the package** if you actually want it gone;
  removing the CLI preserves OMH state.

---

## 6. Key ideas from the nyk "Hermes Agent Masterclass" field note

Source: [@nykdotdev](https://x.com/nykdotdev), X Article, 2026-07-06, ~39.1K views. It is a
practitioner field note, not documentation — useful for framing, weak on numbers.

### The one-line model

> Hermes is a self-improving agent that lives where the work happens — not inside one IDE,
> one chat window, or one provider account.

The beginner question is *what can Hermes do?* The operator question is *what loop should
Hermes own forever?* The article's core complaint is that most people use an agent as a
better terminal chatbot, and the hidden cost is broken loops and silent workflow drift.

### The eight-layer stack

| Layer | What it does | What beginners miss |
|---|---|---|
| Project context | Loads `AGENTS.md`, `CLAUDE.md`, `SOUL.md`, `.cursorrules` | The repo can shape the agent before you prompt |
| Tools | Search, terminal, browser, files, memory, cron, delegation | Tools are the hands, not decorations |
| Memory | Durable facts across sessions | Memory should be curated, not dumped |
| Skills | Reusable procedures | The agent should stop repeating your corrections |
| Session search | Recalls past conversations | Old work becomes retrievable infrastructure |
| Cron | Scheduled tasks | The agent can operate when you are not present |
| Subagents | Parallel isolated workers | Hard work can split into lanes |
| Messaging gateway | Telegram, Discord, Slack, email, SMS | The interface can be wherever you already work |

### The master loop

1. Define the recurring job
2. Put stable project context in `AGENTS.md` / `CLAUDE.md` (Hermes prefers `.hermes.md` / `HERMES.md`)
3. Run the workflow manually once
4. Save the reusable procedure as a skill
5. Add durable preferences to memory
6. Add verification commands or checks
7. Schedule it with cron if it repeats
8. **Review outputs and patch the skill when it fails**

Step 8 is the argument. *"A normal automation breaks silently. A Hermes workflow should teach
the agent what broke."*

### Memory vs skills — the separation that stops accidents

| | Answers | Example |
|---|---|---|
| **Memory** | What should the agent *know about you*? | "User prefers approval-gated posting and distinguishes scheduled state from executed state" |
| **Skill** | What should the agent *know how to do*? | "When deploying this app: build, tests, migration check, release notes, smoke test, rollback plan" |

The failure the article names: putting *"Always run the deployment checklist"* in memory turns
a preference into an ambient command. Keep procedures in skills, stable facts in memory,
progress in project docs.

### The approval rule

| Let it execute | Require approval |
|---|---|
| Read files, summarise docs, draft posts, run tests, prepare reports, create local artifacts | Posting publicly, sending messages, moving money, deleting data, production changes, credential handling, anything reputational |

And the cron corollary: **schedule detection, keep execution approval-gated.** *"Good cron
jobs stay quiet until thresholds are crossed."*

### The five failure modes

| Failure | Symptom | Fix |
|---|---|---|
| Memory bloat | Everything is memory, so memory stops being useful | Stable facts only; procedures → skills; progress → project docs |
| Skill rot | A skill that worked last month fails after the repo changed | Patch the skill; stop re-prompting around a broken procedure |
| Cron spam | The job reports every tiny change | Threshold it; silence is a feature |
| Approval confusion | "Scheduled" read as posted, "approved" read as executed | Keep drafted / approved / executed-and-verified separate |
| One-agent soup | One profile is CEO, coder, therapist, growth lead and ops manager | Split profiles when the role needs different memory and skills |

### Where the article and OMH converge

The article was written about bare Hermes and never mentions OMH — but its central demand
(*"do not call it done until it is verified"*) is precisely the four-rung ladder OMH
implements. Its "subagents are lanes, not theatre" is `ulw-work`'s disjoint file lanes. Its
"profile pattern" is Hermes' own `hermes profile` isolation, which OMH mirrors with
`--operating-model` and `--profile-pack`. Read together: **the article is the argument, OMH
is one opinionated implementation of it.**

---

## 7. So — does it belong in the course?

**Not as a required install.** Three reasons, in order of weight:

1. **Context cost.** A `--full` install degrades every unrelated conversation a student has.
   Diagnosing that in a live 60-minute session is not possible.
2. **Maturity.** Two months old, one maintainer, agent-authored, versus an upstream with
   230k stars. The course's own bias — build your own small skill, know what is in it —
   already covers most of the ground OMH covers.
3. **Vocabulary load.** "Prepared vs observed", "harness", "specialist", "capability
   projection", "exposure", "evidence boundary". The discipline is excellent. The vocabulary
   is enterprise-shaped and beginners will bounce off it.

**Yes as three ideas, and one optional homework path:**

| Take | Where it fits |
|---|---|
| The four-rung evidence ladder | Everywhere. Especially Module 3's deploy step — "deployed" is not "HTTPS verified" |
| Memory vs skills as different things | Module 1 built the second brain; this is the rule that keeps it clean |
| Category-based model choice with a written fallback chain | Pairs with Module 2's `/model-research` and this module's `/media-models` |
| `omh setup` (core profile) + `omh doctor`, as optional homework for students who already finished | A concrete look at what a professional skill pack looks like from the inside |

The honest framing for students: **OMH is what your own skills grow into if you keep
patching them for a year.** Reading its `SKILL.md` files is a better lesson than installing
them — every one names why it exists, when *not* to use it, its quality bar, its completion
checklist, and its recovery notes. That is a template worth copying into
`/media-models` and `/media-gen`.

---

## What's confirmed vs. inferred

### Confirmed against primary sources (repo files and official docs, read 2026-08-15)

- OMH repo metadata: 980 stars, 93 forks, MIT, created 2026-06-03, latest release v1.0.6
  published 2026-08-14, primary language Python. *(GitHub API)*
- 102 skill directories under `skills/` at `main`; docs describe `--full` as "~89 skills". *(git tree + `docs/INSTALLATION.md`)*
- The eight `ulw-` workflows, their triggers, roles and quality tiers, and the underlying
  canonical skill each wraps. *(`skills/ulw-*/SKILL.md`, `README.md`)*
- The four-rung evidence ladder, verbatim. *(`README.md`)*
- Core vs full profiles, the per-turn context cost statement, `context_cost_warning`,
  `omh docs skill-context-cost`, non-destructive installs. *(`docs/INSTALLATION.md`)*
- The model recommendation chains and the inspect → confirm → preview → apply → verify
  staging. *(`README.md`, `docs/INSTALLATION.md`)*
- The nine roles and their explicit "descriptors, not runtime agents" status. *(`docs/ROLES.md`)*
- The playbook list and the Hermes/OMH/executor ownership split. *(`docs/PLAYBOOKS.md`)*
- Hermes Agent: 230,864 stars, 45,800 forks, MIT, created 2025-07-22. *(GitHub API)*
- Hermes context-file discovery order — `.hermes.md`/`HERMES.md` → `AGENTS.md` →
  `CLAUDE.md` → `.cursorrules`, **first match wins**, with `SOUL.md` always loaded
  separately as identity. *(Hermes docs, Context Files)*
- Hermes cron supports attaching skills, platform delivery, and `no_agent=True` script-only
  jobs that skip the agent entirely. *(Hermes docs, Cron Scheduling)*
- Hermes profiles are genuinely isolated instances — own config, sessions, skills, home
  directory — managed with `hermes profile`. *(Hermes docs, CLI reference)*
- `hermes setup --portal` wires Nous Portal (300+ models) plus the Tool Gateway (Firecrawl
  search, FAL images, OpenAI TTS, Browser Use cloud browser) in one command. *(Hermes README)*

### Inferred, secondhand, or contradicted

- **"60+ tools" (article) vs "40+ tools" (Hermes docs).** The docs' Tools page says 40+.
  The article's number is unsourced; treat 40+ as the figure to quote.
- **"20+ messaging surfaces" (article) vs six gateway platforms (docs).** The gateway lists
  Telegram, Discord, Slack, WhatsApp, Signal and email/CLI. Cron *delivery* targets are
  broader — Matrix, SMS via Twilio, "and many others" — so the article is probably counting
  delivery targets, not chat surfaces. Two different numbers, worth not conflating.
- **The article's bracketed citations `[E1] [E2] [E6]`** point to nothing in the published
  post. The claims behind them check out against the Hermes docs anyway.
- **Whether OMH's routing actually improves outcomes.** The repo is scrupulous about never
  claiming it — it reports capability impact across separate dimensions and states it has
  "no aggregate capability score." No benchmark exists either way. Unmeasured, not disproved.
- **The "~89 vs 102 skills" gap** is most likely install-visibility: router-only,
  harness-only and agent-context surfaces stay routable references and do not generate an
  installed `SKILL.md`. Not confirmed by a count.
- **Course fit.** Section 7 is my judgement, not a finding.

### Open questions, not yet researched

1. What does a `core` install actually weigh in tokens, measured? `omh docs skill-context-cost`
   would answer it directly — needs a real install to run.
2. Does OMH work at all against a non-Nous provider (OpenRouter, local Morpheus) or does
   the model-alias flow assume Portal? Module 1's bonus OpenRouter path makes this relevant.
3. Does `omh setup` touch an existing `HERMES.md` / Hermes config in a way that could break
   a student's Module 1 setup mid-course? The docs say "preserve unrelated existing Hermes
   config" — unverified on a real machine.
4. Windows behaviour. [[module-00-fundamentals-windows-11/README|Module 0]] is Windows;
   OMH ships a PowerShell installer but the repo's testing emphasis is macOS/Linux.
5. Supply-chain posture: a `curl | sh` installer plus a skill tap plus ~89 markdown files
   injected into every turn is a real trust surface for a beginner audience.

---

## Sources

**oh-my-hermes (primary)**
- [rlaope/oh-my-hermes](https://github.com/rlaope/oh-my-hermes) — repository, `main`, 2026-08-15
- [README.md](https://github.com/rlaope/oh-my-hermes/blob/main/README.md) — ultra-skills table, evidence ladder, model chains
- [docs/INSTALLATION.md](https://github.com/rlaope/oh-my-hermes/blob/main/docs/INSTALLATION.md) — install paths, core vs full, guided model setup
- [docs/CAPABILITIES.md](https://github.com/rlaope/oh-my-hermes/blob/main/docs/CAPABILITIES.md) — capability families, task-scoped projection, claim boundary
- [docs/WORKFLOWS.md](https://github.com/rlaope/oh-my-hermes/blob/main/docs/WORKFLOWS.md) — generated per-skill reference
- [docs/ROLES.md](https://github.com/rlaope/oh-my-hermes/blob/main/docs/ROLES.md) — the nine roles
- [docs/PLAYBOOKS.md](https://github.com/rlaope/oh-my-hermes/blob/main/docs/PLAYBOOKS.md) — situation-level pipelines
- [Project site](https://rlaope.github.io/oh-my-hermes/)

**Hermes Agent (primary)**
- [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent)
- [Context Files](https://hermes-agent.nousresearch.com/docs/user-guide/features/context-files)
- [Cron Scheduling](https://hermes-agent.nousresearch.com/docs/user-guide/features/cron)
- [Tools & Toolsets](https://hermes-agent.nousresearch.com/docs/user-guide/features/tools)
- [Skills System](https://hermes-agent.nousresearch.com/docs/user-guide/features/skills)
- [CLI Reference](https://hermes-agent.nousresearch.com/docs/reference/cli-commands)
- [Tool Gateway](https://hermes-agent.nousresearch.com/docs/user-guide/features/tool-gateway)

**Practitioner commentary (secondary)**
- nyk, ["Hermes Agent Masterclass: build the agent OS that keeps learning after the chat ends"](https://x.com/nykdotdev), X Article, 2026-07-06

**Related notes in this vault**
- [[research-2026-ai-best-practices]] — context rot, context engineering, the six Anthropic tactics
- [[course-overview]] — where Hermes sits in the AI Power Users series
- [[module-03-media-models-and-your-own-domain/README|Module 3 README]] — `/media-models`, `/media-gen`, the `HERMES.md` step

# Module 1 — Student Guide

**Agent & Private AI Second Brain Setup**
Day 1 · Module 1 of 15 · follow along here.

This guide has every step, every link, and a plain-language description of what
each tool does. **No coding experience needed** — you'll copy and paste a few
commands, and we explain each one. Everything here is free, and almost everything
is open source.

> ⏱️ **Before class:** the downloads are the slow part. If you can, install
> Obsidian, Handy, and cmux, and run the Hermes install command **before** the
> session starts. Then in class we spend our time wiring it together, not waiting
> on progress bars.

---

## What you're building

A **private AI second brain**: a personal AI agent that lives inside your own
notes, thinks using a private model, and doesn't depend on any single company's
cloud.

```
          ┌─────────────────────────────────────────────┐
          │   Your Second Brain  (a folder on your disk) │
          │   organised with the PARA method             │
          │                                              │
   speak  │   Obsidian + VS Code ── view/edit notes,      │
  ─────▶  │      ▲                  skills & config        │
  Handy   │      │                                        │
  (STT)   │   Hermes agent  ── runs in the folder,        │
          │      │             reads & writes your notes  │
          │      ▼                                        │
          │   Morpheus  ── the private LLM brain          │
          │   (api.mor.org, OpenAI-compatible)           │
          └─────────────────────────────────────────────┘
              all launched from cmux (your terminal)
```

### The six pieces

| Tool | What it does | Free? | Open source? | Link |
|---|---|---|---|---|
| **Handy** | Offline speech-to-text. Press a key, talk, and it types for you. Your voice never leaves your computer. | ✅ Free | ✅ Yes | [handy.computer](https://handy.computer) |
| **Obsidian** | Your window into your notes. Turns a folder of plain text files into a searchable, linked knowledge base with a graph view. | ✅ Free (personal) | ⚪ Free, not open source | [obsidian.md](https://obsidian.md) |
| **VS Code** | A full code editor for inspecting the machinery: your skill files, config, and any code the agent writes. | ✅ Free | ✅ Yes (MIT) | [code.visualstudio.com](https://code.visualstudio.com) |
| **Hermes** | The AI agent. Lives in your folder and can read/write notes and run tasks for you. From Nous Research. | ✅ Free | ✅ Yes | [hermes-agent.nousresearch.com](https://hermes-agent.nousresearch.com) |
| **Morpheus** | The private "brain" — a decentralized network of large language models. Gives you an OpenAI-compatible API. | ✅ Free to start | ✅ Open network | [mor.org](https://mor.org) |
| **cmux** | A terminal built for running AI agents (macOS). On Windows/Linux use your normal terminal. | ✅ Free | ✅ Yes | [github.com/manaflow-ai/cmux](https://github.com/manaflow-ai/cmux) |

---

## Step 0 — What you need

- A laptop: **macOS, Windows, or Linux**.
- A **microphone** (built-in is fine) for speech-to-text.
- About **2 GB** of free disk space (models + apps).
- 45 minutes, and a willingness to paste a few commands.

Throughout, when you see a code block, copy it and paste it into your terminal (or
into cmux). We'll tell you which OS each command is for.

---

## Step 1 — Install Obsidian (your notes window)

**What it is:** Obsidian reads a folder of Markdown (`.md`) text files and turns
it into a beautiful, linkable knowledge base. Your notes stay as plain files on
your disk — no lock-in.

1. Go to **[obsidian.md](https://obsidian.md)** and click **Download**.
2. Install it like any app. It's **free for personal use**.
3. Open it once, then leave it — we'll point it at our folder in Step 7.

---

## Step 2 — Install Handy (speak instead of type)

**What it is:** Handy is a free, open-source speech-to-text app that runs
**completely offline**. Press a shortcut, speak, release, and your words are typed
into whatever app you're using. It uses OpenAI's Whisper models (or a fast
CPU-friendly model called Parakeet) — all locally, so your audio never leaves your
machine.

**Install:**

- **macOS:**
  ```bash
  brew install --cask handy
  ```
  (Don't have Homebrew? Download directly from [handy.computer](https://handy.computer).)
- **Windows:**
  ```powershell
  winget install cjpais.Handy
  ```
- **Linux / any:** download from the [releases page](https://github.com/cjpais/Handy/releases).

**First run:**

1. Open Handy. It will **download a speech model** automatically (this can take a
   few minutes — start it before class).
2. Grant the permissions it asks for: **microphone** and **accessibility** (so it
   can type for you).
3. Note the **push-to-talk shortcut** in settings. Test it in any text box: hold
   the shortcut, say "hello world," release — it should type it.

---

## Step 3 — Install VS Code (inspect the machinery)

**What it is:** Visual Studio Code is a free, open-source code editor. Obsidian is
how you *read and write your notes*; VS Code is how you *look under the hood* — the
actual skill files (`SKILL.md`), your Hermes config, and any code your agent
writes. Same folder, two lenses.

1. Go to **[code.visualstudio.com](https://code.visualstudio.com)** and click
   **Download**. Install it like any app.
2. Open VS Code → **File → Open Folder…** → choose your `SecondBrain` folder (once
   you create it in Step 7). You'll see every file and folder in a sidebar.
3. That's all for now. We'll use it in Step 9 to view and edit your first skill
   file.

> Prefer a 100% open-source build with no telemetry? [VSCodium](https://vscodium.com)
> is the same editor built from the MIT source — either works for this course.

---

## Step 4 — Install cmux (your agent terminal)

**What it is:** cmux is a free, open-source terminal designed for running AI
agents, with handy features like notifications when an agent needs you. It's
**macOS only** right now.

- **macOS:** download from **[github.com/manaflow-ai/cmux](https://github.com/manaflow-ai/cmux)** (see its README for the latest install).
- **Windows:** use **Windows Terminal** (built in) — or the community port [wmux](https://github.com/amirlehmam/wmux).
- **Linux:** your normal terminal app works perfectly.

> Any terminal works for this course — cmux just makes running agents nicer. If
> you're not on macOS, use the terminal you already have and follow along.

---

## Step 5 — Install Hermes (your AI agent)

**What it is:** Hermes is an open-source AI agent from Nous Research. Unlike a
plain chatbot, it can take actions: read and write files in your folder, run
commands, remember things, and run reusable **skills**. It's the worker; Morpheus
(next step) is its brain.

**Install** (paste into your terminal / cmux):

- **macOS / Linux / WSL:**
  ```bash
  curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
  ```
- **Windows (PowerShell):**
  ```powershell
  iex (irm https://hermes-agent.nousresearch.com/install.ps1)
  ```

The installer sets up everything Hermes needs (a Python environment and a few
tools) — you don't need to understand those; just let it finish.

**Then reload your terminal and check it runs:**
```bash
source ~/.bashrc   # or open a new terminal window
hermes --help
```
If you see Hermes help text, you're good. **Don't fully start it yet** — we
connect the brain first.

> Hermes stores its settings in `~/.hermes/` (on Windows, `%LOCALAPPDATA%\hermes`).
> We'll use that folder for skills in Step 9.

---

## Step 6 — Create your Morpheus account & API key (the private brain)

**What it is:** Morpheus is a decentralized network that serves large language
models. Instead of your prompts going to one company, they're served across an
open network, with private providers running in secure hardware enclaves. It gives
you an **OpenAI-compatible API**, which is the standard "language" most AI tools
speak — so Hermes can use it directly.

1. Go to **[app.mor.org](https://app.mor.org)** and **create an account**, then log in.
2. In the app, **create an API key**. If there's an "automation enabled" toggle,
   make sure it's on.
3. **Copy your API key** and paste it somewhere safe for now (a note you'll
   delete). ⚠️ Treat it like a password — we cover key safety properly on Day 5.
4. Still in the app, open the **test / chat page**, try a model, and **note the
   model's name** exactly as shown. You'll paste it into Hermes in Step 8.

**The two things to keep from this step:**

- Your **API key** (looks like a long string of characters).
- The **base URL** for Morpheus's API: `https://api.mor.org/api/v1`

There is a **free way to get started**. You can explore paid staking options
later, but you don't need them for this course.

---

## Step 7 — Build your second brain with PARA

**What it is:** Your second brain is just a folder. We organise it with **PARA**,
a simple system by Tiago Forte with four buckets:

- **1-Projects** — things with a goal and a finish line ("Launch my portfolio site").
- **2-Areas** — ongoing responsibilities ("Health," "Finances," "Job search").
- **3-Resources** — topics and references you want to keep ("AI tools," "Recipes").
- **4-Archives** — finished or inactive stuff, out of the way.

Everything you ever capture goes into exactly one of these four. That's the whole
system.

**Create the folder and structure:**

- **macOS / Linux:**
  ```bash
  mkdir -p ~/SecondBrain/{1-Projects,2-Areas,3-Resources,4-Archives}
  ```
- **Windows (PowerShell):**
  ```powershell
  mkdir $HOME\SecondBrain\1-Projects, $HOME\SecondBrain\2-Areas, $HOME\SecondBrain\3-Resources, $HOME\SecondBrain\4-Archives
  ```

**Open it as an Obsidian vault:**

1. Open Obsidian → **"Open folder as vault"** → choose your `SecondBrain` folder.
2. Create your first note inside **1-Projects** — call it after something you're
   actually working on. Write a sentence or two. (You can dictate it with Handy!)

You now have a visual, linked knowledge base. Try Obsidian's **graph view** to see
your notes as a network.

---

## Step 8 — Run Hermes in your folder and connect it to Morpheus

The trick that makes this a *second brain* and not just a chatbot: we launch the
agent **inside** the folder, so its working directory is your notes. It can then
read and write them directly.

**1. Open your folder in the terminal (cmux or your normal terminal):**
```bash
cd ~/SecondBrain
```

**2. Point Hermes at the Morpheus brain.** Paste these two lines, replacing the
key with yours:

- **macOS / Linux:**
  ```bash
  export OPENAI_BASE_URL="https://api.mor.org/api/v1"
  export OPENAI_API_KEY="paste-your-morpheus-key-here"
  ```
- **Windows (PowerShell):**
  ```powershell
  $env:OPENAI_BASE_URL="https://api.mor.org/api/v1"
  $env:OPENAI_API_KEY="paste-your-morpheus-key-here"
  ```

**3. Launch the agent** (from inside `~/SecondBrain`):
```bash
hermes
```

**4. Tell it which Morpheus model to use** — paste the model name you noted in
Step 6:
```
/model openai:PASTE-MORPHEUS-MODEL-NAME
```
(You can also check/change the model any time with `hermes model`.)

**5. Say hello and prove it works:**
```
Read the note in 1-Projects and tell me what it's about in one sentence.
```
If Hermes reads your note and answers, your private stack is fully wired:
**your model, your notes, your folder.** 🎉

> **Want this to stick between sessions?** Instead of `export` each time, add the
> two `OPENAI_...` lines to your shell profile (`~/.zshrc` or `~/.bashrc`), or set
> them with `hermes config set`. We'll tidy this up in Module 2.

---

## Step 9 — Create your first skill: a daily standup

A **skill** is a reusable instruction you save once and run any time. Hermes reads
skills from `~/.hermes/skills/`, and each one becomes a slash command named after
its folder. We'll make a `daily-standup` skill: your AI reviews your projects and
progress with you, like a stand-up meeting, and saves the result into your brain.

**1. Create the skill folder:**

- **macOS / Linux:**
  ```bash
  mkdir -p ~/.hermes/skills/daily-standup
  ```
- **Windows (PowerShell):**
  ```powershell
  mkdir $env:LOCALAPPDATA\hermes\skills\daily-standup
  ```

**2. Create the file `SKILL.md` inside that folder** and paste this in. This is a
perfect job for **VS Code**: open the `.hermes/skills/daily-standup` folder (or use
**File → New File**), paste, and save as `SKILL.md`. (Obsidian or any text editor
works too.)

```markdown
---
name: daily-standup
description: A daily stand-up with your AI. Reviews your active projects, your
  goals, and your progress, asks three quick questions, and saves a dated note.
---

# Daily Standup

When the user runs this skill:

1. Read the notes in the `1-Projects` folder and briefly list the active projects
   you find, one line each.
2. Greet the user by name if you know it, and share a one-sentence read on where
   their projects stand.
3. Ask these three questions, one at a time, and wait for each answer:
   - "What did you get done since your last standup?"
   - "What's the one most important thing to move forward today?"
   - "Is anything blocking you?"
4. When you have the answers, write a new Markdown file named
   `2-Areas/standups/YYYY-MM-DD-standup.md` (use today's date) containing:
   - A short heading with the date.
   - **Done**, **Today**, and **Blockers** sections from the answers.
   - A one-line encouragement tied to their goals.
5. Confirm the file was saved and show the user its path so they can open it in
   Obsidian.

Keep it warm, brief, and practical. This is a two-minute morning ritual, not a
report.
```

**3. Run it.** Back in Hermes (launched from `~/SecondBrain`):
```
/daily-standup
```

**4. Answer by talking.** When the agent asks a question, use **Handy**: hold your
push-to-talk shortcut, speak your answer, release — it types for you. Do that for
all three questions.

**5. See the result in Obsidian.** Open `2-Areas/standups/` — there's today's
standup note, written by a private AI, from your own words. Run it every morning.

---

## ✅ You did it

You now have:

- 🎙️ **Handy** — offline speech-to-text
- 🧠 **Obsidian** — a PARA-organised second brain
- 🧩 **VS Code** — a window into your files, skills, and config
- 🤖 **Hermes** — an agent living in your notes
- 🔐 **Morpheus** — a private model powering it
- 💻 **cmux** — a terminal for your agents
- ⭐ **A daily-standup skill** you'll actually use

---

## Homework (due Module 3 — the peer lab)

1. **Finish the setup** if any piece didn't connect. Troubleshooting help comes in
   Module 2 — bring your specific error.
2. **Run your daily standup at least twice** before the peer lab.
3. **Capture 5 real notes** into your PARA folders (at least one in `1-Projects`).
4. **Do the [quiz](./quiz.md)** — bring your written answers to the open-ended
   questions for peer review.

## Troubleshooting quick hits (more in Module 2)

- **`hermes: command not found`** → open a new terminal, or run `source ~/.bashrc`
  (macOS/Linux). On Windows, reopen PowerShell.
- **Agent errors about the model / auth** → re-check `OPENAI_BASE_URL` is exactly
  `https://api.mor.org/api/v1`, your key is pasted with no spaces, and the model
  name matches app.mor.org exactly.
- **Handy types nothing** → confirm microphone + accessibility permissions, and
  that the speech model finished downloading.
- **Hermes can't see your notes** → make sure you ran `hermes` from **inside**
  `~/SecondBrain` (`pwd` should show that folder).

## All the links in one place

- Obsidian — https://obsidian.md
- VS Code — https://code.visualstudio.com · fully-open build: https://vscodium.com
- Handy — https://handy.computer · releases: https://github.com/cjpais/Handy/releases
- cmux — https://github.com/manaflow-ai/cmux
- Hermes — https://hermes-agent.nousresearch.com
- Morpheus app (account + API key) — https://app.mor.org
- Morpheus API base URL — `https://api.mor.org/api/v1`
- PARA method (background reading) — https://fortelabs.com/blog/para/

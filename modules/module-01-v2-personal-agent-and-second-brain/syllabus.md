# Module 1 V2: Setting Up Your Personal AI Agent and Second Brain

**Status:** syllabus draft v1, planning phase.
**Date:** 2026-08-18
**Platform:** macOS and Windows 11, both taught side by side. Every command in this module
is given twice where the two systems differ.
**Prereq:** none. This is the entry point to the course.
**Replaces:** Module 0 and Module 1, rebuilt as one two hour session.
**Companions:** `student-guide.md` · `agenda.md` · `quiz.md` · `luma-description.md` ·
`image-prompts.md` · `youtube-livestream.md` · `web-page.md`

---

## Format

**One session, 120 minutes, no break, live on YouTube.**

| Part | Time | Theme |
|---|---|---|
| **Part 1** | 0:00-0:32 | The machine. Terminal, the four installs, your voice as an input device |
| **Part 2** | 0:32-0:56 | The second brain. PARA, Obsidian, VS Code, one folder seen three ways |
| **Part 3** | 0:56-1:32 | The agent. OpenRouter, one model, and your first skill |
| **Part 4** | 1:32-2:00 | The agent that never sleeps. A server you rent, and Telegram on your phone |

**What is different from the first version of Module 1.** Three things.

1. **Windows 11 is a first class path**, not a footnote. Module 0's terminal, File Explorer
   and shortcut material is folded in, so a Windows student never needs a separate session.
2. **The model provider is OpenRouter, and we pick exactly one model.** The first version
   led with a private decentralized model and treated OpenRouter as a bonus. That inverted
   the difficulty. Students spent the session debugging a model connection instead of
   building. Privacy is now Day 5's subject, taught properly, and today's job is an agent
   that works on the first try.
3. **The session ends on a server, not on a laptop.** The old Module 1 ended with a skill
   you could run when your laptop was open. This one ends with an agent running on a
   machine you rent, reachable from your phone by text message.

**The one artifact.** A Telegram bot, on your phone, that is your own agent, running on
your own server, holding your own notes. You send it a message from the street and it
answers.

---

## Learning objectives

By the end, a student can:

1. Read a filesystem path out loud and navigate to it from a terminal on either macOS or
   Windows 11
2. Install and configure local speech to text, and dictate into any application
3. Explain what PARA sorts by, and file a real note into the right bucket
4. Open one folder as an Obsidian vault and as a VS Code project, and say what each lens
   is for
5. Choose one model on OpenRouter by filtering on tool support first and price second, and
   say what one agent session costs
6. Launch an agent inside their notes folder and prove it can read and write those notes
7. Write a skill from scratch, in a file, and run it as a slash command
8. Rent a Linux server, connect to it over SSH with a key, and install an agent on it
9. Create a Telegram bot, allowlist themselves, and run the agent as a service that
   survives a reboot
10. State who can control an agent that has a terminal, and what stops them

---

# PART 1: The machine (32 min)

## 0:00-0:04 · Open with the payoff (4 min)

Phone on screen, screen shared. Send a Telegram message to a bot:

> "What did I say I would finish this week?"

The bot answers with three lines pulled from a note. Then show the terminal window on the
server it came from, still open, still logged in.

> "That is not an app. There is no company between us. That is a computer I rent for six
> dollars a month, running software I installed, reading notes that live in a folder I own.
> By two hours from now you will have your own."

Then the honest frame, said once, plainly:

> "Two hours. Four installs, one folder, one model, one skill, one server. Nothing here is
> hard. There is just a lot of it, and the order matters."

**Cost, said in the first four minutes:** about 5 USD of OpenRouter credit, which lasts
months, and about 6 USD a month for the server, which you can destroy at the end of the
session and pay pennies for.

---

## 0:04-0:16 · Terminal ground rules, both platforms (12 min)

This is Module 0 compressed. Run it fast. The goal is not fluency, it is that nobody is
frightened of the window for the next 116 minutes.

**Open it.**

| | macOS | Windows 11 |
|---|---|---|
| Open the terminal | `Cmd+Space`, type `terminal` | Press `Windows`, type `terminal` |
| What you are looking at | zsh | PowerShell |
| The prompt says | `you@Mac ~ %` | `PS C:\Users\you>` |

**Why it exists at all,** in three sentences, not three minutes:

> "A click cannot be saved, repeated, or sent to someone else. A command can. And an AI
> agent produces text while a terminal consumes text, which is why the terminal is the one
> place you and a machine can work on the same thing."

**The six commands, taught once, used all day:**

| Job | macOS | Windows 11 |
|---|---|---|
| Where am I | `pwd` | `pwd` |
| What is in here | `ls` | `ls` |
| Go into a folder | `cd Documents` | `cd Documents` |
| Go up one | `cd ..` | `cd ..` |
| Go home | `cd ~` | `cd ~` |
| Clear the screen | `Cmd+K` | `cls` |

**The two keys that matter more than the six commands.** Teach these hard, on camera,
slowly.

- **Tab** completes a name you have started typing. It kills every typo before it happens.
- **Up arrow** brings back the last command.

**`Ctrl+C` stops whatever is running.** Say out loud that nothing in this session can
damage their laptop.

**Reading a path.** Put both on screen at once:

```
macOS:       /Users/you/secondBrain/1-projects
Windows 11:  C:\Users\you\Documents\secondBrain\1-projects
```

> "Same idea, different punctuation. A slash means go inside. `~` means your home folder,
> which is `/Users/you` on a Mac and `C:\Users\you` on Windows."

**The one gotcha worth 30 seconds.** In a terminal, `Ctrl+C` copies **only when text is
selected**. With nothing selected it stops the running command instead. On macOS use
`Cmd+C` and `Cmd+V` and the problem does not arise.

**The bridge, both directions.** This is the idea that makes the rest of the session make
sense: the terminal and the file browser are two windows onto the same folder.

| Direction | macOS | Windows 11 |
|---|---|---|
| Terminal to file browser | `open .` | `start .` |
| File browser to terminal | Right click the folder, Services, New Terminal at Folder | Right click inside the folder, Open in Terminal |

**Windows only, do this now, it prevents an hour of confusion later.** File Explorer,
**View**, **Show**, tick **File name extensions** and **Hidden items**. Windows hides the
end of filenames by default and shows `note` when the file is really `note.md`.

> Host note: do not teach `cp`, `mv`, `rm` here. Nobody needs them today and every minute
> spent on them is a minute stolen from the server segment.

---

## 0:16-0:24 · Install the four tools (8 min)

Four installs, kicked off together, explained while they download.

| Tool | What it is for | Cost | Open source |
|---|---|---|---|
| **Handy** | Talk instead of type, offline | Free | Yes, MIT |
| **Obsidian** | Read, link and think in your notes | Free for personal use | No |
| **VS Code** | Look at what is actually in the files | Free | Code is MIT. Microsoft's build adds telemetry |
| **Hermes** | The agent that lives in your folder | Free | Yes, MIT |

**The three app downloads,** started first because they are the slow part:

- Handy: https://handy.computer
- Obsidian: https://obsidian.md
- VS Code: https://code.visualstudio.com

**Then the agent,** which is a single command.

macOS:

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
```

Windows 11, in PowerShell:

```powershell
iex (irm https://hermes-agent.nousresearch.com/install.ps1)
```

> "That one command installs Python, Node, ripgrep and ffmpeg for you. You will never
> touch any of them. This is the correct division of labour: the installer knows what it
> needs, you do not have to."

**Then open a new terminal window** and check it landed:

```
hermes doctor
```

`hermes doctor` prints what is missing and how to fix it. It is the first thing to run
whenever anything is wrong for the rest of the course.

**Say what Obsidian being closed source means, in one line, and move on:**

> "Obsidian is the one thing here that is not open source, and we use it anyway, because
> your notes are plain text files in a normal folder. If Obsidian vanished tomorrow every
> note still opens in Notepad. Ask what happens to your files if the company dies. That is
> the question, not the licence."

---

## 0:24-0:32 · Handy, your voice as an input device (8 min)

**Pick the model first.** Handy, **Models**, download a **Whisper** model. Whisper supports
99 or more languages including Khmer and Malay. Whisper Large is the most accurate. Whisper
Turbo or Small are faster on older machines.

> The model download is over a gigabyte. This is the single biggest reason the student
> guide says install before you arrive.

**Permissions.** macOS asks for **microphone** and **accessibility**. Windows asks for
**microphone**. Handy cannot type for you until accessibility is granted, and a student who
skips this will think the app is broken.

**Set a push to talk key.** Settings, Shortcuts, click the shortcut, press your keys.

| Mode | How it works |
|---|---|
| **Push to talk** | Hold the key, speak, release. Recommended |
| **Toggle** | Press to start, press again to stop |

**Prove it types anywhere.** Dictate one sentence into three different applications: the
terminal, a browser address bar, and a text editor. That is the whole point. It is not a
transcription app, it is a keyboard.

**Two settings worth the 60 seconds:**

- **Custom Words**, under Advanced, for names and terms it keeps getting wrong. Put your
  own name in it now.
- **History**, in the settings sidebar, holds every past transcription with its audio and a
  one click copy. This is where lost text goes, not into the void.

**The claim and its limit, together:**

> "Every word of this runs on your laptop. Your voice does not reach a company and it works
> with the wifi off. The limit is that it is only as good as the model you downloaded, and
> on an older machine the accurate model will be too slow, so you will trade accuracy for
> speed. That is a real trade, not a marketing one."

---

# PART 2: The second brain (24 min)

## 0:32-0:42 · Build the vault with PARA (10 min)

**PARA is four buckets and one rule.** Tiago Forte's system, and the only organising system
in this course.

| Folder | What goes in it | The test |
|---|---|---|
| `1-projects` | Has an outcome and an end | Can I tick this off? |
| `2-areas` | Ongoing, no end date | Do I do this forever? |
| `3-resources` | Reference material | Would I want this later? |
| `4-archives` | Finished or dormant | Is this done or dead? |

> "PARA sorts by how soon you must act, not by subject. Learn Khmer is an Area. Pass the
> Khmer exam in March is a Project. Same topic, different bucket, because the question is
> when do I touch this, not what is it about."

**Build it.**

macOS:

```bash
mkdir -p ~/secondBrain/{1-projects,2-areas,3-resources,4-archives}
cd ~/secondBrain
ls
```

Windows 11, in PowerShell:

```powershell
cd ~\Documents
mkdir secondBrain
cd secondBrain
mkdir 1-projects, 2-areas, 3-resources, 4-archives
ls
```

**Windows only, one extra step.** Right click `secondBrain` in File Explorer and choose
**Always keep on this device**, so OneDrive keeps a real copy on the machine instead of a
cloud placeholder. An agent cannot read a placeholder.

**Open it as an Obsidian vault.** Obsidian, **Open folder as vault**, pick `secondBrain`.

> "Nothing was imported. Nothing was converted. A vault is just a folder. That sentence is
> the reason this stack has no lock in."

**Write the first real note, out loud.** In `1-projects`, create a note named after
something they are actually working on, and dictate two sentences into it with Handy. The
agent needs something to read in twenty minutes and this is it.

---

## 0:42-0:50 · Obsidian, the three features that compound (8 min)

Do not tour the app. Three features, then stop.

**1. Detect all file extensions.** Settings, Files and links, turn on **Detect all file
extensions**. By default Obsidian shows markdown and hides everything else, so students
think their images and PDFs vanished. They did not.

**2. Links, and the trick inside them.**

```markdown
[[Note Name]]                  link to a note
[[Note Name|display text]]     link with a different label
![[Note Name]]                 embed that note's content here
![[image.png|400]]             embed an image, 400px wide
```

> "Linking to a note that does not exist yet is a feature, not an error. Obsidian creates
> it the moment you click. Write the link first and fill the note in later."

**3. Backlinks.** Every note shows which other notes link to it, worked out from the files,
maintained by nobody. Below it, **Unlinked mentions** finds notes that name this one without
linking and offers to link them in one click.

> "This is why a vault compounds and a folder does not. In a folder you have to remember
> where you filed something. Here, things find each other."

**Four shortcuts, on screen, then move on:**

| Action | macOS | Windows 11 |
|---|---|---|
| New note | `Cmd+N` | `Ctrl+N` |
| Jump to any note | `Cmd+O` | `Ctrl+O` |
| Every command by name | `Cmd+P` | `Ctrl+P` |
| Search the whole vault | `Cmd+Shift+F` | `Ctrl+Shift+F` |

---

## 0:50-0:56 · VS Code, the same folder underneath (6 min)

From the terminal, standing in the folder:

```
code .
```

If `code` is not found on macOS, open VS Code, press `Cmd+Shift+P`, and run **Shell
Command: Install 'code' command in PATH**. On Windows the installer adds it by default.

> "Third window, same folder. Obsidian shows your thinking. VS Code shows the files. You
> open this window when something is wrong, when a file is hidden, or when you want to see
> exactly what the agent changed."

**Open the built in terminal** with `` Ctrl+` `` and run `pwd`. It is already standing in
`secondBrain`. Files on the left, terminal underneath, one folder.

> "That layout is how most people actually work, and it is where you will run your agent
> for the rest of the course."

**One extension, to show what installing one means:** `Cmd+Shift+X` or `Ctrl+Shift+X`,
search **Markdown All in One**, click Install. A search box and a button. That is all a
plugin is.

---

# PART 3: The agent (36 min)

## 0:56-1:06 · OpenRouter, and picking exactly one model (10 min)

**What OpenRouter is, in one sentence:**

> "One account, one key, one bill, and several hundred models behind it. You change which
> model your agent uses by changing one line, not by signing up anywhere new."

**Get the key.**

1. Sign up at https://openrouter.ai
2. **Credits**, add **5 USD**. That is the number for this course, not a suggestion.
3. **Keys**, create a key. It starts `sk-or-v1-`. It is shown once.

**Then teach the filter, which is the actual lesson.** Go to https://openrouter.ai/models
live, on camera, and narrow it in this order:

| Order | Filter | Why it is in this position |
|---|---|---|
| **1** | **Supports tools** | Non negotiable. An agent that cannot call tools cannot read or write a file. A model without tool support will chat happily and do nothing, which is the most confusing failure in the course |
| **2** | **Context length** | The agent re-reads the conversation on every turn. Under about 100k tokens you will hit the ceiling inside a long session |
| **3** | **Price, per million tokens** | Two numbers, input and output. Output is normally three to five times input |
| **4** | Speed | Last. A model that answers slowly and correctly beats one that answers instantly and wrong |

**Then do the arithmetic on camera**, because this is the number that stops people being
afraid of the meter:

> "A working session where the agent reads a few notes and writes one is roughly 50,000
> tokens in and 5,000 out. At the prices on this screen that is about half a cent. Five
> dollars is somewhere near a thousand of those. You are not going to run out today."

**The class pick.** One model, everyone on the same one, so that when something breaks we
are all breaking the same way.

| Model | Input per 1M | Output per 1M | Context | Why |
|---|---|---|---|---|
| **`z-ai/glm-4.7-flash`** | **0.06 USD** | **0.40 USD** | **202,752** | **The class default.** Built for tool calling loops, cheap enough to be careless with |
| `qwen/qwen3.5-flash-02-23` | 0.065 USD | 0.26 USD | 1,000,000 | Cheaper output and a much larger context. Use it if your vault is big |
| `deepseek/deepseek-v4-flash` | 0.083 USD | 0.165 USD | 1,048,576 | Cheapest output of the three. Use it if the agent writes long answers |

> **Verified live against the OpenRouter models API on 2026-08-18.** There were 328 paid
> models with tool support on that date. **Re-check all three prices the morning of the
> stream** and change the table if they moved. Reading a wrong price out loud is worse than
> pausing to look it up.

**The free option, and why it is not the default.** OpenRouter has models ending `:free`,
including `nvidia/nemotron-3.5-lightning:free`. Free accounts are limited to **20 requests
per minute and 50 requests per day**, rising to 1,000 per day once you have bought at least
10 USD of credit. One agent turn is several requests, so 50 a day is a handful of sessions.
Say this straight: free is for someone who genuinely cannot pay, not for someone saving
five dollars.

**The privacy line, stated once and not softened:**

> "OpenRouter is not private. Your prompt and whatever note content the agent sends leaves
> your machine, passes through OpenRouter, and reaches whichever company actually serves
> that model. Handy is private. Your notes on disk are private. This part is not. Running a
> model on your own hardware is Day 5, and it is a real answer, not a consolation prize.
> Today we are buying reliability, and that is a fair trade as long as you know you made
> it."

---

## 1:06-1:16 · Run the agent inside your notes (10 min)

**The move that makes this a second brain and not a chatbot:** launch the agent from inside
the folder, so its working directory is your notes.

```
cd ~/secondBrain
hermes
```

Windows 11:

```powershell
cd ~\Documents\secondBrain
hermes
```

**Wire it to OpenRouter.** Two ways, both shown, because students will meet both.

The wizard, which is what most people should use:

```
hermes model
```

Pick **OpenRouter** in the left column, paste the key when asked, then pick the model from
the right column.

The direct way, for anyone whose wizard misbehaves:

```
hermes config set OPENROUTER_API_KEY sk-or-v1-paste-your-key-here
```

then inside a session:

```
/model z-ai/glm-4.7-flash --provider openrouter --global
```

`--global` persists the choice to `~/.hermes/config.yaml` as well as switching the running
session. Without it the change lasts until you close the window.

**Where things live.** Put this table on screen. Students will need it three more times
today.

| | macOS | Windows 11 native |
|---|---|---|
| Config and data | `~/.hermes/` | `%LOCALAPPDATA%\hermes\` |
| Config file | `~/.hermes/config.yaml` | `%LOCALAPPDATA%\hermes\config.yaml` |
| Secrets | `~/.hermes/.env` | `%LOCALAPPDATA%\hermes\.env` |
| Skills | `~/.hermes/skills/` | `%LOCALAPPDATA%\hermes\skills\` |

**Prove it, with a request that can only succeed if everything worked:**

```
Read the note in 1-projects and tell me in one sentence what it is about. Then add a
line at the bottom of that note saying when you read it.
```

If it reads the note and edits the file, the stack is wired. Open the file in Obsidian on
screen and show the new line appear.

> "That is the whole thing. Your model, your notes, your folder, and nothing in between.
> Everything after this is making it more convenient."

---

## 1:16-1:32 · BUILD BY ASKING: your first skill (16 min)

**The centrepiece of Part 3.** A skill is a set of instructions you save once and run any
time by name. Hermes reads skills from a folder, and the folder name becomes the slash
command.

**First, the name check, and say why on camera.** Hermes has built in slash commands:
`/model`, `/reset`, `/sessions`, `/usage`, `/profile`, `/status`, `/context`, `/tools`,
`/skills`, `/help`. **A skill that shadows a built in fails silently**, which is the worst
kind of failure. `/standup` is clear of all of them, so `/standup` is the name.

**Create the folder.**

macOS:

```bash
mkdir -p ~/.hermes/skills/personal/standup
```

Windows 11, in PowerShell:

```powershell
mkdir $env:LOCALAPPDATA\hermes\skills\personal\standup
```

**Then the brief.** Read this to the agent on screen, verbatim. This is the pasteable thing
and it is the most valuable text in the module.

```
Write me a skill and put it at ~/.hermes/skills/personal/standup/SKILL.md
On Windows put it at %LOCALAPPDATA%\hermes\skills\personal\standup\SKILL.md

Ask me anything ambiguous first. Then write the file, show me the full contents, and
tell me the exact path you wrote it to.

────────────────────────────────────────────────────────────
SKILL: standup
A two minute morning ritual. Not a report, not a journal entry.

FRONTMATTER, exactly this shape:
  name: standup
  description: A short morning standup. Reads my active projects, asks three questions,
    and saves a dated note.
  version: 1.0.0
  metadata:
    hermes:
      tags: [daily, planning]
      category: personal

WHEN I RUN IT, DO THIS IN ORDER:

1. Read every note in the `1-projects` folder of the vault you are running in. List the
   active projects you found, one line each, maximum seven lines. If there are more than
   seven, say how many you skipped and pick the seven most recently modified.

2. Give me one sentence on where those projects stand. Base it on what you actually read.
   If the notes do not say enough to judge, say that instead of guessing.

3. Ask me these three questions ONE AT A TIME. Wait for my answer before asking the next.
   Do not batch them into a single message.
   - What did you finish since the last standup?
   - What is the one thing that matters most today?
   - What is in your way?

4. Write a new file at `2-areas/standups/YYYY-MM-DD-standup.md` using today's actual date.
   Create the `standups` folder if it does not exist. The file contains:
   - A heading with the date
   - `## Done`, `## Today`, `## Blockers`, filled from my three answers
   - `## Projects`, the list from step 1
   - A final line linking to yesterday's standup with a wikilink in double square
     brackets, whether or not that file exists yet

5. Tell me the exact path you saved, so I can open it in Obsidian.

RULES:
- If a standup file for today already exists, tell me and ask whether to append or replace.
  Never silently overwrite.
- Never invent an answer to any of the three questions. If I do not answer one, write
  "not answered" in that section.
- Keep the whole exchange under two minutes of my time. Short questions, no preamble.
```

**Then run it, live, and answer by voice.**

```
/standup
```

Hold the Handy key, speak each answer, release. Three questions, three spoken answers.

> "That is speech to text, a language model, an agent, and a note system, doing one job
> together. Four tools you installed ninety minutes ago."

**Open the result in Obsidian** and show the dated file, the three sections, and the
wikilink to a note that does not exist yet, sitting there waiting for tomorrow.

**Then read the frontmatter aloud and make the point that generalises:**

> "The description field is not documentation. It is how the agent decides whether this
> skill is relevant to what you just asked. A vague description gets a skill that never
> fires. Write it for the agent, not for yourself."

**If the skill does not appear:** run `/reset` to reload skills in the current session, or
restart `hermes`. Check the file is named `SKILL.md`, in capitals, and that the folder name
is `standup`.

---

# PART 4: The agent that never sleeps (28 min)

## 1:32-1:38 · Rent a server and get inside it (6 min)

**Why this segment exists, said before any command:**

> "Everything so far dies when you shut your laptop. An agent that only runs when you are
> at your desk is a tool. An agent that runs whether or not you are there is something
> else. It can check on things overnight, and you can reach it from a queue at the airport."

**The pick, and the reasoning, not just the answer.**

| Provider | Entry plan | Price | Notes |
|---|---|---|---|
| **DigitalOcean** | 1 vCPU, 1 GB RAM, 25 GB SSD, 1,000 GiB transfer | **6 USD a month** | **The class pick.** Singapore region, plain interface, billed per second so a destroyed server costs pennies |
| DigitalOcean, one size up | 1 vCPU, 2 GB RAM, 50 GB SSD | 12 USD a month | Take this if you want the agent's browser tool to work |
| Hetzner Cloud | Comparable shared vCPU plans | Cheaper than DigitalOcean | Genuinely cheaper and has a Singapore region. Identity verification on signup is stricter and has stopped students mid class before |

> **DigitalOcean prices verified against digitalocean.com/pricing/droplets on 2026-08-18.**
> Hetzner's exact figure is **not verified** and is deliberately not printed here. Check it
> on the day if you want to name it. See the prep checklist.

**Create it.** Ubuntu LTS, the 6 USD plan, **Singapore** region, and **SSH key**
authentication rather than a password.

**Make the key first, on your laptop.** Same command on both systems:

```
ssh-keygen -t ed25519 -C "hermes-server"
```

Press Enter three times. Then print the public half and paste it into DigitalOcean:

```
cat ~/.ssh/id_ed25519.pub
```

Windows 11 has the OpenSSH client built in, so this works in PowerShell with no extra
install.

> "Two files were just created. One ends in `.pub` and is safe to paste anywhere. The other
> has no extension and must never leave your machine. That is the entire concept."

**Connect:**

```
ssh root@YOUR-SERVER-IP
```

Type `yes` at the fingerprint prompt. The prompt changes. Run `pwd` and `ls`.

> "Same six commands. Different computer, four thousand kilometres away. That is the point
> of learning them in the first half hour."

---

## 1:38-1:46 · Install the agent on the server (8 min)

**Two prerequisites first,** because Ubuntu ships without them and the installer needs
them:

```bash
apt update && apt install -y git curl xz-utils
```

**Then the same install command as your laptop,** with one flag:

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash -s -- --skip-browser
```

> "`--skip-browser` skips Chromium. On a 1 GB server the browser tool is the one thing that
> will run you out of memory. If you want it, take the 12 dollar plan and drop the flag.
> Say what you are giving up: this agent cannot open web pages. It can still read your
> notes, run commands, and talk to you."

**Reload the shell and check:**

```bash
source ~/.bashrc
hermes doctor
```

**Give it the same OpenRouter key and the same model:**

```bash
hermes config set OPENROUTER_API_KEY sk-or-v1-paste-your-key-here
hermes model
```

**Give it something to read.** Build the same PARA skeleton on the server, then copy up the
one skill you wrote:

```bash
mkdir -p ~/secondBrain/{1-projects,2-areas,3-resources,4-archives}
mkdir -p ~/.hermes/skills/personal
```

Then from a **second terminal on your laptop**, not on the server:

```
scp -r ~/.hermes/skills/personal/standup root@YOUR-SERVER-IP:~/.hermes/skills/personal/
```

Windows 11 PowerShell uses the same `scp` command.

> "The skill you wrote an hour ago just moved to a different continent as a text file. That
> is what it means for a skill to be a file rather than a setting in an app."

**The honest caveat about the notes, said now rather than discovered later:**

> "The server does not have your vault. It has an empty PARA skeleton and one skill. Making
> your notes exist in both places is a sync problem, and sync problems are their own
> session. The homework does it with a private git repository, which is the right answer
> and takes twenty minutes when nobody is watching."

---

## 1:46-1:54 · Telegram, and why it is Telegram (8 min)

**Name the alternatives first, then dismiss them with reasons.** Hermes speaks to Telegram,
Discord, Slack, WhatsApp, Signal, SMS, Matrix, email and more. That is a menu, not an
answer.

| Channel | What it costs you to set up | Verdict |
|---|---|---|
| **Telegram** | A bot token from a chat with a bot. About 60 seconds | **The pick.** No business account, no server, no dedicated phone number, and the allowlist is a numeric id |
| Discord | A developer application and a server you own | Fine, more steps, and it drags a whole server into the picture |
| Slack | A workspace plus a bot token and an app token | Two tokens and an admin. Correct for a team, wrong for one person |
| WhatsApp | A Meta business account or a browser bridge | The most familiar app and the worst setup. Bridges break and accounts get flagged |
| **Signal** | `signal-cli`, a Java 17 runtime, and linking as a second device by QR code | **The privacy answer, and genuinely more work.** Say this out loud rather than pretending Telegram is private |
| SMS | A Twilio account and per message charges | It costs money per message and adds nothing |

> "Telegram wins on setup cost, not on privacy. A Telegram bot chat is not end to end
> encrypted. Telegram can read it. If that matters for what you send your agent, Signal is
> the answer and it is a longer evening. Choose deliberately."

**Make the bot.**

1. In Telegram, open a chat with **@BotFather**
2. Send `/newbot`
3. Give it a display name, then a username ending in `bot`
4. BotFather replies with a token that looks like `123456789:ABCdefGHI...`

**Get your own numeric id.** Message **@userinfobot**. It replies with a number. That number
is not your username.

**Wire it up on the server.** The wizard:

```bash
hermes gateway setup
```

Choose Telegram, paste the token, paste your numeric id. Or write it directly to
`~/.hermes/.env`:

```
TELEGRAM_BOT_TOKEN=123456789:ABCdefGHIjklMNOpqrSTUvwxYZ
TELEGRAM_ALLOWED_USERS=123456789
```

**Start it in the foreground first**, so the class can see it working before it disappears
into the background:

```bash
hermes gateway
```

**Then message the bot from your phone.** Screen share the phone.

```
Read 1-projects and tell me what is in there.
```

**The security segment, and it is not optional.** Put this on screen and read it:

> "That bot has a terminal on a computer you own. Anyone who has the token can control it,
> and anyone you allowlist can run commands on it. There is a setting called
> `GATEWAY_ALLOW_ALL_USERS`. **Never set it to true on a bot with terminal access.** Your
> allowlist is one number long today. Keep it that way until you have a reason."

Then the three rules, stated plainly:

| Rule | Why |
|---|---|
| The bot token goes in `.env` and nowhere else | Anyone with the token owns the bot. If it leaks, `/revoke` in BotFather |
| `TELEGRAM_ALLOWED_USERS` contains only your own id | An unallowlisted stranger who finds your bot gets nothing |
| Do not put anything on that server you would mind losing | It is a six dollar computer on the public internet |

---

## 1:54-1:58 · Make it survive a reboot (4 min)

Foreground is a demo. A service is the deliverable.

```bash
hermes gateway install
sudo loginctl enable-linger $USER
hermes gateway status
```

`enable-linger` is what makes a user service start at boot rather than at login, which
matters on a machine nobody logs into. Then prove it:

```bash
reboot
```

Wait forty seconds, message the bot from the phone again without touching the server. It
answers.

> "Nobody logged in. Nobody opened a laptop. That is the difference between a script and a
> service, and it is the whole reason this segment exists."

**Show the logs once**, so they know where to look when it breaks:

```bash
journalctl --user -u hermes-gateway -f
```

**And show how to stop the bill:**

> "Destroy the droplet in the DigitalOcean panel when you are done. Billing is per second.
> If you rebuild it next week, everything you just typed is fifteen minutes of work, because
> all of it was commands, and commands can be saved."

---

## 1:58-2:00 · Close and homework (2 min)

Recap in one breath: four tools, one folder, one model, one skill, one server, one bot.

Then the caveat that makes the rest credible:

> "You did not build a private AI today. Handy is private. Your notes are private. The model
> is not, and the server is a rented computer, and the Telegram chat is readable by
> Telegram. What you built is an agent you control, on infrastructure you chose, with every
> piece replaceable. Privacy is Day 5 and it is a real session, not a footnote. Knowing
> exactly which parts are private is worth more than believing all of it is."

Brief the homework, point at the quiz, stop talking.

---

## Homework

1. Run `/standup` on three consecutive mornings. **Bring the third note**, not the first.
2. Put your vault in a private git repository and clone it onto the server, so the agent
   there reads the same notes. Twenty minutes when nobody is watching.
3. Write a second skill. Anything. `/weekly-review`, `/inbox`, `/log`. Check the name
   against the built in slash commands before you write a line.
4. Message your agent from your phone at least once when you are not at your desk. Report
   what you asked and whether the answer was worth the six dollars.
5. Go back to https://openrouter.ai/models, filter for tool support, and find a model
   cheaper than the class pick that still works. Report what broke, or that nothing did.
6. Destroy your droplet, then rebuild it from scratch using only the student guide. Time
   yourself. If it takes more than twenty minutes, tell us which step was ambiguous.

---

## Prep checklist for the host

Everything on this list has to be verified the morning of the stream, not the week before.

| Item | Status | Action |
|---|---|---|
| The three OpenRouter model prices in the 0:56 table | **Verified 2026-08-18 against the OpenRouter models API** | Re-check on the day. Prices move |
| DigitalOcean 6 USD plan specification and price | **Verified 2026-08-18 against digitalocean.com/pricing/droplets** | Re-check on the day |
| Hetzner's exact entry price | **Unknown, not verified** | Either verify from Hetzner's own console before the stream, or say "cheaper than DigitalOcean" and do not name a figure |
| Hermes minimum RAM on a headless server | **Unverified, secondhand.** 1 GB is reported to work without browser tools | Test the 6 USD droplet end to end at least once before the stream. If it swaps or dies, promote the 12 USD plan to the class pick |
| `hermes gateway install` on Windows 11 | Documented as using Scheduled Tasks | Not exercised in this module, since the gateway runs on the server. Do not demo it |
| Luma URL and GitHub student guide URL | **Both 404 on 2026-08-18** | See `luma-description.md`. Do not publish until both load |
| A pre built droplet in reserve | Not built | Build one the night before, with the agent already installed and the bot already running. See the agenda contingency |
| The vault used on camera | | Use a vault with real content. A two note vault makes the standup skill look pointless |

---

## What is confirmed and what is not

### Confirmed against primary sources, read 2026-08-18

- Hermes install commands, both platforms, and the `--skip-browser` flag, from the Hermes
  installation documentation
- Hermes data directory locations on macOS and native Windows, from the Windows native guide
- Skill folder layout, `SKILL.md` frontmatter fields, and slash command naming, from the
  Hermes skills guide
- `hermes model`, `hermes config set OPENROUTER_API_KEY`, and
  `/model <id> --provider openrouter --global`, from the Hermes model configuration guide
- Telegram setup: BotFather, `@userinfobot`, `hermes gateway setup`, `TELEGRAM_BOT_TOKEN`,
  `TELEGRAM_ALLOWED_USERS`, and the warning against `GATEWAY_ALLOW_ALL_USERS=true`, from the
  Hermes Telegram documentation and the team Telegram assistant guide
- `hermes gateway install` plus `loginctl enable-linger`, and the `journalctl --user`
  log command, from the Hermes messaging gateway documentation
- Signal requiring `signal-cli`, Java 17 or later, and QR linking as a second device, from
  the Hermes Signal documentation
- OpenRouter model prices and context lengths, queried directly from the OpenRouter models
  API on 2026-08-18. 328 paid models carried tool support on that date
- OpenRouter free tier limits of 20 requests per minute and 50 per day, rising to 1,000 per
  day above 10 USD of purchases, from the OpenRouter rate limit documentation
- DigitalOcean Basic Droplet pricing and the existence of a Singapore region, from
  DigitalOcean's own pricing page
- The course page URL pattern `myargoquest.com/courses/ai-power-users/module-<n>`, checked
  with curl. Module 0, 1 and 2 return 200

### Not confirmed

- **Hetzner's current entry price.** Secondhand sources disagree and a price change landed
  in April 2026. Not printed anywhere a student will read it
- **The minimum RAM for Hermes on a headless server.** The 1 GB figure comes from third
  party write ups, not from Nous Research. Must be tested before the stream
- **Whether `hermes gateway setup` on the server can complete over a plain SSH session
  without a browser.** The wizard is arrow key driven, which is fine, but if any step
  attempts an OAuth redirect the manual `.env` route is the fallback. The `.env` route is
  documented and is written into the student guide for exactly this reason

### Open questions, not researched

- Whether Handy's Whisper models handle Khmer well enough at the Small size for students on
  older machines. Module 0 raised this and it is still untested
- Whether a private git repository is the right sync answer for the vault, or whether
  `hermes profile export` and `hermes import` do the job better. The homework says git
  because git is teachable in twenty minutes

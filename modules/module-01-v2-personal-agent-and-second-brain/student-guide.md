# Module 1 V2 Student Guide: Setting Up Your Personal AI Agent and Second Brain

**Follow along here.** Every step, every command, every link from the session.
**Two hours. macOS and Windows 11.** Where the two differ, both are given.
**From Argo, hosted by Konrad Gnat. myargoquest.com**

No technical background needed. You will not write code. You will paste commands, and every
one of them is explained.

---

# Before the session

## Install these three

| App | Download from |
|---|---|
| **Handy** | https://handy.computer |
| **Obsidian** | https://obsidian.md |
| **VS Code** | https://code.visualstudio.com |

Then **open Handy, go to Models, and download a Whisper model.** That download is over a
gigabyte. It will not finish on venue wifi with thirty people trying at once.

## Create these two accounts

| Account | Why | Cost |
|---|---|---|
| **OpenRouter**, https://openrouter.ai | One key, several hundred models behind it | **Add 5 USD of credit before you arrive** |
| **DigitalOcean**, https://www.digitalocean.com | The server your agent lives on at the end | 6 USD a month, billed per second. You can destroy it the same day |

Also install **Telegram** on your phone, https://telegram.org. You will not need an account
with anyone else.

## What you need

- A laptop running **macOS** or **Windows 11**
- A microphone, built in is fine
- About **5 GB** free disk
- A payment card for the two accounts above

## What this costs, honestly

| Thing | Cost |
|---|---|
| Handy, Obsidian, VS Code, Hermes | Free |
| OpenRouter credit | **5 USD, and it will last you months.** One agent session is roughly half a cent |
| The server | **6 USD a month.** Billed per second, so a server you destroy after the session costs a few cents |

---

# What you are building

```
   YOUR LAPTOP                                  A SERVER YOU RENT
   ───────────                                  ─────────────────
   Handy        talk instead of type
      |
      v
   secondBrain/   one folder                    secondBrain/   the same shape
      1-projects                                   1-projects
      2-areas                                      2-areas
      3-resources                                  3-resources
      4-archives                                   4-archives
      |                                               |
   Obsidian   read and link it                    Hermes agent, always on
   VS Code    inspect the files                       |
   Hermes     an agent living in it                   v
      |                                            Telegram
      v                                               |
   OpenRouter   one model, one key  <────────────────-+
                                                      |
                                                      v
                                                  YOUR PHONE
```

**By the end you have:** a `/standup` skill you wrote, and a Telegram bot that is your own
agent, running on your own server, answering from your notes.

---

# 1. The terminal, in ten minutes

## Open it

| | macOS | Windows 11 |
|---|---|---|
| Open it | `Cmd+Space`, type `terminal`, Enter | Press `Windows`, type `terminal`, Enter |
| You are running | zsh | PowerShell |
| The prompt looks like | `you@Mac ~ %` | `PS C:\Users\you>` |

## The six commands

| Job | macOS | Windows 11 |
|---|---|---|
| Where am I | `pwd` | `pwd` |
| What is in here | `ls` | `ls` |
| Go into a folder | `cd Documents` | `cd Documents` |
| Go up one level | `cd ..` | `cd ..` |
| Go home | `cd ~` | `cd ~` |
| Clear the screen | `Cmd+K` | `cls` |

## The two keys that matter more than the six commands

- **Tab** completes a name you have started typing. Use it constantly. It kills typos.
- **Up arrow** brings back your last command.

**`Ctrl+C` stops whatever is running.** Nothing in this guide can hurt your laptop.

## Reading a path

```
macOS:       /Users/you/secondBrain/1-projects
Windows 11:  C:\Users\you\Documents\secondBrain\1-projects
```

Nested drawers. A slash means "go inside". `~` is shorthand for your home folder.

## Copy and paste, and the one gotcha

| Action | macOS | Windows 11 |
|---|---|---|
| Copy | `Cmd+C` | `Ctrl+C` |
| Paste | `Cmd+V` | `Ctrl+V` |

**Windows only:** in the terminal, `Ctrl+C` copies **only when text is selected**. With
nothing selected it stops the running command instead. `Ctrl+V` always pastes.

## The bridge between the terminal and your file browser

They are two windows onto the same folder.

| Direction | macOS | Windows 11 |
|---|---|---|
| Terminal to file browser | `open .` | `start .` |
| File browser to terminal | Right click the folder, Services, New Terminal at Folder | Right click inside the folder, Open in Terminal |

## Windows 11 only: do this now

File Explorer, **View**, **Show**, tick:

- **File name extensions**
- **Hidden items**

Windows hides the end of filenames by default. It shows `note` when the file is really
`note.md`. Most beginner confusion with files starts here.

Useful File Explorer keys while you are in there:

| Action | Keys |
|---|---|
| Open File Explorer | `Win+E` |
| New folder | `Ctrl+Shift+N` |
| Rename a file | `F2` |
| Up one level | `Alt+Up` |
| Address bar | `Ctrl+L` |

And a Windows habit worth having: `Alt+Tab` switches applications, `Ctrl+Tab` switches tabs
inside one, `Win+Left` and `Win+Right` snap windows side by side.

---

# 2. Install the four tools

| Tool | What it does | Free | Open source |
|---|---|---|---|
| **Handy** | Offline speech to text. Hold a key, talk, and it types wherever your cursor is | Yes | Yes, MIT |
| **Obsidian** | Turns a folder of plain text files into a linked, searchable knowledge base | Yes for personal use | No |
| **VS Code** | A code editor for inspecting the actual files: your skills, your config, anything the agent changed | Yes | Code is MIT. Microsoft's build adds telemetry |
| **Hermes** | The AI agent. Lives in your folder, reads and writes your notes, runs skills | Yes | Yes, MIT |

## Handy, Obsidian and VS Code

Download and install from their websites. They are normal applications.

- https://handy.computer
- https://obsidian.md
- https://code.visualstudio.com

## Hermes

**macOS**, in the terminal:

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
```

**Windows 11**, in PowerShell:

```powershell
iex (irm https://hermes-agent.nousresearch.com/install.ps1)
```

The installer handles Python, Node.js, ripgrep and ffmpeg for you. You do not need to know
what any of those are.

**Then open a brand new terminal window** and run:

```
hermes doctor
```

`hermes doctor` tells you what is missing and how to fix it. Run it any time something is
wrong for the rest of this course.

> If you get `hermes: command not found`, you are in the old terminal window. Close it and
> open a new one. On macOS you can also run `source ~/.zshrc`.

## A word on Obsidian not being open source

It is the one closed source tool here, and we use it anyway, because your notes are plain
text files in a normal folder. If Obsidian disappeared tomorrow, every note still opens in
Notepad. The question that matters is not the licence. It is what happens to your files if
the company dies.

---

# 3. Handy: talk instead of type

## Pick a model

Handy, **Models**, download a **Whisper** model.

Whisper supports 99 or more languages, including Khmer and Malay. **Whisper Large** is the
most accurate. **Whisper Turbo** or **Small** are faster on older machines.

## Grant the permissions

- **macOS:** microphone **and** accessibility. Without accessibility Handy cannot type for
  you, and it will look broken.
- **Windows 11:** microphone.

## Set a push to talk key

**Settings**, then **Shortcuts**. Click the shortcut and press the keys you want.

| Mode | How it works |
|---|---|
| **Push to talk** | Hold the key, speak, release. Recommended |
| **Toggle** | Press to start, press again to stop |

## Prove it works everywhere

Dictate one sentence into three different places: your terminal, a browser address bar, and
a text editor. **It types wherever your cursor is**, in any application. It is a keyboard,
not a transcription app.

## Two settings worth 60 seconds

- **Advanced**, then **Custom Words**: add names and terms it keeps getting wrong. Put your
  own name in now.
- **History**, in the settings sidebar: every past transcription, with its audio, and a one
  click copy back to your clipboard. This is where lost text goes.

Everything here runs on your laptop. Your voice does not reach a company and it works with
the wifi off. The limit is that on an older machine the accurate model will be slow, so you
will trade accuracy for speed. That is a real trade.

---

# 4. Build your second brain with PARA

## The four buckets

| Folder | What goes in it | The test |
|---|---|---|
| `1-projects` | Has an outcome and an end | Can I tick this off? |
| `2-areas` | Ongoing, no end date | Do I do this forever? |
| `3-resources` | Reference material | Would I want this later? |
| `4-archives` | Finished or dormant | Is this done or dead? |

**PARA sorts by how soon you must act, not by subject.** "Learn Khmer" is an Area. "Pass the
Khmer exam in March" is a Project. Same topic, different bucket.

## Create the folder

**macOS:**

```bash
mkdir -p ~/secondBrain/{1-projects,2-areas,3-resources,4-archives}
cd ~/secondBrain
ls
```

**Windows 11:**

```powershell
cd ~\Documents
mkdir secondBrain
cd secondBrain
mkdir 1-projects, 2-areas, 3-resources, 4-archives
ls
```

**Windows 11 only, one extra step.** Right click `secondBrain` in File Explorer and choose
**Always keep on this device**. OneDrive otherwise leaves a cloud placeholder on disk, and
an agent cannot read a placeholder.

## Open it as an Obsidian vault

Obsidian, **Open folder as vault**, pick `secondBrain`.

Nothing was imported. Nothing was converted. **A vault is just a folder.**

## Write your first real note

In `1-projects`, create a note named after something you are actually working on. Write two
sentences. **Dictate them with Handy.**

Your agent is going to read this note in about half an hour, so make it real.

---

# 5. Obsidian: the three features that compound

## Detect all file extensions

**Settings**, **Files and links**, turn on **Detect all file extensions**.

By default Obsidian shows markdown and hides everything else. Turn this on and your images,
PDFs and audio appear in the sidebar too.

## Links

Type `[[` and Obsidian autocompletes.

```markdown
[[Note Name]]                  link to a note
[[Note Name|display text]]     link with a different label
[[Note Name#Heading]]          link to a section
![[Note Name]]                 embed that note's content here
![[image.png|400]]             embed an image, 400px wide
```

**Linking to a note that does not exist yet is a feature.** Obsidian creates it the moment
you click. Write the link first, fill the note in later.

## Backlinks

Every note automatically shows **which other notes link to it**, in the Backlinks pane. You
never maintain this. It is worked out from your files.

Below it, **Unlinked mentions** finds notes that mention this note's title without linking,
and offers to link them in one click.

This is why a vault compounds and a folder does not. In a folder you have to remember where
you filed something. Here, things find each other.

## Shortcuts

| Action | macOS | Windows 11 |
|---|---|---|
| New note | `Cmd+N` | `Ctrl+N` |
| Jump to any note | `Cmd+O` | `Ctrl+O` |
| Every command by name | `Cmd+P` | `Ctrl+P` |
| Search the whole vault | `Cmd+Shift+F` | `Ctrl+Shift+F` |
| Toggle edit and reading view | `Cmd+E` | `Ctrl+E` |

---

# 6. VS Code: the same folder underneath

From your terminal, standing in `secondBrain`:

```
code .
```

> **macOS, if `code` is not found:** open VS Code, press `Cmd+Shift+P`, and run
> **Shell Command: Install 'code' command in PATH**. Then try again in a new terminal.

Third window, same folder. **Obsidian shows your thinking. VS Code shows the files.** Open
this window when something is wrong, when a file is hidden, or when you want to see exactly
what your agent changed.

## The terminal inside VS Code

Press `` Ctrl+` `` and run `pwd`. It is already standing in `secondBrain`. Files on the
left, terminal underneath, one folder. That layout is where you will run your agent.

## Install one extension

`Cmd+Shift+X` on macOS, `Ctrl+Shift+X` on Windows. Search **Markdown All in One**, click
**Install**.

A search box and a button. That is all a plugin is.

---

# 7. OpenRouter: one account, one key, one model

## Get the key

1. Sign up at **https://openrouter.ai**
2. Go to **Credits** and add **5 USD**
3. Go to **Keys** and create one. It starts `sk-or-v1-`. **It is shown once.** Copy it
   somewhere safe for the next ten minutes

## How to choose a model, which matters more than which one we chose

Go to **https://openrouter.ai/models** and filter in this order.

| Order | Filter | Why it is in this position |
|---|---|---|
| **1** | **Supports tools** | Non negotiable. An agent that cannot call tools cannot read or write a file. A model without tool support will chat happily and do nothing at all, which is the most confusing failure you can hit |
| **2** | **Context length** | The agent re-reads the conversation every turn. Under about 100,000 tokens you will hit the ceiling in a long session |
| **3** | **Price per million tokens** | Two numbers, input and output. Output normally costs three to five times input |
| **4** | Speed | Last. A model that is right slowly beats one that is wrong instantly |

## What one session costs

A working session where the agent reads a few notes and writes one is roughly **50,000
tokens in and 5,000 out**. At the prices below that is about **half a cent**. Five dollars
is somewhere near a thousand of those sessions.

## The class pick

Everyone on the same model, so that when something breaks we are all breaking the same way.

| Model | Input per 1M tokens | Output per 1M tokens | Context | When to pick it |
|---|---|---|---|---|
| **`z-ai/glm-4.7-flash`** | **0.06 USD** | **0.40 USD** | **202,752** | **The class default.** Built for tool calling loops |
| `qwen/qwen3.5-flash-02-23` | 0.065 USD | 0.26 USD | 1,000,000 | Cheaper output, far more context. Pick it if your vault is large |
| `deepseek/deepseek-v4-flash` | 0.083 USD | 0.165 USD | 1,048,576 | Cheapest output of the three. Pick it if your agent writes long answers |

> **Prices verified against the OpenRouter models API on 2026-08-18.** There were 328 paid
> models with tool support that day. Prices move. Check the site rather than trusting this
> table in three months. Looking it up instead of memorising it is the skill.

## The free option, and why it is not the default

OpenRouter has models whose id ends in `:free`, such as
`nvidia/nemotron-3.5-lightning:free`.

**Free accounts are limited to 20 requests per minute and 50 requests per day**, rising to
1,000 per day once you have bought at least 10 USD of credit. One agent turn is several
requests, so 50 a day is a handful of sessions. Free is for someone who genuinely cannot
pay, not for someone saving five dollars.

## The privacy trade, stated plainly

**OpenRouter is not private.** Your prompt, and whatever note content the agent sends with
it, leaves your machine, passes through OpenRouter, and reaches whichever company actually
serves that model.

Handy is private. Your notes on disk are private. This part is not.

Running a model on your own hardware is a Day 5 session and it is a real answer. Today we
are buying reliability, which is a fair trade as long as you know you made it.

---

# 8. Run the agent inside your notes

The move that makes this a second brain and not a chatbot: **launch the agent from inside
the folder**, so its working directory is your notes.

**macOS:**

```bash
cd ~/secondBrain
hermes
```

**Windows 11:**

```powershell
cd ~\Documents\secondBrain
hermes
```

## Connect it to OpenRouter

**The wizard, which is what most people should use.** Inside Hermes or from the terminal:

```
hermes model
```

Pick **OpenRouter** in the left column, paste your key when asked, then pick
`z-ai/glm-4.7-flash` from the right column.

**The direct way, if the wizard misbehaves:**

```
hermes config set OPENROUTER_API_KEY sk-or-v1-paste-your-key-here
```

Then inside a Hermes session:

```
/model z-ai/glm-4.7-flash --provider openrouter --global
```

`--global` saves the choice as well as switching the running session. Without it, the change
lasts until you close the window.

## Where everything lives

You will need this table three more times today.

| | macOS | Windows 11 |
|---|---|---|
| Config and data | `~/.hermes/` | `%LOCALAPPDATA%\hermes\` |
| Config file | `~/.hermes/config.yaml` | `%LOCALAPPDATA%\hermes\config.yaml` |
| Secrets | `~/.hermes/.env` | `%LOCALAPPDATA%\hermes\.env` |
| Skills | `~/.hermes/skills/` | `%LOCALAPPDATA%\hermes\skills\` |

## Prove it

Paste this into Hermes:

```
Read the note in 1-projects and tell me in one sentence what it is about. Then add a
line at the bottom of that note saying when you read it.
```

Switch to Obsidian and watch the line appear in the file.

**That is the whole thing.** Your model, your notes, your folder, and nothing in between.
Everything from here is making it more convenient.

---

# 9. Your first skill: `/standup`

A **skill** is a set of instructions you save once and run any time by name. Hermes reads
skills from a folder, and the folder name becomes the slash command.

## Check the name first

Hermes has built in slash commands: `/model`, `/reset`, `/sessions`, `/usage`, `/profile`,
`/status`, `/context`, `/tools`, `/skills`, `/help`.

**A skill that shadows a built in fails silently**, which is the worst kind of failure.
`/standup` is clear of all of them. Check your own skill names against this list before you
write a line.

## Create the folder

**macOS:**

```bash
mkdir -p ~/.hermes/skills/personal/standup
```

**Windows 11:**

```powershell
mkdir $env:LOCALAPPDATA\hermes\skills\personal\standup
```

## Paste this brief into your agent

This is the most valuable text in the module. Paste it whole.

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

## Run it, and answer by voice

```
/standup
```

Hold your Handy push to talk key, speak each answer, release. Three questions, three spoken
answers.

Then open `2-areas/standups/` in Obsidian. There is today's note, written by an agent, from
your own voice, with a wikilink to tomorrow's note sitting there waiting.

## Why the description field matters

Read the frontmatter your agent wrote. The `description` is not documentation. **It is how
the agent decides whether this skill is relevant to what you just asked.** A vague
description gets a skill that never fires. Write it for the agent, not for yourself.

## If the skill does not appear

- Run `/reset` to reload skills in the current session, or restart `hermes`
- Check the file is named `SKILL.md`, in capitals
- Check the folder is named `standup`, lowercase, no spaces

---

# 10. Rent a server and get inside it

Everything so far dies when you shut your laptop. An agent that only runs when you are at
your desk is a tool. An agent that runs whether or not you are there is something else.

## Which server, and why

| Provider | Entry plan | Price | Notes |
|---|---|---|---|
| **DigitalOcean** | 1 vCPU, 1 GB RAM, 25 GB SSD, 1,000 GiB transfer | **6 USD a month** | **What this guide uses.** Singapore region, plain interface, billed per second |
| DigitalOcean, one size up | 1 vCPU, 2 GB RAM, 50 GB SSD | 12 USD a month | Take this if you want the agent's browser tool to work |
| Hetzner Cloud | Comparable shared vCPU plans | Cheaper than DigitalOcean | Genuinely cheaper, and it has a Singapore region. Identity verification on signup is stricter and slower |

> DigitalOcean prices verified against digitalocean.com/pricing/droplets on 2026-08-18.

**Billing is per second.** A server you create at the start of this section and destroy at
the end costs a few cents.

## Make an SSH key first

An SSH key is two files. One is safe to paste anywhere, the other must never leave your
machine.

Same command on macOS and Windows 11:

```
ssh-keygen -t ed25519 -C "hermes-server"
```

Press Enter three times to accept the defaults. Then print the **public** half:

```
cat ~/.ssh/id_ed25519.pub
```

Copy the whole line, starting `ssh-ed25519`.

> Windows 11 has the OpenSSH client built in. `ssh`, `ssh-keygen` and `scp` all work in
> PowerShell with nothing extra installed.

## Create the droplet

In DigitalOcean, **Create**, **Droplets**:

| Setting | Choose |
|---|---|
| Image | **Ubuntu**, the LTS version offered |
| Plan | **Basic**, **Regular**, the **6 USD a month** size |
| Region | **Singapore**, or the one nearest you |
| Authentication | **SSH Key**, and paste the public key you just copied |
| Hostname | Anything. `hermes-1` is fine |

## Connect

```
ssh root@YOUR-SERVER-IP
```

Type `yes` at the fingerprint prompt. The prompt changes. Run `pwd` and `ls`.

Same six commands you learned in the first ten minutes. Different computer.

---

# 11. Install the agent on the server

## Two prerequisites Ubuntu ships without

```bash
apt update && apt install -y git curl xz-utils
```

## The same install command, with one flag

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash -s -- --skip-browser
```

**What `--skip-browser` gives up:** the agent cannot open web pages. Chromium is the one
thing that will run a 1 GB server out of memory. If you want the browser tool, take the
12 USD plan and drop the flag.

## Check it, and connect it

```bash
source ~/.bashrc
hermes doctor
hermes config set OPENROUTER_API_KEY sk-or-v1-paste-your-key-here
hermes model
```

Pick OpenRouter and the same model as your laptop.

## Give it a folder and your skill

On the server:

```bash
mkdir -p ~/secondBrain/{1-projects,2-areas,3-resources,4-archives}
mkdir -p ~/.hermes/skills/personal
```

Then, **from a second terminal on your laptop**, not on the server:

```
scp -r ~/.hermes/skills/personal/standup root@YOUR-SERVER-IP:~/.hermes/skills/personal/
```

The skill you wrote an hour ago just moved to a different continent, because a skill is a
text file rather than a setting inside an app.

> **The server does not have your notes.** It has an empty PARA skeleton and one skill.
> Getting your vault onto it is a sync problem, and sync problems are their own session. The
> homework does it with a private git repository, which is the right answer and takes about
> twenty minutes.

---

# 12. Telegram: reach your agent from your phone

## Why Telegram, and what you are giving up

Hermes speaks to Telegram, Discord, Slack, WhatsApp, Signal, SMS, Matrix and email. That is
a menu, not an answer.

| Channel | What it costs you to set up | Verdict |
|---|---|---|
| **Telegram** | A bot token from a chat with a bot. About 60 seconds | **The pick.** No business account, no server to own, no second phone number |
| Discord | A developer application and a server you own | Fine, more steps |
| Slack | A workspace plus two separate tokens | Right for a team, wrong for one person |
| WhatsApp | A Meta business account or a browser bridge | Most familiar app, worst setup. Bridges break |
| **Signal** | `signal-cli`, a Java 17 runtime, and linking as a second device by QR code | **The privacy answer, and genuinely more work** |
| SMS | A Twilio account and per message charges | Costs money per message and adds nothing |

**Telegram wins on setup cost, not on privacy.** A Telegram bot chat is not end to end
encrypted. Telegram can read it. If that matters for what you send your agent, Signal is the
answer and it is a longer evening.

## Make the bot

1. In Telegram, open a chat with **@BotFather**
2. Send `/newbot`
3. Give it a display name, then a username **ending in `bot`**
4. BotFather replies with a token like `123456789:ABCdefGHIjklMNOpqrSTUvwxYZ`

**Keep the token secret.** Anyone with it controls your bot. If it leaks, send `/revoke` to
BotFather and get a new one.

## Get your numeric user ID

Message **@userinfobot**. It replies with a number like `123456789`.

**That number is not your username.** The allowlist uses the number.

## Wire it up, on the server

The wizard:

```bash
hermes gateway setup
```

Choose Telegram, paste the token, paste your numeric id.

Or write it straight into `~/.hermes/.env`:

```
TELEGRAM_BOT_TOKEN=123456789:ABCdefGHIjklMNOpqrSTUvwxYZ
TELEGRAM_ALLOWED_USERS=123456789
```

## Start it and message it

```bash
hermes gateway
```

That runs in the foreground so you can watch it. Now open Telegram on your phone and message
your bot:

```
Read 1-projects and tell me what is in there.
```

## Read this before you go any further

That bot has a terminal on a computer you own.

| Rule | Why |
|---|---|
| The bot token lives in `.env` and nowhere else | Anyone with the token owns the bot |
| `TELEGRAM_ALLOWED_USERS` contains only your own id | An unallowlisted stranger who finds your bot gets nothing |
| **Never set `GATEWAY_ALLOW_ALL_USERS=true`** on a bot with terminal access | Anyone who finds your bot could then run commands on your server |
| Do not put anything on that server you would mind losing | It is a six dollar computer on the public internet |

---

# 13. Make it survive a reboot

Foreground is a demo. A service is the deliverable.

```bash
hermes gateway install
sudo loginctl enable-linger $USER
hermes gateway status
```

`enable-linger` is what makes a user service start at **boot** rather than at **login**,
which matters on a machine nobody ever logs into.

## Prove it

```bash
reboot
```

Wait about forty seconds, then message your bot from your phone without touching the server.
It answers.

## Where the logs are

```bash
journalctl --user -u hermes-gateway -f
```

Press `Ctrl+C` to stop watching.

## Managing the service

```bash
hermes gateway status
hermes gateway stop
hermes gateway start
hermes gateway restart
hermes gateway uninstall
```

## Stop the bill

Destroy the droplet in the DigitalOcean panel when you are done. Billing is per second. If
you rebuild it next week, everything in sections 10 to 13 is about fifteen minutes of work,
because all of it was commands, and commands can be saved.

---

# You did it

- **Handy**, offline speech to text on both operating systems
- **A PARA vault**, one folder, open in Obsidian and VS Code at once
- **Hermes**, running inside your notes, on one deliberately chosen OpenRouter model
- **`/standup`**, a skill you wrote, that you will actually use
- **A server you rent**, with the same agent on it
- **A Telegram bot** on your phone that is your agent, and survives a reboot

## What is private and what is not

Being precise about this is worth more than believing all of it is private.

| Piece | Private |
|---|---|
| Handy, your voice | **Yes.** It never leaves your machine and works offline |
| Your notes on disk | **Yes.** Plain files in a folder you own |
| The model, via OpenRouter | **No.** Prompts pass through OpenRouter and the upstream provider |
| The server | **Yours, not private.** A rented computer on the public internet |
| The Telegram chat | **No.** Bot chats are not end to end encrypted |

Running a model on your own hardware is a Day 5 session, and it is a real answer.

---

# Homework

1. Run `/standup` on three consecutive mornings. **Bring the third note**, not the first.
2. Put your vault in a private git repository and clone it onto the server, so the agent
   there reads the same notes.
3. Write a second skill. Anything. `/weekly-review`, `/inbox`, `/log`. **Check the name
   against the built in slash commands first.**
4. Message your agent from your phone at least once when you are not at your desk. Report
   what you asked and whether the answer was worth six dollars.
5. Go back to https://openrouter.ai/models, filter for tool support, and find a model
   cheaper than the class pick that still works. Report what broke, or that nothing did.
6. Destroy your droplet, then rebuild it from scratch using only this guide. Time yourself.
   If it takes more than twenty minutes, tell us which step was ambiguous.

---

# Troubleshooting

| Problem | Fix |
|---|---|
| `hermes: command not found` | Open a **new** terminal window. On macOS, `source ~/.zshrc`. On Windows, close and reopen PowerShell |
| Anything else wrong with Hermes | Run `hermes doctor` first. It names what is missing |
| The agent chats but never reads or writes a file | Your model does not support tools. Go back to section 7 and filter on **Supports tools** first |
| `API key not set` | `hermes config set OPENROUTER_API_KEY sk-or-v1-...` |
| The agent cannot see your notes | You launched it from the wrong folder. Run `pwd` and confirm you are inside `secondBrain` |
| `/standup` does nothing | Run `/reset`. Check the file is `SKILL.md` in capitals, in a folder named `standup` |
| Handy types nothing | macOS: grant **accessibility**, not just microphone. Both: check the model finished downloading |
| Windows: my file is called `note`, not `note.md` | File Explorer, View, Show, tick **File name extensions** |
| Windows: files show a cloud icon and open slowly | Right click `secondBrain`, **Always keep on this device** |
| Obsidian shows an empty vault | Wrong folder. **Open folder as vault** and pick `secondBrain` |
| `ssh: connection refused` | The droplet is still booting. Wait 60 seconds and try again |
| `Permission denied (publickey)` | The droplet was created with the wrong key. Recreate it, or add the key in the DigitalOcean console |
| The install fails on the server | You skipped `apt install -y git curl xz-utils` |
| The server runs out of memory | You dropped `--skip-browser` on the 1 GB plan. Reinstall with the flag, or resize to the 12 USD plan |
| The Telegram bot ignores you | Your numeric id is not in `TELEGRAM_ALLOWED_USERS`. Message `@userinfobot` again and check you used the number, not the username |
| The bot dies when you close the SSH session | You are still on `hermes gateway` in the foreground. Run `hermes gateway install` |

---

# Every link in one place

**Tools**

- Handy: https://handy.computer
- Obsidian: https://obsidian.md
- VS Code: https://code.visualstudio.com
- VSCodium, the fully open build: https://vscodium.com
- Hermes: https://hermes-agent.nousresearch.com
- Hermes documentation: https://hermes-agent.nousresearch.com/docs

**Accounts**

- OpenRouter: https://openrouter.ai
- OpenRouter models: https://openrouter.ai/models
- DigitalOcean: https://www.digitalocean.com
- DigitalOcean droplet pricing: https://www.digitalocean.com/pricing/droplets
- Telegram: https://telegram.org

**Reference**

- PARA method: https://fortelabs.com/blog/para
- Hermes Telegram setup: https://hermes-agent.nousresearch.com/docs/user-guide/messaging/telegram
- Hermes messaging gateway: https://hermes-agent.nousresearch.com/docs/user-guide/messaging
- Hermes Windows guide: https://hermes-agent.nousresearch.com/docs/user-guide/windows-native

**The course**

- Course page: https://myargoquest.com/courses/ai-power-users
- Argo: https://myargoquest.com

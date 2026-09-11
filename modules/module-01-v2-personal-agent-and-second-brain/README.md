# Module 1 V2: Setting Up Your Personal AI Agent and Second Brain

**Module 1 V2 of AI Power Users · 120 minutes · live on YouTube · macOS and Windows 11**
**From Argo, myargoquest.com. Hosted by Konrad Gnat.**

The rebuilt entry point to the course. Module 0 and Module 1 folded into one two hour
session, with Windows 11 as a first class path rather than a footnote, and a different
ending: the agent does not stop when you shut your laptop.

This module costs the student money for the first time in the series. About 5 USD of
OpenRouter credit, which lasts months, and about 6 USD a month for a server that can be
destroyed the same day for a few cents.

## What you build

```
   YOUR LAPTOP                                  A SERVER YOU RENT
   ───────────                                  ─────────────────
   Handy        talk instead of type
      |
      v
   secondBrain/   one folder                    secondBrain/   the same shape
      1-projects  2-areas                          1-projects  2-areas
      3-resources 4-archives                       3-resources 4-archives
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

## The one artifact

**A Telegram bot, on your phone, that is your own agent, running on your own server, holding
your own notes.** You send it a message from the street and it answers. It survives a
reboot without you touching anything.

## What changed from the first version of Module 1

| # | Change | Why |
|---|---|---|
| 1 | **Windows 11 is a first class path.** Module 0's terminal, File Explorer and shortcut material is folded in | A Windows student no longer needs a separate session before they can start |
| 2 | **OpenRouter is the provider, and we pick exactly one model** | The first version led with a private decentralized model and treated OpenRouter as a bonus. Students spent the session debugging a connection instead of building. Privacy is now taught properly on Day 5 |
| 3 | **The session ends on a server, not a laptop** | The old ending was a skill you could run when your laptop was open. This one is an agent that runs whether or not you are there |
| 4 | **Two hours instead of forty five minutes** | The old timing assumed everything installed in advance. It never was |

## Learning objectives

By the end of this module you can:

1. Read a filesystem path out loud and navigate to it from a terminal on either macOS or
   Windows 11
2. Install and configure local speech to text, and dictate into any application
3. Explain what PARA sorts by, and file a real note into the right bucket
4. Open one folder as an Obsidian vault and as a VS Code project, and say what each lens is
   for
5. Choose one model on OpenRouter by filtering on tool support first and price second, and
   say what one agent session costs
6. Launch an agent inside your notes folder and prove it can read and write those notes
7. Write a skill from scratch, in a file, and run it as a slash command
8. Rent a Linux server, connect to it over SSH with a key, and install an agent on it
9. Create a Telegram bot, allowlist yourself, and run the agent as a service that survives a
   reboot
10. State who can control an agent that has a terminal, and what stops them

## The four tools

| Tool | Role | Free | Open source |
|---|---|---|---|
| **Handy** | Local speech to text. Hold a key, talk, and it types wherever your cursor is | Yes | Yes, MIT |
| **Obsidian** | Read, link and think in your notes | Yes for personal use | No |
| **VS Code** | Look at what is actually in the files | Yes | Code is MIT. Microsoft's build adds telemetry |
| **Hermes** | The agent that lives in your folder and runs your skills | Yes | Yes, MIT |

Plus two accounts: **OpenRouter** for the model, and **DigitalOcean** for the server.

## The skill your agent writes

| Skill | Cadence | What it does | Why it matters |
|---|---|---|---|
| **`/standup`** | Every morning | Reads `1-projects`, lists what is active, asks three questions one at a time, and writes a dated note into `2-areas/standups/` with a wikilink to the next one | It is the first thing the agent does that a chatbot cannot: it reads your actual files, then writes one back. Two minutes, every day, and the vault fills itself |

> **Why the name is `/standup` and not `/daily`.** Hermes has built in slash commands and a
> skill that shadows one **fails silently**. Checking a skill name against the built in list
> before writing it is a habit, not a footnote.

## The six steps

| # | Step | Output |
|---|---|---|
| 1 | The terminal, both platforms | Six commands, tab completion, and no fear of the window |
| 2 | Install the four tools | `hermes doctor` runs clean |
| 3 | **Build the vault with PARA** | `secondBrain/`, open in Obsidian and VS Code |
| 4 | **OpenRouter, and one model** | A key, a deliberate model choice, and the cost of one session |
| 5 | **Write `/standup`** | A skill file you wrote, run by voice |
| 6 | **The server and the bot** | **A Telegram bot on your phone that survives a reboot** |

## The ideas that carry the module

**Filter on tool support before price.** An agent that cannot call tools will chat happily
and never touch a file. It looks broken and it is not, which makes it the most confusing
failure in the course. Everything else about model choice is secondary to this one filter.

**A vault is just a folder.** Nothing is imported and nothing is converted. That single fact
is why this whole stack has no lock in, and it is why the same skill file can be copied to a
server on another continent with `scp` and simply work.

**PARA sorts by actionability, not subject.** "Learn Khmer" is an Area. "Pass the Khmer exam
in March" is a Project. Same topic, different bucket, because the question is when you touch
it, not what it is about.

**A skill's description is written for the agent.** It is not documentation. It is how the
agent decides whether the skill is relevant to what you just asked. A vague description gets
a skill that never fires.

**A service is not a script.** `enable-linger` is what makes something start at boot rather
than at login. That distinction is the whole difference between an agent you run and an
agent that is simply there.

**Know exactly which parts are private.** Handy is private. Your notes are private. The
model is not, the server is rented, and Telegram bot chats are readable by Telegram. Being
precise about that is worth more than believing all of it is private.

## Files in this module

- [`syllabus.md`](./syllabus.md), the full plan: every segment, every brief, host guidance
- [`agenda.md`](./agenda.md), the 120 minute run of show and the contingency
- [`student-guide.md`](./student-guide.md), **follow along here**: every step, link and copy
  pasteable brief
- [`quiz.md`](./quiz.md), 5 multiple choice plus 4 open ended, peer evaluated
- [`luma-description.md`](./luma-description.md), the finished event copy
- [`image-prompts.md`](./image-prompts.md), the Luma square and YouTube livestream image
  prompts
- [`youtube-livestream.md`](./youtube-livestream.md), title, description, chapters and tags
  for the broadcast
- [`web-page.md`](./web-page.md), the draft for the module page on myargoquest.com

## Before the session

**No prior module required.** This is the entry point.

| Account | Why | Cost |
|---|---|---|
| [OpenRouter](https://openrouter.ai) | One key, several hundred models behind it | **Add 5 USD of credit before you arrive** |
| [DigitalOcean](https://www.digitalocean.com) | The server the agent ends up on | 6 USD a month, billed per second |

Install **Handy**, **Obsidian** and **VS Code** beforehand, and **open Handy once and
download a Whisper model**. That download is over a gigabyte and will not finish on venue
wifi with thirty people trying at once.

Also install **Telegram** on your phone.

**Bring:** a laptop running macOS or Windows 11, a microphone, about 5 GB free disk, a
payment card, and one real project you want your agent to track.

## Homework

1. Run `/standup` on three consecutive mornings. Bring the third note, not the first.
2. Put your vault in a private git repository and clone it onto the server, so the agent
   there reads the same notes.
3. Write a second skill. Check the name against the built in slash commands first.
4. Message your agent from your phone when you are not at your desk. Report whether the
   answer was worth six dollars.
5. Find a model cheaper than the class pick that still works. Report what broke, or that
   nothing did.
6. Destroy your droplet and rebuild it from the student guide alone. Time yourself.

## The honest caveat

You did not build a private AI today. Handy is private and your notes are private. The model
is not, the server is a computer you rent on the public internet, and the Telegram chat is
readable by Telegram.

What you built is an agent you control, on infrastructure you chose, with every piece
replaceable. Running a model on your own hardware is a Day 5 session and it is a real
answer, not a consolation prize.

The second caveat is smaller and will bite sooner. **The server does not have your notes.**
It has an empty PARA skeleton and one skill file. Getting your vault onto it is a sync
problem, sync problems are their own session, and the homework does it with a private git
repository in about twenty minutes.

# Module 1 V2 Agenda: Run of Show

**Module 1 V2 of AI Power Users · 120 minutes · no break · live on YouTube**
**Platform: macOS and Windows 11, taught side by side.**
Theme: four tools, one folder, one model, one skill, one server, one bot.

> **Host note.** There are two time sinks and they are both in the first half. Downloads,
> which the student guide tells people to do beforehand and half of them will not have, and
> the OpenRouter signup, where the card form will defeat somebody. **Protect 1:32 to 2:00
> for the server and Telegram.** A bot on a phone is the deliverable. If the clock slips,
> cut from Obsidian and VS Code, never from the ship.

---

## Part 1: The machine (0:00-0:32)

| Time | Segment | What happens | Attendee action |
|---|---|---|---|
| 0:00-0:04 | **Open with the payoff** | Message a Telegram bot from a phone on screen. It answers from a note. Then show the terminal on the server it came from. Say the cost out loud: 5 USD of credit, 6 USD a month for the server | Open the student guide |
| 0:04-0:16 | **The terminal, both platforms** | `pwd`, `ls`, `cd`, clear. **Tab completion and up arrow, taught hard.** `Ctrl+C` stops things. Read a path out loud in both punctuations. The bridge: `open .` and `start .`. **Windows: turn on file name extensions and hidden items now** | Open a terminal, run the six commands |
| 0:16-0:24 | **Install the four tools** | Kick off Handy, Obsidian and VS Code from their websites, then the Hermes one liner while they download. Explain each as it installs. Finish on `hermes doctor`. One line on why Obsidian being closed source is acceptable | Start all four installs |
| 0:24-0:32 | **Handy** | Whisper model, 99 or more languages including Khmer and Malay. Permissions, and that macOS needs accessibility or it looks broken. Push to talk key. **Dictate into three different applications.** Custom Words and the History tab | Pick a model, set a key, dictate |

## Part 2: The second brain (0:32-0:56)

| Time | Segment | What happens | Attendee action |
|---|---|---|---|
| 0:32-0:42 | **Build the vault with PARA** | The four buckets and the actionability test. Build it in the terminal on both platforms. **Windows: Always keep on this device.** Open folder as vault, nothing is imported. Write and dictate the first real note in `1-projects` | Create the folder, open the vault, write one note |
| 0:42-0:50 | **Obsidian, three features** | **Detect all file extensions.** `[[Links]]` and `![[embeds]]`, and that linking to a note that does not exist creates it. **Backlinks and unlinked mentions**, and why a vault compounds where a folder does not | Make two linked notes |
| 0:50-0:56 | **VS Code** | `code .` from the terminal, and the macOS PATH fix. Obsidian shows your thinking, VS Code shows the files. `` Ctrl+` `` for the built in terminal, already in the folder. Install Markdown All in One to show what a plugin is | Open the folder, install the extension |

## Part 3: The agent (0:56-1:32)

| Time | Segment | What happens | Attendee action |
|---|---|---|---|
| 0:56-1:06 | **OpenRouter, and one model** | Sign up, add 5 USD, create the key. Then **filter live on the models page in order: tool support, context, price, speed.** Do the arithmetic on camera: one session is about half a cent. Name the class pick and say the prices were checked today. The privacy trade, stated once and not softened | Create the account and the key |
| 1:06-1:16 | **Run the agent in your notes** | `cd` into the vault, then `hermes`. `hermes model` wizard, and the `hermes config set` fallback. Show the paths table for both platforms. **Prove it with a request that reads a note and edits it**, then watch the line appear in Obsidian | Launch and connect, run the proof |
| 1:16-1:32 | **BUILD BY ASKING: your first skill** | Check the name against the built in slash commands and say why a shadowed skill fails silently. Create the folder. **Read the brief on screen, verbatim.** Agent writes `SKILL.md`. Run `/standup` and **answer all three questions by voice with Handy.** Open the dated note in Obsidian. Read the description field aloud and say what it is really for | Paste the brief, run the skill, answer by voice |

## Part 4: The agent that never sleeps (1:32-2:00)

| Time | Segment | What happens | Attendee action |
|---|---|---|---|
| 1:32-1:38 | **Rent a server, get inside it** | Why: an agent that only runs when your laptop is open is a tool. `ssh-keygen`, and the two files, one safe to paste and one that never leaves the machine. Create the droplet: Ubuntu, 6 USD, Singapore, SSH key. `ssh root@IP`, then `pwd` and `ls`. Same six commands, different continent | Make the key, create the droplet, connect |
| 1:38-1:46 | **Install the agent on the server** | `apt install git curl xz-utils` first. The same install command with `--skip-browser`, and say what that gives up. `hermes doctor`, the key, the model. Build the PARA skeleton. **`scp` the skill up from a second terminal on the laptop** | Install, connect, copy the skill up |
| 1:46-1:54 | **Telegram, and why Telegram** | Run the channel comparison table and dismiss the alternatives with reasons, including that Signal is the privacy answer and is more work. BotFather, `/newbot`, the token. `@userinfobot` for the numeric id. `hermes gateway setup`, or the `.env` route. `hermes gateway` in the foreground. **Message it from the phone, screen shared.** Then the security block, read out loud, including never setting `GATEWAY_ALLOW_ALL_USERS=true` | Make the bot, wire it, message it |
| 1:54-1:58 | **Make it survive a reboot** | `hermes gateway install`, then `loginctl enable-linger` and why boot is not login. **`reboot` live, wait, message the bot from the phone again without touching the server.** Show `journalctl`. Show how to destroy the droplet and stop the bill | Install the service, reboot, message it |
| 1:58-2:00 | **Close** | Four tools, one folder, one model, one skill, one server, one bot. The private and not private table. Homework, quiz, stop | Note the homework |

---

## Materials on screen

- Student guide, pinned in chat for the whole session
- A terminal and a File Explorer or Finder window, side by side, from 0:04
- **A Windows 11 machine and a Mac, both visible.** If only one is available, run the other
  in a VM and switch to it for every command that differs
- handy.computer, obsidian.md, code.visualstudio.com
- https://openrouter.ai/models, with the filter panel open
- The DigitalOcean create droplet page
- A phone running Telegram, screen shared, from 1:46
- **A mature vault** with real content for the Obsidian segment. A two note vault makes
  backlinks and the standup skill look pointless

---

## Homework, briefed at 1:58

1. Run `/standup` on three consecutive mornings. Bring the third note, not the first.
2. Put your vault in a private git repository and clone it onto the server.
3. Write a second skill. Check the name against the built in slash commands first.
4. Message your agent from your phone when you are not at your desk. Report whether the
   answer was worth six dollars.
5. Find a model cheaper than the class pick that still works. Report what broke.
6. Destroy the droplet and rebuild it from the student guide alone. Time yourself.

---

## Contingency

Cut in this order. The named clock times are the decision points.

- **At 0:32 and behind:** cut the Handy demo from three applications to one, the terminal.
  Say on camera that it types anywhere and move on.
- **At 0:56 and behind:** cut the Obsidian segment to **Detect all file extensions** and
  backlinks only. Drop the links syntax block and the VS Code extension install. The student
  guide has both.
- **At 1:16 and behind:** do not shorten the skill brief. Instead, drop step 2 of the proof
  in the previous segment and let the skill run be the proof that the agent can write files.
- **At 1:32 and behind:** switch to the **pre built droplet in reserve**, with Hermes already
  installed and the bot already running. Narrate the create and install steps from the
  DigitalOcean panel and the student guide, and go straight to BotFather. The teaching in
  Part 4 is the token, the allowlist and the service, not watching `apt` run.
- **If the droplet will not boot or SSH refuses twice:** go to the reserve droplet
  immediately. Do not debug DNS or firewalls on stream.
- **If a student's card is declined at OpenRouter:** put them on
  `nvidia/nemotron-3.5-lightning:free` for the session, say the 50 requests a day limit out
  loud, and help in the chat. Do not stop the room.
- **Never cut:** the tool support filter at 0:56, the skill brief at 1:16, the security
  block at 1:46, and the reboot proof at 1:54. Those four are the module.

---

## Two things that will go wrong, and the answer

**Downloads.** Half the room will not have installed anything, whatever the Luma page said.
The four installs are staged at 0:16 precisely so they run in the background through Handy,
PARA and Obsidian. Do not wait for anyone. Say at 0:16 that the installs will still be
running at 0:50 and that is fine.

**The model that chats but does nothing.** Somebody will pick a model without tool support
and it will answer politely and never touch a file. This is the most confusing failure in
the course and it looks like the agent is broken. **Name it before it happens, at 0:56**, so
that when it happens in the chat you can point at it in one line instead of debugging it
live.

# Module 1 V2: YouTube Livestream

Copy and paste metadata for the YouTube live broadcast. No emojis, no em dashes, no model
names. The holding screen for OBS is generated from `image-prompts.md`.

---

## Title

**Primary, use this:**

> Build Your Own AI Agent and Second Brain (LIVE) | AI Power Users M1 V2

**Alternates:**

- Your Own AI Agent, On Your Own Server, In Your Pocket | AI Power Users M1 V2
- I Built an AI Agent That Texts Me From Its Own Server (Live, 2 Hours)
- AI Agent + Second Brain From Scratch, Mac and Windows 11 | AI Power Users

> Front load the hook before the series tag, and keep it under about 70 characters so it is
> not truncated on mobile. The primary is 69.

---

## Short description, for the first line and for social

Install a working AI agent inside your own notes, write its first skill, then put it on a
server you rent and text it from your phone. Two hours, live, from zero. macOS and Windows
11, both taught side by side. No coding experience required.

---

## Full YouTube description

```
Two hours, live, from nothing installed to an AI agent running on a server you own that
answers you on Telegram.

This is Module 1 V2 of AI Power Users, a hands on live course from Argo. It is the entry
point, so nothing before it is required, and the first ten minutes are the terminal from
scratch on both macOS and Windows 11.

FOLLOW ALONG: the free student guide has every command, both operating systems, and the
full skill brief you paste into your agent.
Student guide: [link to student-guide.md]
Course page: https://myargoquest.com/courses/ai-power-users
Register for the live cohort: [Luma link]

WHAT WE BUILD

Four free tools, one folder, one model, one skill, one server, one bot.

1. Local speech to text, so you can talk instead of type. Runs offline, works with the wifi
   off, types into any application.
2. A second brain: a folder of plain text files organised with PARA, opened as a vault in
   Obsidian and as a project in VS Code at the same time. Nothing is imported and nothing is
   converted, which is why there is no lock in.
3. An open source agent that runs inside that folder and can read and write the files in it.
4. A model, chosen live on camera by filtering on TOOL SUPPORT FIRST. This is the part most
   people get wrong. An agent on a model without tool support will chat happily and never
   touch a single file. It looks broken. It is not.
5. Your first skill, written into a file, run by voice: /standup reads your active projects,
   asks three questions one at a time, and writes a dated note back into your vault.
6. A Linux server you rent for six dollars a month, with the same agent installed on it and
   the skill you wrote copied up as a plain text file.
7. A Telegram bot that is your agent, allowlisted to exactly one person, that survives a
   reboot without you touching the server.

WHAT IT COSTS

About 5 USD of model credit, which lasts months. One agent session is roughly half a cent.
About 6 USD a month for the server, billed by the second, so a server you destroy at the end
of the session costs a few cents. The four tools are free.

THE HONEST PART

You are not building a private AI in this session. The speech to text is private and your
notes are private. The model is not, because your prompts pass through the model provider.
The server is a rented computer on the public internet. A Telegram bot chat is not end to
end encrypted. Running a model on your own hardware is a later session in this course and it
is a real answer. Knowing exactly which parts are private is worth more than believing all
of them are.

We also spend real minutes on the allowlist and on the one gateway setting you must never
turn on, because an agent with a terminal reachable from a chat app is a thing that can be
misused.

CHAPTERS

00:00 A bot on a phone, and the server it came from
04:00 The terminal, macOS and Windows 11 side by side
16:00 Install the four tools
24:00 Talk instead of type: offline speech to text
32:00 Build your second brain with PARA
42:00 Obsidian: links, backlinks, and why a vault compounds
50:00 VS Code: the same folder underneath
56:00 Choosing a model: filter on tool support, not price
1:06:00 Run the agent inside your notes
1:16:00 Write your first skill and run it with your voice
1:32:00 Rent a server and get inside it over SSH
1:38:00 Install the agent on the server
1:46:00 Telegram, and why Telegram rather than Signal or WhatsApp
1:54:00 Make it survive a reboot
1:58:00 Recap, what is private, and homework

TOOL LINKS

Handy, offline speech to text: https://handy.computer
Obsidian: https://obsidian.md
VS Code: https://code.visualstudio.com
Hermes agent: https://hermes-agent.nousresearch.com
OpenRouter: https://openrouter.ai
DigitalOcean: https://www.digitalocean.com
Telegram: https://telegram.org
PARA method: https://fortelabs.com/blog/para

ABOUT

AI Power Users is a hands on live course from Argo. Every session ends with something you
built, and every material is free and open on GitHub.

Hosted by Konrad Gnat, founder of MyArgoQuest.com, a private AI journaling app, and host of
the Argo Podcast.

Argo: https://myargoquest.com
Course materials on GitHub: https://github.com/kon-rad/ai-power-users-course

Subscribe to catch every module live. Questions go in chat, we build together.

#AI #AIagents #Obsidian #SecondBrain #SelfHosting #Telegram #OpenSource #Windows11 #macOS #LearnAI #Argo
```

---

## Metadata fields

| Field | Value |
|---|---|
| Category | Science and Technology |
| Visibility | Public, scheduled live |
| Made for kids | No |
| Language | English |
| Playlist | AI Power Users |
| Latency | Normal, so chat questions can be answered without a long lag |

**Tags:**

```
AI agent, AI second brain, Obsidian, PARA method, Hermes agent, OpenRouter, self hosted AI,
AI on a VPS, Telegram bot, AI agent Telegram, speech to text, offline speech to text, Handy,
VS Code, DigitalOcean, SSH, Windows 11 terminal, macOS terminal, no code AI, AI course,
personal AI assistant, Argo, AI Power Users
```

---

## Thumbnail

**Text on the thumbnail, three lines maximum:**

```
MY AI AGENT
TEXTS ME BACK
2 HRS · FROM ZERO
```

**The image:** a phone, held up, showing the chat, with the dark room and the glowing
laptop behind it. Not a terminal screenshot. Crop from the square Luma cover so the
thumbnail and the event cover are visibly the same session.

**Check it at 120 pixels wide.** That is how most people will see it.

---

## Stream setup

| Item | Setting |
|---|---|
| Holding screen | `module-01-v2-youtube-loading-1920.jpg`, in OBS, running from about 10 minutes before |
| Music bed | Added in OBS, not baked into the image, so the same bed is reused across modules |
| Scenes needed | Holding screen · Full screen terminal · Terminal plus webcam · **Phone screen share** · Browser full screen |
| The phone scene | **Set this up and test it before going live.** It is used at 0:00, 1:46 and 1:54, and it is the payoff shot three times |
| Two machines | A Mac and a Windows 11 machine, both capturable, since the module teaches both. If only one is available, run the other in a VM as its own scene |
| Font size | Terminal at a size that is readable at 480p. Most live viewers are on phones |

---

## Placeholders to fill before going live

- `[link to student-guide.md]`, the public GitHub link. **404 as of 2026-08-18** because this
  module has not been pushed. Push first, then check it loads
- `[Luma link]`, the registration URL for this event
- The chapter times, if the run of show changed. **YouTube chapters must start at 00:00 and
  be in ascending order or none of them appear at all**
- Confirm `https://myargoquest.com/courses/ai-power-users` still returns 200. It did on
  2026-08-18

---

## After the stream

1. Trim the pre roll so the recording starts on the first spoken word, then re-check every
   chapter timestamp. Trimming shifts all of them.
2. Pin a comment with the student guide link and the two costs.
3. Add the video to the AI Power Users playlist.
4. Put the real recorded chapter times back into this file, so the next module's estimate
   starts from a real number rather than the plan.

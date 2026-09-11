# Luma Event: Module 1 V2

> House rules: short, concrete, no em dashes, no emojis, no model names, links at the end,
> and the host and sponsor footer pasted byte for byte.

## Event title

**The one we are using:**

AI Power Users · Module 1 V2: Setting Up Your Personal AI Agent and Second Brain (Live)

**Two alternates:**

2. AI Power Users · Module 1 V2: Your Own AI Agent, On Your Own Server, In Your Pocket (Live)
3. AI Power Users · Module 1 V2: Build the Agent, Then Put It Somewhere It Never Sleeps (Live)

## Short blurb (for previews)

Install a working AI agent inside your own notes, write its first skill, then put it on a
server you rent and text it from your phone. Two hours, live. macOS and Windows 11. About
5 USD of model credit and 6 USD a month for the server.

## Full description

This is Module 1 V2 of AI Power Users, a hands on live course from Argo. It runs on macOS
and Windows 11, both taught side by side, and it is the entry point. Nothing before it is
required.

This is the rebuilt version of the first session. The original ended with an agent that
worked when your laptop was open. This one does not stop when you close it.

Two halves.

**First, the agent moves into your notes.**

You install four free tools: local speech to text so you can talk instead of type, a folder
of plain text files organised with PARA, an editor to look underneath, and an open source
agent that runs inside that folder and can read and write the files in it.

Then you connect it to a model, and this is the part most people get wrong. You do not pick
the smartest model. You filter on **tool support first**, because an agent that cannot call
tools will chat happily and never touch a single file. It looks broken. It is not. Then
context length, then price, then speed, in that order. We do this live on the model list, on
camera, and we work out on screen what one session actually costs.

**Then you write its first skill and run it with your voice.**

**/standup** reads your active projects, tells you where they stand, asks you three
questions one at a time, and writes a dated note back into your vault with a link to
tomorrow's. You answer all three by speaking. Two minutes a morning, and the vault fills
itself.

**Then we put it somewhere it never sleeps.**

1. Make an SSH key, and see the two files: the one you paste into a web form, and the one
   that never leaves your machine.
2. Rent a Linux server for six dollars a month and log into it from your own terminal.
3. Install the same agent on it, and copy the skill you wrote an hour earlier up to it as a
   plain text file.
4. Create a Telegram bot, allowlist exactly one person, yourself, and connect the agent to
   it.
5. Message your agent from your phone, on screen, live.
6. Reboot the server, wait forty seconds, and message it again without touching anything.

One agent session where the agent reads a few notes and writes one is roughly **half a
cent**. Five dollars of credit is somewhere near a thousand of them.

**By the end you will have:**

- A PARA vault on your own disk, open in two applications at once, that no company can take
  away
- Speech to text that runs offline and types into any application on your machine
- A `/standup` skill you wrote yourself, in a file, that you will actually run tomorrow
  morning
- A Linux server you own the login to, with your agent installed on it
- **A Telegram bot on your phone that is your agent, and that survives a reboot**

**Who it is for:** anyone starting from zero. No coding experience required, no prior
module required, and no terminal experience assumed. The first ten minutes are the terminal
from scratch, on both operating systems.

**The honest part, three things:**

- **You are not building a private AI today.** The speech to text is private and your notes
  are private. The model is not, because your prompts pass through the model provider. The
  server is a rented computer on the public internet. A Telegram bot chat is not end to end
  encrypted. Running a model on your own hardware is a later session and it is a real
  answer. Knowing exactly which parts are private is worth more than believing all of them
  are.
- **The server does not get your notes.** It gets the same folder structure and the skill
  you wrote. Syncing your actual vault to it is a real problem and it is homework, not a
  live segment.
- **An agent with a terminal, reachable from a chat app, is a thing that can be misused.**
  We spend real minutes on the allowlist and on the one setting you must never turn on. This
  is not a disclaimer at the end. It is part of the build.

**What it costs:** about **5 USD of model credit**, which will last you months, and about
**6 USD a month for the server**, billed by the second, so a server you destroy at the end
of the session costs a few cents. The four tools are free. **Add the model credit before you
arrive, not during the session.**

**Format:** one live session, 120 minutes, no break. Live on YouTube. macOS and Windows 11.

## Links

Course page: https://myargoquest.com/courses/ai-power-users/module-1-v2

Setup and download instructions:
https://github.com/kon-rad/ai-power-users-course/blob/main/modules/module-01-v2-personal-agent-and-second-brain/student-guide.md

Everything you need to install beforehand is in the setup instructions. Start there.

> **Both URLs returned 404 when checked with curl on 2026-08-18.**
>
> They follow the patterns that do resolve. `https://myargoquest.com/courses/ai-power-users`
> and `/module-0`, `/module-1`, `/module-2` all return 200, so the course page just has to be
> built. The GitHub guide 404s because this module has not been pushed yet.
> `https://github.com/kon-rad/ai-power-users-course/blob/main/modules/module-02-agent-mastery-and-vibe-coding/student-guide.md`
> returns 200, which confirms the shape is right.
>
> **Do not publish this description until both URLs load.**

## Footer (fixed copy, paste byte for byte)

Paste the host and sponsor block verbatim at the end of the description. It contains zero
width characters. Do not retype it. Copy the file.

> **The footer file was not found on this machine.** The style guide and the module skill
> both point at `.claude/skills/argo-events/assets/luma-footer.md`, and neither that file
> nor the `argo-events` skill exists under `~/.claude/skills/` as of 2026-08-18. Recover it
> from whichever machine last published a Luma page, or copy the footer out of the live
> Module 2 event, and put it back at that path before this description ships. **Do not
> retype it from memory.** The zero width characters will not survive.

## Suggested Luma settings

- Cost: Free to attend. About 5 USD of model credit and 6 USD a month for a server required
- Capacity: uncapped, livestream
- Location: Online, YouTube Live, link sent to registrants
- Host: Argo (Konrad Gnat)
- Duration: 140 minutes, a 120 minute session plus buffer
- Tags: AI, Agents, Second Brain, Obsidian, Telegram, Self Hosting, Windows, macOS,
  No-Code, Beginner-Friendly

## Notes for the host

- **Lead the promo image with the phone, not the terminal.** The deliverable is a bot in a
  chat app. That is what sells the session, and it is what nobody else's beginner AI class
  ends on.
- **The two costs have to be visible before someone registers**, not discovered in the
  session. Five dollars of model credit and six dollars a month is not much, but finding out
  about it at minute four is what generates refund messages.
- **Do not name a model anywhere in this description.** Whatever is named will be wrong by
  the stream date, and the module's whole point is that you look it up and filter on tool
  support instead of memorising a name.
- **Both operating systems are stated in the first line on purpose.** Module 0 was Windows
  only and Module 3 was macOS only, and people will ask.
- The word "V2" stays in the title. It tells returning students that this replaces the
  session they already sat through, and it tells new students nothing confusing.

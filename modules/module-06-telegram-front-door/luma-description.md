# Luma Event: Module 6

> House rules: short, concrete, no em dashes, no emojis, links at the end, and the host
> and sponsor footer pasted byte for byte.

## Event title

**The one we are using:**

AI Power Users · Module 6: The Telegram Front Door (Live, Free)

**Two alternates:**

2. AI Power Users · Module 6: Talk to Your Agent From Your Phone (Live, Free)
3. AI Power Users · Module 6: Your Server Asks Permission (Live, Free)

## Short blurb (for previews)

Your agent becomes reachable from your phone, and asks your permission before it acts. One
session, 60 minutes. Free, and it needs the server from Module 5.

## Full description

This is Module 6 of AI Power Users, a free hands on live course from Argo.

Module 5 put your agent on a server that never closes. It is still only reachable by SSH.

Two halves.

**First, the bot exists.** A token from BotFather, your numeric user ID, two lines in a
config file, one restart. Then the part that matters: who is allowed to talk to it.

**Then we make it safe and useful.**

1. Trigger an approval prompt and answer it from your phone.
2. Deny one. Then let one time out and watch it deny itself.
3. Split admin commands from user commands.
4. Pair someone else in, then revoke them.
5. Send a scheduled summary to your phone that stays quiet on quiet days.
6. Make a script message you when it breaks, with no model involved.

Adding a chat interface to your server opens zero inbound ports. We check the firewall on
camera.

**By the end you will have:**

- Your agent answering on your phone
- An allowlist that refuses everyone else
- An approval prompt you approved and one you denied
- A morning summary that stays silent when nothing changed
- **A script that texts you the moment it fails**

**Who it is for:** anyone who finished Module 5 and has a server running their agent.

**The honest part:** the lock on this door is a number identifying your Telegram account. If
your token leaks, whoever holds it controls your bot until you revoke it. Good setup, not a
vault.

**Format:** one live session, 60 minutes, no break. Live on YouTube.

## Links

Course page: https://myargoquest.com/courses/ai-power-users/module-6

Setup and download instructions:
https://github.com/kon-rad/ai-power-users-course/blob/main/modules/module-06-telegram-front-door/student-guide.md

Everything you need to install beforehand is in the setup instructions. Start there.

> **Both URLs returned 404 when checked on 2026-08-23.** They follow the pattern that
> resolves for Module 2, which was confirmed working the same day. Do not publish the
> description until each one loads.

## Footer (fixed copy, paste byte for byte)

Paste the host and sponsor block from
`.claude/skills/argo-events/assets/luma-footer.md` verbatim at the end of the description.
It contains zero width characters. Do not retype it. Copy the file.

> **That file was not present on this machine on 2026-08-23.** Take the footer from the
> published Module 2 or Module 3 Luma event instead, by copy and paste, not retyping.

## Suggested Luma settings

- Cost: Free to attend. Requires the Module 5 server, about 6 USD a month
- Capacity: uncapped, livestream
- Location: Online, YouTube Live, link sent to registrants
- Host: Argo (Konrad Gnat)
- Duration: 75 minutes, a 60 minute session plus buffer
- Tags: AI, Agents, Telegram, Automation, Self-Hosting, Security, Beginner-Friendly

## Notes for the host

- Lead the promo image with the phone, showing an approval prompt mid conversation. The
  phone is the whole pitch.
- This module gates hard on Module 5. Say so in the blurb, not only in the description, or
  people will register without a server and cannot follow along.
- Do not name a model in the description.
- Crop or blur the token when BotFather returns it, and revoke that token after the stream
  on camera so the revoke flow gets demonstrated once.

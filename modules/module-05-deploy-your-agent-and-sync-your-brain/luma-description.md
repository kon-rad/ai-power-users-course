# Luma Event: Module 5

> House rules: short, concrete, no em dashes, no emojis, links at the end, and the host
> and sponsor footer pasted byte for byte.

## Event title

**The one we are using:**

AI Power Users · Module 5: Deploy Your Agent and Sync Your Brain (Live)

**Two alternates:**

2. AI Power Users · Module 5: The Agent That Runs When Your Laptop Is Closed (Live)
3. AI Power Users · Module 5: Put Your Second Brain on a Server (Live)

## Short blurb (for previews)

Your agent moves off your laptop onto a server that does not close, holding the same notes
and the same skills. One session, 60 minutes. About 6 USD a month for the server, and it
bills until you destroy it.

## Full description

This is Module 5 of AI Power Users, a free hands on live course from Argo. Local commands
are macOS, the server is Ubuntu.

Module 3 put your site on a domain you own. Today your agent moves off the laptop.

Two halves.

**First, your second brain becomes a git repository.** The ignore list, the first commit,
and your own skills moved in so they travel with it.

**Then the server.**

1. Create a Droplet and harden it without locking yourself out.
2. Run your agent in Docker, with no open ports.
3. Give it a credential scoped to one repository.
4. Clone your brain onto it and prove the skills load.
5. Schedule a push job that stays silent unless it fails.
6. Write a note on the server. Read it in Obsidian on your laptop.

Mine is 16 GB on disk and 4.7 MB of markdown.

**By the end you will have:**

- A server running your agent while your laptop is closed
- Your vault in a private repository
- Your skills loading from that repository on both machines
- A sync job running every 15 minutes
- **A note written by the server, open in Obsidian**

**Who it is for:** anyone with a working agent who wants it running when they are not.

**The honest part:** the two agents share a folder, not a mind. Memory and sessions stay on
the machine that made them. Anything that has to cross gets written down.

**What it costs:** about 6 USD a month, and it bills until you destroy it. We destroy one
on camera.

**Format:** one live session, 60 minutes, no break. Live on YouTube.

## Links

Course page: https://myargoquest.com/courses/ai-power-users/module-5

Setup and download instructions:
https://github.com/kon-rad/ai-power-users-course/blob/main/modules/module-05-deploy-your-agent-and-sync-your-brain/student-guide.md

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

- Cost: Free to attend, about 6 USD a month of server cost required
- Capacity: uncapped, livestream
- Location: Online, YouTube Live, link sent to registrants
- Host: Argo (Konrad Gnat)
- Duration: 75 minutes, a 60 minute session plus buffer
- Tags: AI, Agents, DevOps, Self-Hosting, Git, Automation, Beginner-Friendly

## Notes for the host

- Lead the promo image with Obsidian on the laptop and a server terminal beside it, the
  same note visible in both. The deliverable sells the session.
- The recurring cost has to be visible before someone registers. It is a harder sell than
  Module 3's one off 2 USD, so the destroy step is part of the pitch, not an afterthought.
- Do not name a model in the description.
- DigitalOcean's new account credit was listed by third parties as 200 USD for 60 days on
  2026-08-23 but was not confirmed on a DigitalOcean page. Do not put a credit figure in
  the description unless it is read off the signup page first.
- The student guide also documents an optional, un-timed capability: pointing the
  laptop agent's terminal backend at the droplet over SSH so it runs commands there
  instead of locally. It is homework, not a segment, and it is not in the description.
  Adding it would misstate what the 60 minutes actually teaches live. Demo it only if
  the close finishes early.

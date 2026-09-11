# Luma Event: Module 7

> House rules: short, concrete, no em dashes, no emojis, links at the end, and the host
> and sponsor footer pasted byte for byte.

## Event title

**The one we are using:**

AI Power Users · Module 7: Your Own Cloud Drive (Live)

**Two alternates:**

2. AI Power Users · Module 7: The 19 Gigabytes Git Will Not Carry (Live)
3. AI Power Users · Module 7: One Folder, Two Machines, Every File (Live)

## Short blurb (for previews)

Your whole vault, not just the notes, reachable from both machines and from your agent. One
session, 60 minutes. Storage is about 2 cents a month for 13 GB, and it needs the server from
Module 5.

## Full description

This is Module 7 of AI Power Users, a free hands on live course from Argo.

Module 5 put your notes in git. Git carries five megabytes; the vault is nineteen gigabytes.

Two halves.

**First, the split.** Three categories, not two: what needs history, what needs durability,
and what you can rebuild with one command. One filter file decides which is which.

**Then the loop.**

1. Create a private bucket and two keys, one read only.
2. Run the first two way sync, dry run first.
3. Break it on purpose and watch it refuse.
4. Let the server fetch a file it does not have.
5. Stop old versions billing you forever.
6. Schedule it, silent until it fails.

Nineteen gigabytes of vault, thirteen of it video, and the storage bill is about two cents a
month.

**By the end you will have:**

- A private bucket holding every large file in your vault
- A filter file that is the complement of your ignore list
- A sync that refuses to run when one side is missing
- **A server that fetches a video without storing it**
- A scheduled job that stays quiet until something breaks

**Who it is for:** anyone who finished Module 5 and has more than notes in their vault.

**The honest part:** two way sync can delete on both sides at once. The tool's own manual
says data loss can result. Half the session is the guards.

**What it costs:** the first ten gigabytes of storage are free, then about seven dollars per
terabyte a month. Add a card before you arrive.

**Format:** one live session, 60 minutes, no break. Live on YouTube.

## Links

Course page: https://myargoquest.com/courses/ai-power-users/module-7

Setup and download instructions:
https://github.com/kon-rad/ai-power-users-course/blob/main/modules/module-07-your-own-cloud-drive/student-guide.md

Everything you need to install beforehand is in the setup instructions. Start there.

> **Both URLs returned 404 when checked on 2026-08-26.** They follow the pattern that
> resolves for Module 2, which was confirmed returning 200 the same day. Do not publish the
> description until each one loads.

## Footer (fixed copy, paste byte for byte)

Paste the host and sponsor block from
`.claude/skills/argo-events/assets/luma-footer.md` verbatim at the end of the description.
It contains zero width characters. Do not retype it. Copy the file.

> **That file was not present on this machine on 2026-08-26.** Take the footer from the
> published Module 5 or Module 6 Luma event instead, by copy and paste, not retyping.

## Suggested Luma settings

- Cost: Free to attend. Backblaze storage is about 2 cents a month for 13 GB, and it needs
  the Module 5 server at about 6 USD a month
- Capacity: uncapped, livestream
- Location: Online, YouTube Live, link sent to registrants
- Host: Argo (Konrad Gnat)
- Duration: 75 minutes, a 60 minute session plus buffer
- Tags: AI, Agents, Backup, Cloud Storage, Self-Hosting, Automation, Beginner-Friendly

## Notes for the host

- Lead the promo image with the three category split, not with a cloud icon. The idea people
  have not heard before is that `node_modules` belongs in none of the systems they pay for.
- This module gates hard on Module 5. Say so in the blurb, not only in the description.
- Do not name a model in the description.
- Crop the Backblaze application key when it appears. B2 shows it exactly once.
- The two cents figure is true and sounds like marketing. Pair it with the 2 TB comparison in
  the session, where B2 is more expensive than Dropbox Plus, so the claim reads as arithmetic
  rather than a pitch.

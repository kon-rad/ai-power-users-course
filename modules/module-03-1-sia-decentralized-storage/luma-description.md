# Luma Event: Module 3.1

> House rules: short, concrete, no em dashes, no emojis, links at the end, and the host
> and sponsor footer pasted byte for byte.

## Event title

**The one we are using:**

AI Power Users · Module 3.1: Sia and Decentralized Cloud Storage (Live, Free)

**Two alternates:**

2. AI Power Users · Module 3.1: Back Up Your Vault to a Network No One Owns (Live, Free)
3. AI Power Users · Module 3.1: Give Your Agent a Decentralized Backup (Live, Free)

## Short blurb (for previews)

Module 3 gave your agent a camera and a domain. Neither one is backed up anywhere. This
session puts the vault on Sia, storage spread across independent hosts instead of one
company's data center, and proves it by actually restoring from it live. One session, 60
minutes, 0 USD.

## Full description

This is Module 3.1 of AI Power Users, a free hands on live course from Argo.

Module 3 gave your agent a camera and a domain. Today it gets a backup that survives the
laptop being gone.

Two halves.

**First, what Sia actually is.** Your file gets encrypted, split into shards, and spread
across independent hosts, so no single one ever holds a complete copy. Sia Storage's own
indexer coordinates the contracts, which is why sign up asks for no crypto knowledge.

**Then the build.**

1. Measure the vault against the free tier.
2. Create a Sia Storage account, upload one file by hand.
3. Install the SDK, read its own README first.
4. Get your agent to write `/vault-backup`, backup and restore.
5. Run the backup, get back a real object id.
6. Delete the vault, restore it, diff the result.

50 GB free, no card, no crypto wallet.

**By the end you will have:**

- A Sia Storage account holding a real backup
- **A `/vault-backup` skill your agent wrote**
- A tested restore, diffed against the original
- A measured vault size against the free tier
- The habit of reading a package's own docs first

**Who it is for:** anyone who finished Module 3 and has never tested whether their vault
backup actually restores.

**The honest part:** the SDK behind `/vault-backup` is version 0.0.14, pre 1.0, and its
interface can change without notice, which is why you read its README live instead of
copying code from a description.

**Format:** one live session, 60 minutes, no break. Live on YouTube. macOS.

## Links

Course page: https://myargoquest.com/courses/ai-power-users/module-3-1

Setup and download instructions:
https://github.com/kon-rad/ai-power-users-course/blob/main/modules/module-03-1-sia-decentralized-storage/student-guide.md

> **Both URLs checked 2026-09-04: the course page returns 404 and the GitHub link returns
> 404.** They follow the pattern that resolves for Module 3.2, so they are the right shape,
> but do not publish this description until each one loads.

## Footer (fixed copy, paste byte for byte)

Paste the host and sponsor block from
`.claude/skills/argo-events/assets/luma-footer.md` verbatim at the end of the description.
It contains zero width characters. Do not retype it. Copy the file.

## Suggested Luma settings

- Cost: Free to attend, 0 USD required
- Capacity: uncapped, livestream
- Location: Online, YouTube Live, link sent to registrants
- Host: Argo (Konrad Gnat)
- Duration: 75 minutes, a 60 minute session plus buffer
- Tags: AI, Agents, Decentralized Storage, Sia, Backups, Beginner Friendly

## Notes for the host

- Lead the promo image with app.sia.storage mid upload, or the clean `diff -rq` output from
  the restore, not a generic cloud graphic. The restore is the deliverable.
- State plainly that this uses Sia Storage's hosted account, not a self hosted node, and say
  what that trades away before anyone asks.
- Do not quote a specific host count or shard count on stream. Public docs state 3x
  redundancy without a fixed number as of 2026-09-04.
- Do not read the `/vault-backup` connect step from memory on stream. Open the installed
  package's README live and read from that.

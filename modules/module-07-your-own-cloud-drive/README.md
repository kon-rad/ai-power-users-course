# Module 7: Your Own Cloud Drive

**Module 7 of AI Power Users · 60 minutes · live on YouTube · laptop plus the Ubuntu server from Module 5**
**From Argo, myargoquest.com**

Module 5 put the vault in git and moved it to a server. Git carries 5 MB of markdown. The
vault is 19 GB. This session gives the other 18.99 GB somewhere to live, and gives both
agents a way to reach it.

Backblaze B2 costs 6.95 USD per TB per 30 days and the first 10 GB is free, so a 13 GB media
library bills at about 2 cents a month. Checked 2026-08-26.

## What you build

```
THE SPLIT
    notes, code, skills  ──▶  GitHub, private     ──  merges, has history
    video, audio, PDFs   ──▶  Backblaze B2        ──  durable, no merge
    node_modules, caches ──▶  nowhere             ──  rebuildable

    .gitignore  and  .rclone-filter  are complements
    every file carried by exactly one of them, none by both

THE LOOP
    laptop  ◀── rclone bisync ──▶  B2   ──  two way, guarded
    server  ◀── rclone copy   ───  B2   ──  read only, on demand
    sync-brain.sh  ──  cron, silent on success, loud on failure
```

## Learning objectives

By the end of this module you can:

1. Sort a folder into three categories, not two, and say which one is filling your disk
2. Say why git is the wrong tool for a 1.8 GB video and object storage is the wrong tool for
   a note
3. Create a B2 bucket and an application key scoped to one bucket with one capability set
4. Write a filter file that is the complement of your `.gitignore`, and say why that matters
5. Run `rclone bisync` safely: dry run first, resync once, then never again
6. Explain what `--check-access` protects against, and demonstrate the failure it catches
7. Give a server read only access to the archive, and say what a compromise can and cannot do
8. Set a lifecycle rule on old versions, and say what the bill looks like without one

## The twelve steps

| # | Step | Output |
|---|---|---|
| 1 | Measure three categories | Your own markdown, media and `node_modules` numbers |
| 2 | Audit the repo | Proof git is still carrying text only |
| 3 | Create the bucket | Private, object lock off |
| 4 | Create two keys | Laptop read write, server read only, both scoped to one bucket |
| 5 | Configure rclone | A working `brain:` remote |
| 6 | **Write the filter file** | **The one place the split is defined** |
| 7 | First sync | `--dry-run`, then `--resync` once |
| 8 | **Set up the access check** | **`RCLONE_TEST` on both sides, and a sync that refuses without it** |
| 9 | Set up the server | rclone with the read only key |
| 10 | Give the agent a fetch skill | `fetch-media`, travelling through git |
| 11 | Stop paying for old versions | A lifecycle rule and a spending cap |
| 12 | Automate it | `sync-brain.sh` on a 30 minute schedule |

## The ideas that carry the module

**There are three categories, not two.** Everyone splits a folder into small and large, then
wonders why the sync is still enormous. The third category is rebuildable, and a thousand
copies of `node_modules` belong in none of the systems you are paying for.

**Git is for things that merge. Object storage is for things that do not.** Git merges text
and always has. Nothing merges an MP4. Picking the tool by file size is the wrong instinct;
pick it by whether a conflict has a sensible resolution.

**Never let two sync systems own the same file.** One line in the filter file excludes
markdown from B2, because git already owns it. Without that line a pull and a sync race each
other, and the loser is overwritten with no error.

**A backup that has never been restored from is not a backup.** The homework is a restore for
exactly this reason.

**`--max-delete` is on by default and it is not enough.** It aborts above 50 percent. Losing
40 percent of a vault proceeds silently. `--check-access` is the guard that fails on the
actual failure mode, an unmounted or mistyped path.

**A script must never answer `--resync` on its own.** Bisync asks for it when it has lost
track of what changed. That is the system asking for a human, and resync means one side wins.

**Scope the key, not the trust.** The server needs to read the archive and has never needed
to delete it. A read only key makes that structural rather than a promise.

## Files in this module

- [`syllabus.md`](./syllabus.md), the full plan: every segment, every brief, host guidance
- [`agenda.md`](./agenda.md), the 60 minute run of show and the contingency
- [`student-guide.md`](./student-guide.md), **follow along here**: every command and brief
- [`quiz.md`](./quiz.md), 5 multiple choice plus 4 open ended, peer evaluated
- [`luma-description.md`](./luma-description.md), event copy

## Before the session

**You need Module 5's setup.** A private vault repository, a hardened Droplet, and Hermes in
Docker with the vault at `/opt/brain`. If yours is not running, the
[Module 5 student guide](../module-05-deploy-your-agent-and-sync-your-brain/student-guide.md)
rebuilds it. Module 6 is optional and only affects the last segment.

| Account | Why | Cost |
|---|---|---|
| [Backblaze B2](https://www.backblaze.com/cloud-storage) | Where the large files live | First 10 GB free, then 6.95 USD per TB per 30 days |

Install rclone before the session and confirm the version:

```bash
brew install rclone
rclone version
```

**v1.66 or newer.** Below that, bisync is not safe to use.

## Homework

1. Restore one file from B2 to a scratch directory and open it.
2. Delete a file locally, sync, then recover it from a hidden version.
3. Rename `RCLONE_TEST` on one side and confirm the sync refuses. Put it back.
4. Run `rclone size --b2-versions` in a week and compare it to the live size.
5. Find your largest committed file. If it is over 10 MB, decide whether to rewrite history
   with `git filter-repo` and write down why.
6. Set a Backblaze cap and alert on the account.
7. Add one directory to `.rclone-filter` and run the `--resync` the change requires.

## The honest caveat

Two way sync is the sharpest tool in this course. rclone's own manual calls bisync an
advanced command and says data loss can result if you have not read the whole thing. Every
guard set today, the dry run, the access check, the max delete, the refusal to auto resync,
exists because the failure mode is not an error message. It is a folder that is quietly
emptier than it was. Run the dry run every time you change anything, and restore a file
before you trust the system with the only copy of something.

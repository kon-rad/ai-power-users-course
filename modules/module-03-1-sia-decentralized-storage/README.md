# Module 3.1: Sia and Decentralized Cloud Storage

**Module 3.1 of AI Power Users · 60 minutes · live on YouTube · run on macOS**
**From Argo, myargoquest.com**

Module 3 gave your agent a camera and a domain. Neither one is backed up anywhere.
This session puts the vault on Sia, a storage network built from independent hosts instead
of one company's data center, and proves the backup works by actually restoring from it.

## What you build

```
THE ACCOUNT
    sia.storage        50 GB free, no card, no crypto wallet
    app.sia.storage     drag a file in, watch it distribute

THE MECHANISM
    your file  ──▶ encrypted, split into shards ──▶ spread across independent hosts
    no single host ever holds a complete, readable copy

THE SKILL
    /vault-backup   ──▶  archives the vault, uploads it through the SDK, prints an object id
    /vault-backup restore <id>  ──▶  downloads it, unpacks it into restore-test/
```

## Learning objectives

By the end of this module you can:

1. Explain how Sia splits and distributes a file so no single host holds a full readable
   copy
2. Create a free Sia Storage account and check your own vault's size against its 50 GB
   ceiling
3. Get your agent to read a package's own README before writing a line of code against it,
   instead of working from a memorized snippet
4. Get your agent to write a skill that archives the vault and uploads it to Sia
5. Restore a backup from Sia and confirm the restored vault matches the original, file for
   file
6. State plainly what "no crypto knowledge required" costs you: who is coordinating the
   storage contracts on your behalf, and what you are trusting them to do
7. Say why redundancy across hosts is not the same guarantee as a backup you have actually
   restored from

## The skill your agent writes

| Skill | Cadence | What it does | Why it matters |
|---|---|---|---|
| **`/vault-backup`** | On demand today, meant to be scheduled | Archives the vault, uploads it to Sia through the SDK, prints the object id back. Given an object id, downloads it and unpacks it into `restore-test/` | A backup with no restore path is a hope, not a backup. This skill builds both halves in the same session, not the upload half now and the restore half someday |

## The 9 steps

| # | Step | Output |
|---|---|---|
| 1 | Measure the vault | A real size in GB, checked against the free tier |
| 2 | Learn the mechanism | Shards, hosts, no single point of failure, in plain terms |
| 3 | Create a Sia Storage account | 50 GB free, live at app.sia.storage |
| 4 | Upload one file by hand | Proof the account and the network actually work |
| 5 | Install the SDK | `@siafoundation/sia-storage` in this project |
| 6 | **Read the SDK's own README** | **The connect, upload and download calls, current today, not memorized** |
| 7 | **Build `/vault-backup`** | **A skill that archives the vault and uploads it** |
| 8 | Run the backup live | An object id, printed back from Sia |
| 9 | **Disaster restore** | **The vault rebuilt from Sia, diffed against the original** |

## The ideas that carry the module

**No single host holds your file.** Sia encrypts and splits the vault into shards before
any of it leaves your machine, then spreads those shards across independent hosts, so one
host, or one company, never sees a complete, readable copy.

**Convenience has a coordinator.** Signing up with no crypto knowledge works because Sia
Storage's own indexer holds the storage contracts on your behalf. That means you are
trusting that indexer's uptime and honesty, on top of trusting the hosts underneath it.

**Redundancy is not backup.** The network's default erasure coding protects the vault
against a host disappearing. It does nothing for a mistake made on your own machine. Step 9
is what actually proves a backup exists, not the upload in Step 8.

**A version 0.0.14 package is a moving target.** The SDK is pre 1.0. Reading its README
fresh instead of running on a memorized snippet is the habit that matters, and it will
outlast this specific package.

**Free has a ceiling.** 50 GB comfortably holds a vault of markdown and skills. A vault
carrying every hero clip variant generated in Module 3 sits a lot closer to it, which is why
Step 1 is a real measurement, not an assumption.

## Files in this module

- [`syllabus.md`](./syllabus.md), the full plan: every segment, every brief, host guidance
- [`agenda.md`](./agenda.md), the 60 minute run of show and the contingency
- [`student-guide.md`](./student-guide.md), **follow along here**: every step and brief
- [`quiz.md`](./quiz.md), 5 multiple choice plus 4 open ended, peer evaluated
- [`luma-description.md`](./luma-description.md), event copy

## Before the session

**You need Module 3 complete.** A project with an agent already wired in and a working
`.env` pattern for keys. If you missed it, the
[Module 3 student guide](../module-03-media-models-and-your-own-domain/student-guide.md)
gets you current.

| Account | Why | Cost |
|---|---|---|
| [Sia Storage](https://sia.storage) | Where the backup actually lives | 50 GB free, no card, no crypto wallet |

Confirm Node is already on this machine from Module 2:

```bash
node --version
```

**Bring:** your vault as it stands after Module 3, including any media it generated.

## Homework

1. Schedule `/vault-backup` so it runs nightly with no one watching it, the same way
   Module 7 later schedules a media sync.
2. Reread the SDK's README before the next stream and note anything that changed since
   version 0.0.14.
3. Restore into a second folder from an older object id if you kept one, and confirm you
   can choose which backup comes back, not just the latest.
4. Measure the vault again in a week and say how much closer to 50 GB it got.

## The honest caveat

The zero crypto sign up is real, but it puts Sia Storage's own indexer between you and the
network, the same trust you place in any hosted service. What is different underneath is
that the actual file storage that indexer coordinates is spread across hosts none of which
controls the whole thing alone. And the SDK wired in today is version 0.0.14: expect
breaking changes, and expect to reread its README before you trust it again, not just this
once.

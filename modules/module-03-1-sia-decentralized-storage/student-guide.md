# Module 3.1: Student Guide

**Sia and Decentralized Cloud Storage**
Follow along here. Every command, every brief, every check.

By the end you will have a free Sia Storage account, one file already uploaded by hand as
proof the network works, a `/vault-backup` skill your agent wrote from the SDK's own
README, and a restore you have actually run and diffed against the original vault.

---

## Before the session

### You need Module 3 complete

```bash
node --version
```

If this fails, the
[Module 3 student guide](../module-03-media-models-and-your-own-domain/student-guide.md)
gets Node and the project set up first.

### Create nothing yet

Sign up happens live, together, in Part 2. Do not create the Sia Storage account ahead of
time. Everything else on this list is prep, not the session itself.

### Bring

- **Your vault as it stands after Module 3**, including any media it generated.

---

## Step 1: Measure the vault

```bash
du -sh ~/Documents/secondbrain
```

Write the number down. Sia Storage's free tier is 50 GB. A vault of markdown and skills
alone sits nowhere near that. A vault carrying every hero clip variant from Module 3 might
not.

> **The trap.** If the number is close to 50 GB, decide now whether media belongs in this
> backup, or whether it gets excluded the way Module 7 later excludes it from git. Do not
> find this out mid upload.

---

## Step 2: The mechanism, in plain terms

Sia's own description: your file gets encrypted and split into shards, jumbled up, and
spread across independent hosts around the world. No single host holds the whole thing.

| Question | Answer |
|---|---|
| What happens to the file before it leaves your machine | Encrypted and split into shards |
| Where do the shards go | Spread across many independent hosts, not one data center |
| What does one host see | One shard. Meaningless alone |
| What does the default redundancy protect against | A host disappearing, not a mistake you make locally |

The honest limit: this still needs a coordinator to match your shards with hosts and keep
the contracts alive. Sia Storage is the version of that coordinator that asks nothing of you
but an account.

---

## Step 3: Create a Sia Storage account

Go to [sia.storage](https://sia.storage). Create a free account. No card, no crypto wallet.

---

## Step 4: Upload one file by hand

Open [app.sia.storage](https://app.sia.storage). Drag in one real, small file. Watch it
upload.

Erasure coding a file into shards and placing them across independent hosts is real work,
not one copy to one server. Give it a moment before assuming something failed.

> **The trap.** This one file is the whole proof for this half of the session. Everything
> after this point automates what you just did by hand.

---

## Step 5: Install the SDK

```bash
npm install @siafoundation/sia-storage
```

---

## Step 6: Read the SDK's own README before writing anything

```bash
cat node_modules/@siafoundation/sia-storage/README.md
```

This package is version 0.0.14, pre 1.0. Its own README, installed on your machine right
now, is more current than any snippet written in advance, including this guide. Read it
before Step 7.

> **The trap.** If the README documents a connect step this guide does not mention, that is
> not an error. That is the correct behavior of a pre 1.0 package. Follow what is actually
> on your machine.

---

## Step 7: Build `/vault-backup`

Paste this to your agent, after you have actually read the README yourself:

```
Install the Sia Storage SDK in this project if it is not already installed:

npm install @siafoundation/sia-storage

Read node_modules/@siafoundation/sia-storage/README.md in full before writing anything.
Using only what that file actually documents, build a skill called /vault-backup that does
two things.

Backup:
1. Connect to my Sia Storage account using whatever credential the README says the SDK
   needs, read from .env, never hardcoded.
2. Archive ~/Documents/secondbrain into a file called vault.tar.gz, excluding node_modules,
   .git, and any dist or build folders.
3. Upload vault.tar.gz using the SDK's documented upload function.
4. Print the returned object id and append it, with today's date, to backup-log.md.
5. Delete the local vault.tar.gz once the upload succeeds.

Restore, given an object id:
1. Download that object using the SDK's documented download function.
2. Unpack it into a folder called restore-test, never over the real vault.

Do not invent a function that is not documented in the README. If something this skill
needs is missing from the README, say so instead of guessing.
```

---

## Step 8: Run the backup

```
/vault-backup
```

Confirm an object id comes back and lands in `backup-log.md`.

> **The trap.** If `npm install` failed silently earlier or the package name has moved,
> check [npmjs.com/package/@siafoundation/sia-storage](https://www.npmjs.com/package/@siafoundation/sia-storage)
> directly before assuming the upload itself is broken.

---

## Step 9: Disaster restore, the actual proof

Move the real vault aside. Do not delete it:

```bash
mv ~/Documents/secondbrain ~/Documents/secondbrain-original
```

Restore using the object id from `backup-log.md`:

```
/vault-backup restore <object-id>
```

Diff the result against the original:

```bash
diff -rq ~/Documents/secondbrain-original restore-test
```

A clean diff is the proof. Put your original folder back:

```bash
mv ~/Documents/secondbrain-original ~/Documents/secondbrain
```

> **The trap.** If the diff is not clean, that is today's actual finding. Say plainly what
> did not survive the round trip before moving on, do not paper over it.

---

## If something breaks

| Symptom | Cause and fix |
|---|---|
| `npm install @siafoundation/sia-storage` fails | Check the package still exists at that exact name on npmjs.com before assuming a network issue |
| The agent's skill calls a function that errors immediately | It likely invented a function not in the README. Reread the README together and correct the brief |
| The browser upload in Step 4 seems stuck | Erasure coding and distributing shards takes longer than a single centralized upload. Wait before assuming failure |
| `/vault-backup restore` returns an old or wrong object id | Check `backup-log.md` for the exact id you meant to restore, entries are appended, not overwritten |
| The diff in Step 9 is not clean | Read exactly which files differ. A missing exclude in the tar step is the most common cause |

---

## Homework

1. Schedule `/vault-backup` with cron or launchd so it runs with no one watching it.
2. Reread the SDK's README before the next stream, note what changed since 0.0.14.
3. Keep an older object id and restore from it specifically, confirming you can choose a
   backup, not only reach the latest one.
4. Measure the vault again in a week and say how much closer to 50 GB it got.

---

## What this cost

| Item | Amount |
|---|---|
| Sia Storage account and 50 GB of storage | 0 USD |
| `@siafoundation/sia-storage` | 0 USD, open source |
| This session's actual data transferred | A single small file in Step 4, plus one vault sized backup and restore, well inside the free tier for most vaults |

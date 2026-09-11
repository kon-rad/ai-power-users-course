# Module 3.1: Sia and Decentralized Cloud Storage

**Status:** syllabus draft v1, planning phase.
**Date:** 2026-09-04.
**Platform:** macOS. Nothing in this module is Apple Silicon specific, but the exact tar
and du commands below are macOS syntax.
**Prereq:** Module 3 complete.
**Companions:** `student-guide.md` · `agenda.md` · `quiz.md` · `luma-description.md`

---

## Format

**One session, 60 minutes, no break, live on YouTube.**

| Part | Time | Theme |
|---|---|---|
| **Open** | 0:00 to 0:03 | State the artifact |
| **Part 1** | 0:03 to 0:15 | What Sia actually is, and whether the vault fits its free tier |
| **Part 2** | 0:15 to 0:27 | Create the account, prove the network works by hand |
| **Part 3** | 0:27 to 0:35 | Install the SDK, read its own README before writing anything |
| **Part 4** | 0:35 to 0:50 | Build and run `/vault-backup` |
| **Part 5** | 0:50 to 0:58 | Disaster restore |
| **Close** | 0:58 to 1:00 | What is now permanent, the honest caveat |

**The spine:** every module so far has added something to the vault. This is the first one
that asks what happens if the laptop holding it is gone tomorrow, and answers it by actually
restoring from Sia on stream, not describing how a restore would work.

---

## Learning objectives

By the end, a student can:

1. Explain how Sia splits and distributes a file so no single host holds a full readable
   copy
2. Create a free Sia Storage account and check the vault's size against its 50 GB ceiling
3. Get an agent to read a package's own README before writing code against it
4. Get an agent to write a skill that archives the vault and uploads it to Sia
5. Restore a backup and confirm every file matches
6. State what "no crypto knowledge required" costs, and who is coordinating on your behalf
7. Say why redundancy is not the same guarantee as a tested restore

---

## 0:00 to 0:03 · Open

State the artifact: a vault backed up to a storage network no single host or company
controls alone, with a restore already proven on stream, not promised for later.

---

# PART 1: What Sia actually is (12 min)

## 0:03 to 0:08 · Measure the vault first

```bash
du -sh ~/Documents/secondbrain
```

Write the number down before anything else happens today. Sia Storage's free tier is 50 GB.
A vault of markdown and skills sits nowhere near that. A vault carrying every hero clip
variant from Module 3 might not.

### Watch for

If the number is close to 50 GB, decide now whether media belongs in this backup at all, or
whether it gets excluded the same way Module 7 later excludes it from git. Do not discover
this mid upload.

## 0:08 to 0:15 · The mechanism, in plain terms

Sia's own description: your file gets encrypted and split into shards, jumbled up, and
spread across independent hosts around the world. Nobody holds the whole thing.

| Question | Answer |
|---|---|
| What happens to the file before it leaves your machine | Encrypted and split into shards |
| Where do the shards go | Spread across many independent hosts, not one data center |
| What does one host see | One shard. Meaningless alone |
| What does the default redundancy protect against | A host disappearing, not a mistake you make locally |

State the honest limit now, before the sign up: this network needs a coordinator to match
your shards with hosts and keep the contracts alive. Sia Storage is the version of that
coordinator that asks nothing of you but an account.

### Watch for

Do not promise a specific host count or shard count on stream. Public documentation states
3x redundancy without a fixed number as of 2026-09-04. A specific number here would be a
guess, not a fact.

---

# PART 2: Create the account, prove it works (12 min)

## 0:15 to 0:20 · Sign up

Go to [sia.storage](https://sia.storage). Create a free account. No card, no crypto wallet.

## 0:20 to 0:27 · Upload one file by hand

Open [app.sia.storage](https://app.sia.storage). Drag in one real file, something small.
Watch it upload.

Erasure coding a file into shards and placing them across independent hosts is real work,
not a single copy to one server. Give it a moment before assuming something failed.

### Watch for

This is the whole proof for this part of the session: one file, uploaded, confirmed present
in the account. Everything after this point is automating what was just done by hand.

---

# PART 3: Install the SDK, read its own README (8 min)

## 0:27 to 0:31 · Install it

```bash
npm install @siafoundation/sia-storage
```

## 0:31 to 0:35 · Read the README before writing anything

```bash
cat node_modules/@siafoundation/sia-storage/README.md
```

This package is version 0.0.14, pre 1.0. Its own README, installed on this machine right
now, is more current than any snippet written in advance, including this syllabus. Read it
together before Part 4 starts.

### Watch for

If the README documents a connect step this syllabus does not mention, that is not an
error, that is the correct behavior of a pre 1.0 package. Follow the README on the machine.

---

# PART 4: Build and run `/vault-backup` (15 min)

## 0:35 to 0:45 · The brief

Paste this to your agent, after confirming the README has actually been read this session:

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

## 0:45 to 0:50 · Run it

```
/vault-backup
```

Confirm an object id comes back and lands in `backup-log.md`.

### Watch for

If the install fails or the package name has moved, check
[npmjs.com/package/@siafoundation/sia-storage](https://www.npmjs.com/package/@siafoundation/sia-storage)
directly before assuming the network connection is the problem.

---

# PART 5: Disaster restore (8 min)

## 0:50 to 0:58 · Prove the backup, not the upload

Move the real vault folder aside, do not delete it:

```bash
mv ~/Documents/secondbrain ~/Documents/secondbrain-original
```

Run the restore using the object id from `backup-log.md`:

```
/vault-backup restore <object-id>
```

Diff the result against the original:

```bash
diff -rq ~/Documents/secondbrain-original restore-test
```

A clean diff is the actual proof. Put the original folder back.

### Watch for

If the diff is not clean, that is the finding for today, not a mistake to hide before the
stream ends. State plainly what did not survive the round trip and why, live.

---

## 0:58 to 1:00 · Review and close

State what is now permanent: a Sia Storage account holding a real backup, `/vault-backup`
able to run it again, and a restore that has actually been tested once, on this machine, on
this stream.

State the honest caveat: the sign up asked for no crypto knowledge because Sia Storage's own
indexer is doing that coordination on your behalf. And the SDK behind all of this is version
0.0.14. Reread its README before trusting it again next time.

---

# Homework

1. Schedule `/vault-backup` with cron or launchd so it runs with no one watching it.
2. Reread the SDK's README before the next stream and note what changed since 0.0.14.
3. Keep an older object id and restore from it specifically, confirming you can choose a
   backup, not only reach the latest one.
4. Measure the vault again in a week and say how much closer to 50 GB it got.

# Peer quiz

Summary only, detail in `quiz.md`: what a single compromised host actually sees · what "no
crypto knowledge required" is trading away · the difference between redundancy and a tested
restore · why the brief in Part 4 tells the agent not to invent a function.

# Prep checklist

## Must verify before the stream

- [ ] `npm install @siafoundation/sia-storage` still resolves, and its README documents a
      connect, upload and download call consistent with the brief above. Checked 2026-09-04
      at version 0.0.14. Re-check the morning of, this package is pre 1.0
- [ ] sia.storage signup still requires no card and no crypto wallet, and still grants
      50 GB free. Checked 2026-09-04 against sia.tech and docs.sia.tech
- [ ] app.sia.storage still accepts a plain browser upload with no extra setup
- [ ] Whether the SDK's real connect step needs anything beyond an account credential, such
      as a separately generated key. Not confirmed from public documentation as of
      2026-09-04. Hand run the full flow before the stream and correct Part 3 if it needs
      an extra step
- [ ] Sia's paid tier price, in case a student asks past 50 GB. Not published in what was
      checked 2026-09-04. Say plainly it is not published yet rather than guessing a number

## Assets

- [ ] A Sia Storage account already created, with one file already uploaded, in case live
      signup or the network stalls
- [ ] A cached copy of `@siafoundation/sia-storage`'s README from the morning of the stream,
      in case the live install is slow or the package changes mid session
- [ ] A small test vault, not the real one, to rehearse the full backup and restore cycle
      before going live with it

# Open questions

1. **Does the SDK's real connect flow need anything beyond an account credential?** Public
   docs do not confirm this. Someone should hand run the whole flow before the stream and
   correct the Part 4 brief if a key generation step is missing from it.
2. **Should this module mention self hosted `indexd` as an alternative to Sia Storage's
   hosted indexer?** It removes the one coordinator this session accepts as a trade off, at
   the cost of the setup this session was built to avoid. Recommend one line in the honest
   caveat at most, not a segment, so it does not dilute the actual build.
3. **Is macOS `tar` with the exclude flags in the Part 4 brief sufficient, or should the
   backup read the vault's `.gitignore` the same way Module 7 later reads it for `rclone`?**
   Worth deciding before the brief above is treated as final.

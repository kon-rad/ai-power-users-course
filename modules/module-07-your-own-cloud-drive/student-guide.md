# Module 7: Student Guide

**Your Own Cloud Drive**
Follow along here. Every command, every brief, ready to copy.

By the end, the whole vault exists on both machines. Git carries the text, Backblaze B2
carries everything else, and a script keeps them in step.

**Part 1 runs on your laptop. Part 2 runs on the Ubuntu server from Module 5.**

Replace `<BUCKET>` everywhere below with your own bucket name.

---

## Before the session

### Check Module 5 still holds

```bash
cd ~/SecondBrain
git status
git remote -v
```

You need a clean tree and a private GitHub remote. If not, the
[Module 5 student guide](../module-05-deploy-your-agent-and-sync-your-brain/student-guide.md)
rebuilds it.

```bash
ssh hermes@<your-droplet-ip>
docker ps
```

The `hermes` container should be running.

### Accounts

| Account | Why | Cost |
|---|---|---|
| [Backblaze B2](https://www.backblaze.com/cloud-storage) | Where the large files live | First 10 GB free, then 6.95 USD per TB per 30 days |

Sign up before the session. You will need a payment method on file even though the first
10 GB is free.

### Install rclone on your laptop

```bash
brew install rclone
rclone version
```

You need **v1.66 or newer**. Below that, bisync is not safe to use. Latest release was
v1.75.0 on 2026-08-26.

---

## Step 1: Measure your three categories

```bash
cd ~/SecondBrain

du -sh .

find . -type d -name node_modules -prune -o -type f -name "*.md" -print | wc -l

find . -type d -name node_modules -prune -o -type f -name "*.md" -print0 \
  | xargs -0 -n 500 du -ck | awk '/total/{s+=$1} END {printf "%.1f MB\n", s/1024}'

find . -type d -name node_modules -prune -o -type f \
  \( -iname "*.mp4" -o -iname "*.mov" -o -iname "*.mp3" -o -iname "*.wav" \
     -o -iname "*.jpg" -o -iname "*.png" -o -iname "*.pdf" \) -print0 \
  | xargs -0 -n 500 du -ck | awk '/total/{s+=$1} END {printf "%.2f GB\n", s/1024/1024}'

find . -type d -name node_modules | wc -l
```

Post your numbers in chat. For comparison, the host's vault on 2026-08-26:

| Category | Size | Count | Destination |
|---|---|---|---|
| Markdown | 5.1 MB | 449 files | GitHub, private |
| Media | 13.37 GB | 8,323 files | Backblaze B2 |
| `node_modules` | 3.25 GB | 1,005 directories | Nowhere |
| Whole vault | 19 GB | | |

> **There are three categories, not two.** Notes need history. Media needs durability.
> `node_modules` needs neither, because `npm install` rebuilds it faster than a restore
> would.

---

## Step 2: Audit what git is carrying

```bash
git count-objects -vH | grep size-pack
git ls-files | wc -l
git ls-files -z | xargs -0 -n 500 du -ck | awk '/total/{s+=$1} END {printf "%.1f MB\n", s/1024}'
git ls-files -z | xargs -0 ls -lS 2>/dev/null | head -10
```

| Answer | Pass mark |
|---|---|
| `size-pack` | Under 50 MB |
| Tracked file count | Your markdown count plus your scripts |
| Tracked total size | Single digit MB |
| Ten largest tracked files | No video, no audio, no `.env` |

If a big file is already committed, it stays in the repository history forever. Note it down
for homework. Do not try to fix it during the session.

---

## Step 3: Create the bucket

In the [Backblaze console](https://secure.backblaze.com), go to **Buckets**, then **Create a
Bucket**.

| Setting | Choice |
|---|---|
| Bucket name | `brain-<yourname>-<4 random chars>`, globally unique |
| Files are | **Private** |
| Default encryption | Enabled |
| Object lock | **Off** |

---

## Step 4: Create two keys

Go to **Application Keys**, then **Add a New Application Key**. Do this twice.

| Name | Bucket | Access | Goes on |
|---|---|---|---|
| `brain-laptop` | Your bucket only | **Read and Write** | Your laptop |
| `brain-server` | Your bucket only | **Read Only** | The Droplet |

> **Do not use the master key.** It has every capability on every bucket and cannot be
> scoped down.

> **The key is shown once.** Copy both the `keyID` and the `applicationKey` into a password
> manager before you close the dialog. Backblaze will not show them again.

---

## Step 5: Configure rclone on the laptop

```bash
rclone config
```

| Prompt | Answer |
|---|---|
| `n` | New remote |
| name | `brain` |
| Storage | `b2` |
| `account` | The **keyID** from `brain-laptop` |
| `key` | The application key |
| `hard_delete` | `false` |
| endpoint | Leave blank |

Then `q` to quit.

Test it with the full path:

```bash
rclone lsd brain:<BUCKET>
```

> **Always use the full path.** A bucket restricted key cannot list buckets, so bare
> `rclone lsd brain:` fails with an error that does not explain itself. Also make sure you
> put the **keyID** in the `account` field, not your account ID. B2 returns a bare 401 if
> you get that wrong.

---

## Step 6: Write the filter file

Create `~/SecondBrain/.rclone-filter`:

```text
- node_modules/
- .venv/
- venv/
- __pycache__/
- .next/
- dist/
- build/
- .git/
- .obsidian/
- .DS_Store
- .env
- .env.*
- auth.json
- *.md
+ **
```

Rules are read top to bottom and the first match wins.

> **`- *.md` is the most important line in this module.** B2 does not carry your notes. Git
> does. If both systems own the same file, a git pull followed by a sync can push the old
> version back over the new one, with no error and no warning.

Your `.gitignore` and your `.rclone-filter` are complements. Open them side by side. Every
file in the vault should be carried by exactly one of them, and nothing by both.

---

## Step 7: The first sync

Dry run first. Always.

```bash
cd ~/SecondBrain

rclone bisync . brain:<BUCKET>/vault \
  --filters-file .rclone-filter \
  --resync --dry-run --verbose
```

Read the output. If it looks right, run it for real:

```bash
rclone bisync . brain:<BUCKET>/vault \
  --filters-file .rclone-filter \
  --resync --verbose
```

This uploads everything and will take a while.

> **`--resync` runs once.** Not on every run. Included every time, a deleted file reappears
> at the end of every sync, because resync copies rather than syncs.
>
> **`--resync` means path1 wins.** A newer file on B2 gets overwritten by your laptop's
> version.
>
> You will need `--resync` again if you edit `.rclone-filter`. Bisync stores a hash of that
> file and stops if it changed.

---

## Step 8: Set up the access check

`RCLONE_TEST` marker files are not created for you.

```bash
cd ~/SecondBrain
touch RCLONE_TEST
rclone copyto RCLONE_TEST brain:<BUCKET>/vault/RCLONE_TEST
```

Every run from now on:

```bash
rclone bisync . brain:<BUCKET>/vault \
  --filters-file .rclone-filter \
  --check-access --max-lock 2m --resilient --recover \
  --conflict-resolve newer --verbose
```

| Flag | What it does |
|---|---|
| `--check-access` | Aborts unless it finds `RCLONE_TEST` on both sides |
| `--max-lock 2m` | Expires the lock file left behind by a killed run |
| `--resilient` | Lesser errors do not force a resync next time |
| `--recover` | An interrupted run picks up instead of demanding a resync |
| `--conflict-resolve newer` | Both sides changed. Keep the newer, rename the loser |

`--max-delete` is already on by default at 50 percent, and aborts if more than half the files
on either side would be deleted.

### Prove the guard works

```bash
mv RCLONE_TEST RCLONE_TEST.off
rclone bisync . brain:<BUCKET>/vault --filters-file .rclone-filter --check-access
mv RCLONE_TEST.off RCLONE_TEST
```

The middle command should refuse to do anything. That is the point.

> Object storage has no merge. Git merges text. **Nothing merges an MP4.** When both sides
> change the same video, one wins and the other gets a `.conflict` suffix.

---

## Step 9: Set up the server

```bash
ssh hermes@<your-droplet-ip>
sudo -v ; curl https://rclone.org/install.sh | sudo bash
rclone version
rclone config
```

Same steps as Step 5, but use the **read only** `brain-server` keyID and key.

Test it:

```bash
rclone ls brain:<BUCKET>/vault --max-depth 1
```

> **The server does not mirror the archive.** Its disk is 25 GB. It fetches files on demand
> into the git checkout at `/opt/brain`, where `.gitignore` already excludes them, so a
> fetched video lands exactly where the note expects it and git ignores it.

---

## Step 10: Give the agent a fetch skill

Run this on your **laptop**, so the skill travels through git.

```
Write a project skill so my agent can fetch media out of object storage on demand.

Create ~/SecondBrain/.hermes/skills/fetch-media/SKILL.md.

Name it fetch-media. The description should say it fetches large files that live in
Backblaze B2 rather than git, and that it should be used whenever a note references a
video, audio file, image or PDF that is not present on disk.

The skill instructs the agent to:
1. Take the path of the missing file, relative to the vault root.
2. Check available disk space with df before doing anything. If the file is larger than
   half the free space, stop and report the numbers instead of fetching.
3. Run: rclone copy brain:<BUCKET>/vault/<relative-path> <vault-root>/<containing-dir>/
   using the exact same relative path on both sides.
4. Confirm the file arrived, report its size, and say it is a cache copy that can be
   deleted.
5. Never run rclone delete, rclone sync, rclone purge or rclone bisync. Only rclone copy,
   rclone ls and rclone lsf. Say this explicitly in the skill.

Include the bucket name. Leave the vault root as a variable the agent resolves at runtime,
since it is ~/SecondBrain on my laptop and /opt/brain on the server.

Commit it with the message "skills: add fetch-media".
```

Push it, then on the server:

```bash
cd /opt/brain && git pull
docker exec -it hermes hermes skills trust /opt/brain
```

Now ask the server agent for a file that only exists in B2. It should fetch it and answer.

> **The read only key is the enforcement. The skill instruction is the reminder.** Step 5 of
> the brief stops the agent from trying. The key stops it from succeeding if it tries anyway.

---

## Step 11: Stop paying for old versions

B2 keeps every version of every file by default, and hides deleted files rather than
removing them. Check the difference:

```bash
rclone size brain:<BUCKET>/vault
rclone size brain:<BUCKET>/vault --b2-versions
```

Set a lifecycle rule:

```bash
rclone backend lifecycle brain:<BUCKET> \
  -o daysFromHidingToDeleting=30 \
  -o daysFromUploadingToHiding=0
```

That gives you 30 days of undo, after which old versions stop costing money.

```bash
rclone cleanup brain:<BUCKET>
```

That clears interrupted uploads older than 24 hours.

Then set a cap and an alert in the Backblaze console under **Caps & Alerts**.

---

## Step 12: Automate it

Paste this into your agent on the laptop:

```
Write ~/SecondBrain/scripts/sync-brain.sh on my laptop. It syncs the vault to Backblaze
B2 and it has to be safe to run unattended.

It must:
- use set -euo pipefail
- exit 0 quietly if another copy is already running, using a lock file in /tmp
- cd to ~/SecondBrain
- run rclone bisync . brain:<BUCKET>/vault with these flags exactly:
  --filters-file .rclone-filter --check-access --max-lock 2m --resilient --recover
  --conflict-resolve newer --log-level INFO --log-file ~/.rclone-brain.log
- never pass --resync, and never pass --force. If the run fails asking for --resync,
  the script must exit non zero and say so rather than adding the flag
- on failure only, print the hostname and the last five lines of the log to stderr
- on success, print the number of transfers and nothing else

Then:
1. Make it executable and run it once. Show me the output.
2. Add a launchd job or a crontab entry that runs it every 30 minutes and appends stdout
   and stderr to ~/.rclone-brain-cron.log.
3. Show me the schedule afterwards.

Do not use the agent to run the sync. This script has no model in it and does not need one.
```

> **Never let a script add `--resync` on its own.** Bisync asks for a resync when it has lost
> track of what changed. That is the system asking for a human. A script that answers by
> resyncing is a script that has been told path1 always wins.

Then break it on purpose. Rename `RCLONE_TEST`, wait for the cron run, and confirm you got
the failure output.

---

## If something breaks

| Symptom | Cause and fix |
|---|---|
| `401 unauthorized` from rclone | You put your account ID in `account` instead of the keyID. Rerun `rclone config` |
| `rclone lsd brain:` fails but `brain:<BUCKET>` works | Normal. A bucket restricted key cannot list buckets. Always use the full path |
| Bisync says it requires `--resync` | Something changed that it cannot reconcile. Run a `--dry-run --resync` first, read it, and only then decide. Do not put the flag in a script |
| Bisync refuses with a lock file error | A previous run was killed. `--max-lock 2m` prevents this. To clear it now, delete the lock in `~/.cache/rclone/bisync` on Linux or `~/Library/Caches/rclone/bisync` on macOS |
| Every run wants a resync after you edited the filter file | Working as designed. Bisync hashes the filter file. Run the resync once |
| A note's markdown got overwritten by an old version | `*.md` is missing from `.rclone-filter`. Two systems were syncing the same file |
| Your B2 storage keeps growing though the vault has not | Old versions. Run `rclone size --b2-versions` and set the lifecycle rule in Step 11 |
| The server agent says the file is not there | It is in B2 and not on the server. That is the design. Ask it to use `/fetch-media` |
| The server cannot delete something in B2 | Working as designed. The server key is read only |
| Bisync aborted saying too many deletions | `--max-delete` caught it at 50 percent. Find out why before you touch `--force` |

---

## Homework

1. Restore one file from B2 to a scratch directory and open it.
2. Delete a file locally, sync, then recover it from a hidden version.
3. Rename `RCLONE_TEST` on one side and confirm the sync refuses.
4. Run `rclone size --b2-versions` in a week and compare it to the live size.
5. Find your largest committed file. If it is over 10 MB, decide whether to rewrite history
   with `git filter-repo` and write down why.
6. Set a Backblaze cap and alert.
7. Add one directory to `.rclone-filter` and run the `--resync` that the change requires.

---

## What this cost

| Item | Amount |
|---|---|
| Backblaze B2 storage | First 10 GB free, then 6.95 USD per TB per 30 days. 13 GB is about 2 cents a month |
| Uploads | Free. B2 does not charge for data sent to it |
| Downloads | Free up to 3x your average monthly storage, then 0.01 USD per GB |
| API calls | Class A, B and C are free on pay as you go |
| Server | Already running from Module 5, about 6 USD a month |

All figures checked against the Backblaze pricing page on 2026-08-26.

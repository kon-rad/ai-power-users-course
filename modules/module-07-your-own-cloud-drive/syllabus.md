# Module 7: Your Own Cloud Drive

**Status:** syllabus draft v1, planning phase.
**Date:** 2026-08-26
**Platform:** macOS on your side, the Ubuntu server from Module 5 on the other. Every local
command is the Mac one.
**Prereq:** Module 5 complete. A private vault repository, a hardened Droplet, and Hermes in
Docker with the vault at `/opt/brain`. Module 6 is optional and only affects the last
segment.
**Companions:** `student-guide.md` · `agenda.md` · `quiz.md` · `luma-description.md`

---

## Format

**One session, 60 minutes, no break, live on YouTube.**

| Part | Time | Theme |
|---|---|---|
| **Part 1** | 0:00-0:28 | The split. Three categories, one bucket, two keys, the first sync |
| **Part 2** | 0:28-1:00 | The loop. The guard that stops a wipe, the server side, versions, automation |

**The spine:** Module 5 put the vault in git and moved it to a server. Git carries 5 MB of
markdown. The vault is 19 GB. Today the other 18.99 GB gets somewhere to live, and both
agents get a way to reach it.

**What is different about this module:** it is the cheapest one in the course. Backblaze B2
costs 6.95 USD per TB per month and the first 10 GB is free, so a 13 GB media library bills
at about 2 cents a month. The expensive thing here is not money, it is the chance of running
a two way sync wrong and deleting files on both sides at once. That is what Part 2 is for.

---

## Learning objectives

By the end, a student can:

1. Sort a folder into three categories, not two, and say which one is the reason their disk
   is full
2. Say why git is the wrong tool for a 1.8 GB video and object storage is the wrong tool for
   a note
3. Create a Backblaze B2 bucket and an application key scoped to one bucket with one
   capability set
4. Write a single filter file that defines the split, and explain why it has to be the
   complement of `.gitignore`
5. Run `rclone bisync` safely: dry run first, resync once, then never again
6. Explain what `--check-access` protects against, and demonstrate the failure it catches
7. Give a server read only access to the archive, and say what a compromised server can and
   cannot do to it
8. Set a lifecycle rule on old versions, and say what the bill looks like without one

---

# PART 1: The split (28 min)

## 0:00-0:03 · Open with the payoff (3 min)

Two windows. A terminal on the laptop, a Telegram chat with the server agent.

Ask the server agent for a video that exists only on the laptop. It fails. Say so out loud.

Run one command on the laptop. Ask again. This time the server fetches the file out of
object storage, and answers.

Then say the number: **19 GB of vault, and the bill is about 2 cents a month.**

> "Module 5 gave both machines the same notes. It did not give them the same folder. Git
> carries five megabytes and stops. Everything you actually made, the video, the audio, the
> images, the exports, is still sitting on one laptop with no second copy."

---

## 0:03-0:08 · Measure the three categories (5 min)

Module 5 measured two things: the vault, and the markdown inside it. That was enough to
justify git. It is not enough to design a sync.

Students run these on their own vault and read the answers into chat.

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

The host's own numbers, measured 2026-08-26:

| Category | Size | Count | Where it goes |
|---|---|---|---|
| Markdown | **5.1 MB** | 449 files | GitHub, private |
| Media: video, audio, images, PDFs | **13.37 GB** | 8,323 files | Backblaze B2 |
| `node_modules` | **3.25 GB** | 1,005 directories | **Nowhere** |
| Whole vault | **19 GB** | | |

Video alone is 10.49 GB of that. The single largest file is a 1.8 GB drone clip.

### The point of the third row

Everyone splits a folder into "small" and "large" and then wonders why the sync is still
enormous. There are three categories, and the third one is the reason the disk is full:

| | Needs history | Needs durability | Rebuildable |
|---|---|---|---|
| Notes, code, skills | Yes | Yes | No |
| Video, audio, images | No | Yes | No |
| `node_modules`, `.venv`, build output | No | No | **Yes** |

**A thing you can regenerate with one command is not data.** Syncing 1,005 copies of
`node_modules` costs you upload time, storage, and a restore that takes longer than
`npm install` would have.

### Host guidance

Read the ratio out loud. The vault is roughly **3,800 times bigger** than the part git
carries. That is not an argument for a bigger git. It is an argument for two systems.

Students with a small vault should still do this. A 400 MB vault has the same three
categories in different proportions, and the exercise is the sorting, not the total.

---

## 0:08-0:13 · Audit what git is carrying (5 min)

Short segment. Module 5 wrote the ignore list, this confirms it held.

```bash
cd ~/SecondBrain

git count-objects -vH | grep size-pack

git ls-files | wc -l

git ls-files -z | xargs -0 -n 500 du -ck | awk '/total/{s+=$1} END {printf "%.1f MB\n", s/1024}'

git ls-files -z | xargs -0 ls -lS 2>/dev/null | head -10
```

Four answers, and each one has a pass mark:

| Command | What good looks like |
|---|---|
| `size-pack` | Under 50 MB. This is what every clone downloads |
| `git ls-files \| wc -l` | Roughly your markdown count plus your scripts |
| Total tracked size | Single digit MB |
| The ten largest tracked files | No video, no audio, no `.env`, nothing over a few MB |

### If a large file is already committed

It is in the object store forever, in every clone, whether or not you delete it now. That is
the Module 5 lesson arriving late. Two options, and the module does not do either live:

| Option | Cost |
|---|---|
| Leave it | Every clone pays for it once, forever |
| Rewrite history with `git filter-repo` | Every existing clone breaks and has to be re-cloned |

**For a personal vault with two clones, rewriting is cheap and worth doing.** Say that,
name the tool, and put it in the homework rather than spending eight minutes on it live.

### Host guidance

Say "private" out loud again. If a student's vault repository is public, this is the last
moment before they push their filter file and their bucket name into the open. The bucket
name is not a secret, but the habit is the point.

---

## 0:13-0:19 · The bucket and the two keys (6 min)

### Why object storage and not a sync app

| | Dropbox, iCloud, Drive | Backblaze B2 |
|---|---|---|
| Price | **9.99 USD a month for 2 TB**, Dropbox Plus, checked 2026-08-26 | **6.95 USD per TB per 30 days**, first 10 GB free, checked 2026-08-26 |
| You pay for | The plan | The bytes |
| 13 GB costs | 9.99 | **About 2 cents** |
| 2 TB costs | 9.99 | 13.90 |
| Your agent can read it | Through a desktop app, on one machine | Through one command, on any machine |

**Read the last two rows together and do not skip the second one.** At 2 TB, B2 is more
expensive than Dropbox. The win here is not that object storage is always cheaper. It is
that you pay for 13 GB instead of buying a 2 TB plan to hold it, and that a server in
another country can reach it with a credential you scoped yourself.

### Create the bucket

In the Backblaze web console: **Buckets**, then **Create a Bucket**.

| Setting | Choice | Why |
|---|---|---|
| Bucket name | Globally unique across all of B2. Use `brain-<yourname>-<4 random chars>` | It is not a secret, but it should not be guessable |
| Files are | **Private** | The only setting on this page that matters |
| Encryption | Server side, enabled | Free, and it is one click |
| Object lock | **Off** | It makes files undeletable for a set period. Powerful, and a foot gun on a bucket you are learning on |

### Two keys, not one

The console offers a master key. Do not use it. It has every capability on every bucket, and
it is the one credential that cannot be scoped down.

| Key | Lives on | Capabilities | Bucket |
|---|---|---|---|
| `brain-laptop` | Your laptop | Read and write | Just this one |
| `brain-server` | The Droplet | **Read only** | Just this one |

B2 application keys can be restricted to one bucket, to a file name prefix, to a capability
set, and to an expiry in seconds. Verified against the Backblaze application key
documentation, 2026-08-26.

> "The server runs unattended, on the internet, with an agent that can execute commands. It
> needs to read your archive. It has never once needed to delete it. So do not give it a key
> that can."

### Host guidance

Crop the key when it appears. B2 shows the application key exactly once and never again,
which is worth saying while it is on screen, because a student who clicks away has to make a
new one.

Name the trade off: a read only server cannot publish media it generates. If that becomes a
requirement later, the answer is a third key with write access restricted to one name
prefix, not upgrading this one.

---

## 0:19-0:28 · The filter file, and the first sync (9 min)

### Install

```bash
brew install rclone           # macOS
rclone version
```

Latest release is **v1.75.0**, checked 2026-08-26. Anything at v1.66 or newer has the
snapshot model that makes bisync survive a file changing mid run. Below that, do not use
bisync at all.

### Configure the remote

```bash
rclone config
```

Choose `n` for new remote, name it `brain`, storage type `b2`. Then:

| Prompt | What to paste |
|---|---|
| `account` | The **keyID**, not your account ID |
| `key` | The application key |
| `hard_delete` | `false` for now. Segment 0:44 explains why |
| `endpoint` | Leave blank |

**Put the keyID in `account`.** A restricted application key with your master account ID in
that field returns 401 and the error does not say why. Verified against the rclone B2
documentation, 2026-08-26.

Then always address the full path, never the bare remote:

```bash
rclone lsd brain:brain-yourname-a7f3        # works
rclone lsd brain:                           # fails on a bucket restricted key
```

### One file defines the split

`~/SecondBrain/.rclone-filter`:

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

Rules are read in order and the first match wins. Everything not matched is included, and
the final `+ **` says so out loud.

### The line that is doing the real work

`- *.md`

**B2 does not carry your notes.** Git does. That looks like a gap and it is the most
important decision in the module.

> "Never let two sync systems own the same file. Git pulls a note you edited on the server.
> Bisync sees a local file that is now older than its own record, decides it is a change,
> and pushes the old one back over it. Nothing errored. You just lost the edit."

`.gitignore` and `.rclone-filter` are complements. Every file the vault contains is carried
by exactly one of them, and nothing is carried by both. Read the two files side by side on
camera.

### The dry run, then the resync

```bash
cd ~/SecondBrain

rclone bisync . brain:brain-yourname-a7f3/vault \
  --filters-file .rclone-filter \
  --resync --dry-run --verbose
```

Read the output. Then run it for real without `--dry-run`.

```bash
rclone bisync . brain:brain-yourname-a7f3/vault \
  --filters-file .rclone-filter \
  --resync --verbose
```

### Three rules about `--resync`

| Rule | Detail |
|---|---|
| It runs **once** | Only on the first run between these two paths |
| It is a copy, not a sync | Included in every run, a deleted file reappears at the end of every run |
| Path1 wins | `--resync` implies `--resync-mode path1`. A newer file on B2 is overwritten by the laptop |

You also have to run it again after editing `.rclone-filter`. Bisync stores a hash of the
filter file and refuses to continue if it changed. Verified against the rclone bisync
documentation, 2026-08-26.

### Host guidance

The upload takes longer than the segment. Start it, leave it running in a visible pane, and
talk over it. Have a bucket with the same tree already uploaded in reserve so 0:36 has
something to fetch from regardless.

Say the `--dry-run` habit as a rule, not a suggestion. **rclone's own manual calls bisync an
advanced command and says data loss can result.** Quoting the tool's own warning lands
better than adding your own.

---

# PART 2: The loop (32 min)

## 0:28-0:36 · The guard that stops a wipe (8 min)

This is the segment that justifies the module.

### The failure

An external drive does not mount. A path has a typo. A `cd` silently failed. Bisync looks at
one empty side, concludes every file was deleted, and propagates that.

### Two guards, and only one is on by default

| Guard | Default | What it does |
|---|---|---|
| `--max-delete` | **On, at 50 percent** | Aborts if more than half the files on either side would be deleted |
| `--check-access` | **Off** | Aborts unless it finds matching marker files on both sides |

`--max-delete` at 50 percent is real protection and it is not enough. Delete 40 percent of a
vault and it proceeds without comment.

### Set up the markers

`RCLONE_TEST` files are **not created for you**. Verified against the rclone bisync
documentation, 2026-08-26.

```bash
cd ~/SecondBrain
touch RCLONE_TEST
rclone copyto RCLONE_TEST brain:brain-yourname-a7f3/vault/RCLONE_TEST
```

Then every run carries the flag:

```bash
rclone bisync . brain:brain-yourname-a7f3/vault \
  --filters-file .rclone-filter \
  --check-access --max-lock 2m --resilient --recover \
  --conflict-resolve newer --verbose
```

### The demonstration

Rename the local marker. Run bisync. It aborts and nothing moves. Rename it back.

**Do this on camera.** A safety feature nobody has watched fire is a safety feature nobody
trusts.

### The four flags after `--check-access`

| Flag | What it buys |
|---|---|
| `--max-lock 2m` | A run killed mid flight leaves a lock file. Without this it never expires and every later run refuses |
| `--resilient` | Lesser errors do not force a `--resync` on the next run |
| `--recover` | An interrupted run picks up rather than demanding a resync |
| `--conflict-resolve newer` | Both sides changed. Keep the newer one and rename the loser rather than stopping |

### The honest limit on `--conflict-resolve newer`

It resolves by modification time, which means it can be wrong. A file touched by a script at
3am beats an edit you made at 2am. The loser is renamed, not deleted, so a wrong call is
recoverable. **Recoverable is the standard here, not correct.**

### Host guidance

Object storage has no merge. Two machines editing the same 200 MB video means one version
wins and the other gets a suffix. Git merges text. Nothing merges an MP4. Say that plainly,
because students who have used git for years assume the conflict handling transfers.

---

## 0:36-0:44 · The server side, without a second copy (8 min)

### Why the server does not mirror

The Module 5 Droplet has a 25 GB disk. Ubuntu and Docker take a chunk of it. A 13.37 GB
media library fits, badly, and stops fitting the month you shoot more video.

| Approach | Disk needed | Verdict |
|---|---|---|
| Full bisync on the server | 13.37 GB and growing | Wrong. Also gives the server delete rights |
| `rclone mount` with a VFS cache | Small, grows as read | Elegant. Needs FUSE inside Docker, which means loosening container privileges |
| **Fetch on demand into the git checkout** | Only what was asked for | **This one** |

The third option works because of a property you already built. `.gitignore` excludes
`*.mp4`. So a video fetched to `/opt/brain/Areas/x/clip.mp4` lands exactly where the note
expects it and git ignores it completely. The two files written in Part 1 make this free.

### Install and configure on the Droplet

```bash
ssh hermes@<your-droplet-ip>
sudo -v ; curl https://rclone.org/install.sh | sudo bash
rclone version
rclone config          # remote name: brain, type: b2, the READ ONLY keyID and key
```

### The skill, which travels through git

Project skills from Module 5 live in `~/SecondBrain/.hermes/skills/` and reach the server on
the next pull. Write it once, on the laptop.

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

Push, pull on the server, then:

```bash
docker exec -it hermes hermes skills trust /opt/brain
```

### Prove it

Ask the server agent, in Telegram or over SSH, for a file that is only in B2. It reads the
note, notices the file is missing, uses the skill, fetches it, and answers.

### Host guidance

Step 5 of the brief is the one to point at. **The read only key is the enforcement and the
skill is the reminder.** Two layers, and the module says which one is which. The key stops
it. The instruction stops it from trying.

Have the fetch target be something small. A 1.8 GB pull is dead air.

---

## 0:44-0:51 · Versions, and the bill that grows quietly (7 min)

### What B2 does by default

Every overwrite creates a new version. Every delete hides a file rather than removing it.
Nothing ever leaves. Your storage graph goes up and never comes down, and you find out on a
bill.

```bash
rclone size brain:brain-yourname-a7f3/vault
rclone size brain:brain-yourname-a7f3/vault --b2-versions
```

Two different numbers. The second is what you pay for.

### The fix, in one command

```bash
rclone backend lifecycle brain:brain-yourname-a7f3 \
  -o daysFromHidingToDeleting=30 \
  -o daysFromUploadingToHiding=0
```

| Setting | Value | Meaning |
|---|---|---|
| `daysFromUploadingToHiding` | `0` | Never hide the current version automatically |
| `daysFromHidingToDeleting` | `30` | An old or deleted version is permanently removed after 30 days |

Thirty days of undo, then it stops costing money. Then clean up interrupted uploads:

```bash
rclone cleanup brain:brain-yourname-a7f3
```

### The pricing that actually applies

Checked against the Backblaze pricing page, 2026-08-26:

| Item | Price |
|---|---|
| Storage | **6.95 USD per TB per 30 days**, pay as you go |
| Free storage | First 10 GB, always |
| Upload | Free. There is no charge to send data to B2 |
| Egress | Free up to **3x** your average monthly storage, then 0.01 USD per GB |
| Class A, B and C API calls | Free for pay as you go |
| Class D API calls | 0.004 USD per 10,000, first 2,500 a day free |

So the host's 13.37 GB: 10 GB free, 3.37 GB billable, **about 2 cents a month.** The 3x
egress allowance means roughly 40 GB of downloads a month before egress costs anything, and
a fetch on demand server nowhere near touches it.

### Set the cap

Backblaze offers caps and alerts on the account. Set them. This is a bucket an agent can
write to, and an agent in a loop uploading the same file is a thing that happens.

### Host guidance

Say the versions number out loud after a few days of real syncing. It is always larger than
people expect and it is the single most common surprise on an object storage bill.

Do not oversell 2 cents. The correct claim is that storage is not the cost. **The cost of
this module is the attention it takes to run a two way sync without breaking it**, and that
cost is real.

---

## 0:51-0:57 · Automate it, and make it tell you when it breaks (6 min)

### The rule from Module 5, applied again

A job with no reasoning in it should not cost tokens. There is nothing to think about here.
Write the script, cron it, and let the model stay out of it.

### The brief

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

### The line that matters most

**Never let a script add `--resync` on its own.** Bisync demands a resync when it has lost
confidence about what changed. That is the system asking for a human. A script that answers
by resyncing has been handed a rule that says path1 wins, and it will happily flatten the
other side.

### Wire it to your phone

If you did Module 6, one line turns a silent failure into a message. Run this on the server
where the gateway lives:

```bash
docker exec hermes hermes send --to telegram "sync-brain failed on $(hostname)"
```

On the laptop, the same idea needs the token, so the simpler route is having the failure
path write to a file the server's morning digest already reads. Either works. Silence on
success, either way.

### Host guidance

Break it on purpose, the same as Module 6. Rename `RCLONE_TEST`, let the cron job fire,
show the failure message arriving. Two modules in a row proving an alarm before relying on
it is the habit being taught, not the tool.

---

## 0:57-1:00 · Close (3 min)

| | |
|---|---|
| What today added | The whole folder, on both machines, split across two systems that each do one job |
| What it cost | About 2 cents a month for 13 GB. The server was already running |
| What is now permanent | Every large file has a second copy that is not on a laptop |

Where the copies actually are, which is the answer to "am I backed up":

| Content | Copies | Where |
|---|---|---|
| Notes, code, skills | **Three** | Laptop, server, GitHub. Every git clone is complete |
| Video, audio, images, PDFs | **Two** | Laptop and B2. The server holds only what it fetched |
| `node_modules`, caches, build output | **One, and that is correct** | Wherever you last ran the install |

**The honest caveat, said plainly:**

> "Two way sync is the sharpest tool in this course. rclone's own manual calls bisync an
> advanced command and says data loss can result if you have not read it. Every guard we set
> today, the dry run, the access check, the max delete, the refusal to auto resync, exists
> because the failure mode is not an error message. It is a folder that is quietly emptier
> than it was."

---

# Homework

1. Restore one file from B2 to a scratch directory and open it. A backup nobody has restored
   from is not a backup.
2. Delete a file locally, sync, then bring it back from a hidden version before your
   lifecycle window closes.
3. Rename `RCLONE_TEST` on one side and confirm the sync refuses. Put it back.
4. Run `rclone size --b2-versions` a week from now and compare it to the live size.
5. Find your largest committed file with `git ls-files -z | xargs -0 ls -lS | head`. If it is
   over 10 MB, decide whether to rewrite history and write down why.
6. Set a Backblaze cap and alert on the account.
7. Add one directory to `.rclone-filter` that you have decided not to sync, and run the
   `--resync` that the change requires.

# Peer quiz

Five multiple choice on the three categories, the filter and ignore complement, `--resync`,
`--check-access` and key scoping. Four open ended, peer scored. One peer exercise where
students audit each other's filter file against their ignore list. Detail in `quiz.md`.

# Prep checklist

## Must verify before the stream

- [ ] **Backblaze free tier on the signup page the morning of the stream.** The pricing page
      said first 10 GB free on 2026-08-26. Read it off the page rather than repeating it
- [ ] That a bucket restricted application key without `listAllBucketNames` really does fail
      on a bare `rclone lsd brain:` and succeeds on the full path. The segment at 0:19 says
      it does
- [ ] The exact bucket creation screen. Backblaze changes this console and the encryption
      and object lock toggles have moved before
- [ ] `rclone backend lifecycle` accepting `-o daysFromHidingToDeleting=30` on the installed
      version, against a throwaway bucket
- [ ] Whether `curl https://rclone.org/install.sh | sudo bash` gets a current version on
      Ubuntu, or lags far enough behind v1.66 to matter. **This decides whether the server
      segment works as written**
- [ ] `hermes skills trust /opt/brain` picking up a newly pulled skill without a container
      restart
- [ ] Whether `--conflict-resolve newer` is reliable against B2 given that B2 stores
      modification time as upload metadata

## Assets

- [ ] A bucket already populated with the vault tree, in reserve, so the fetch demo at 0:36
      does not depend on the live upload finishing
- [ ] A small file, under 5 MB, chosen in advance as the fetch demo target
- [ ] The host's three category table on a slide
- [ ] A second application key created and deleted in rehearsal, so the console flow is
      familiar and the crop region is preset
- [ ] A `.rclone-filter` and a `.gitignore` open side by side in one window
- [ ] The Module 5 Droplet in a known good state with `/opt/brain` populated

# Open questions

1. **Does the upload run live or is it pre seeded?** A real 13 GB upload is honest and it is
   also forty minutes of nothing. Recommend starting a real one on camera, then cutting to a
   pre seeded bucket for every later segment and saying so.
2. **Is `rclone mount` worth showing at all?** It is the better answer for anyone with a
   larger Droplet and it needs FUSE privileges inside Docker. Recommend naming it in the
   table at 0:36 and not demonstrating it.
3. **Should the module teach `git filter-repo` for students who already committed video?**
   It is the correct fix and it is a ten minute segment on its own. Recommend homework, with
   the tool named.
4. **Backblaze versus Cloudflare R2 or Wasabi.** All three would work. Recommend picking one
   and saying the interface is rclone either way, so switching later costs one config entry.
5. **Windows students.** Module 0 covers Windows 11 and this module is written for macOS
   throughout. Recommend an appendix in the student guide rather than dual commands in every
   segment, which doubles the length.

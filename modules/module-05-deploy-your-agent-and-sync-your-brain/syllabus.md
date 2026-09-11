# Module 5: Deploy Your Agent and Sync Your Brain

**Status:** syllabus draft v1, planning phase.
**Date:** 2026-08-23
**Platform:** macOS on your side, Ubuntu on the server. Every local command is the Mac one.
**Prereq:** Module 3 complete. A working Hermes agent and a second brain vault at
`~/SecondBrain`.
**Companions:** `student-guide.md` · `agenda.md` · `quiz.md` · `luma-description.md`

---

## Format

**One session, 60 minutes, no break, live on YouTube.**

| Part | Time | Theme |
|---|---|---|
| **Part 1** | 0:00-0:26 | The brain becomes a repository. Measure it, ignore it correctly, move the skills in |
| **Part 2** | 0:26-1:00 | The server. Create it, harden it, run the agent on it, close the loop |

**The spine:** every module so far ran on the student's laptop. Close the lid and the agent
stops existing. Today it moves to a machine that does not close, and the notes and skills
stay identical on both.

**What is different about this module:** it is the first one with a recurring cost. A
Droplet is about 6 USD a month and it keeps billing after the session ends. Say that in the
first two minutes, and say how to destroy it.

---

## Learning objectives

By the end, a student can:

1. Measure what a vault actually contains, and separate the part that has to sync from the
   part that does not
2. Initialise a git repository with the ignore list correct before the first commit, and
   say why that order cannot be reversed later
3. Move a skill into a repository so it loads as a project skill, and confirm the agent
   picked it up
4. Create a DigitalOcean Droplet and apply the hardening steps in an order that does not
   lock them out
5. Run Hermes in Docker against a persistent data volume, and extend the write sandbox to a
   second directory
6. Pick a deploy credential scoped to one repository, and say what it can and cannot do
7. Schedule a push job that stays silent unless it fails
8. Name the files that must never sync between two agents, and the failure each one causes

---

# PART 1: The brain becomes a repository (26 min)

## 0:00-0:03 · Open with the payoff (3 min)

Two windows on screen. Obsidian on the laptop, a terminal on the Droplet.

Type one brief into the server agent. It researches something short and writes a note. Do
not touch the laptop. Twenty seconds later the note appears in Obsidian.

Then say the number: **about 6 USD a month, and it bills until you destroy it.**

> "Everything you have built so far dies when you close the laptop. That is fine for a tool.
> It is useless for an assistant. Today it moves somewhere that does not close."

---

## 0:03-0:08 · Measure your own vault (5 min)

Nobody designs a sync until they know what they are syncing. Students run four commands on
their own vault and read the answers out in chat.

```bash
du -sh ~/SecondBrain
find ~/SecondBrain -name "*.md" -not -path "*/node_modules/*" | wc -l
find ~/SecondBrain -name "*.md" -not -path "*/node_modules/*" -exec du -ck {} + | tail -1
find ~/SecondBrain -type d -name node_modules | wc -l
```

The host's own numbers, measured 2026-08-23, are the teaching:

| Question | Answer |
|---|---|
| How big is the vault | 16 GB |
| How many markdown files | 414 |
| How big are those files together | **4.7 MB** |
| How many `node_modules` directories | 974 |

### Host guidance

Say the ratio out loud. The vault is roughly three thousand times bigger than the part that
matters. Every heavyweight option people reach for first, block storage, object storage,
continuous file sync daemons, exists to solve a 16 GB problem. There is no 16 GB problem.

Students whose numbers differ should say so in chat. A vault that is genuinely 400 MB of
markdown changes nothing about the approach.

---

## 0:08-0:16 · Git, and the order that cannot be reversed (8 min)

The vault becomes a git repository. The ignore list is written **before** the first commit.

### Why the order is the whole segment

Git tracks a file from the moment it is first committed. Commit 15 GB of video, then add
the ignore rule, and the video is still in the object store. Every clone downloads it
forever. Undoing it means rewriting history, which breaks every clone that already exists.

This is the same lesson as `.gitignore` before `.env` in Module 3, with a bigger blast
radius and no way to revoke your way out of it.

### The brief

```
Turn my vault at ~/SecondBrain into a git repository. Do it in this order and stop if any
step fails.

1. Confirm there is no .git directory here yet. If there is, stop and tell me.
2. Write .gitignore FIRST, before any git init and before any commit. Ignore:
   - node_modules/, .venv/, __pycache__/, *.pyc
   - *.mp4, *.mov, *.mp3, *.opus, *.wav, *.jpg, *.jpeg, *.png, *.gif, *.pdf
   - .env and .env.*
   - .obsidian/workspace.json, .obsidian/workspace-mobile.json, .obsidian/cache
   - .DS_Store
3. Run git init.
4. Run git add -A, then git status, and show me the file count and total size that would
   be committed. Do not commit yet.
5. If that number is above 50 MB, stop and tell me what is large. Do not commit.
6. Report any nested .git directories you found inside the vault and leave them alone.

Then wait for me before committing.
```

### Host guidance

Step 4 is the gate and it is the point of the brief. Read the number on camera before
committing. A student whose staged size comes back at 900 MB has an ignore rule missing,
and finding that before the commit is the entire lesson.

Nested repositories are common and confusing. The host's vault has two. Name them, say they
get cloned separately or added as submodules, and move on. Do not solve submodules live.

Push to a **private** GitHub repository. Say the word private out loud.

---

## 0:16-0:26 · The skills move into the repo (10 min)

The skills follow the same repository, and Hermes has a mechanism built for exactly this.

### Project skills

When Hermes runs inside a git checkout, it looks for skills in:

```text
<project-root>/.hermes/skills/
<project-root>/.agents/skills/
```

The project root is the nearest ancestor directory containing `.git`. Skills found there
are not loaded until the repository is trusted once:

```bash
hermes skills trust
```

Four properties, and each one is a reason to prefer this over installing skills on both
machines:

| Property | What it gets you |
|---|---|
| Highest precedence tier: project, then local, then external dirs | A repo skill wins inside the repo without touching the global profile |
| The autonomous curator never modifies project skill directories | The server cannot quietly rewrite a skill you authored |
| New agent created skills always go to `~/.hermes/skills/` | The repo stays authored, not accumulated |
| Every project skill is rescanned when its content changes | A `git pull` that brings a poisoned skill gets it quarantined, not loaded |

Verified against the Hermes skills documentation, 2026-08-23.

### The brief

```
Move my skills into the vault repository so they travel with it.

1. List what is in ~/.hermes/skills/ and tell me which ones I wrote and which ones shipped
   with Hermes. Only the ones I wrote are moving.
2. Create ~/SecondBrain/.hermes/skills/ and move my own skills there. Copy, verify, then
   remove the original. Do not delete anything until you have confirmed the copy.
3. Run: hermes skills trust
4. Start a fresh session inside ~/SecondBrain and run /skills. Show me the output.
   My moved skills should appear and be tagged as project skills.
5. Commit the result with the message "skills: move to project skills".

If any skill fails to appear in step 4, stop and tell me which one and what the error was.
```

### Host guidance

Step 4 is the proof and it has to happen on camera. A skill that was moved but does not load
is worse than one that was never moved, because the student thinks it works.

The trap to name: **project skills only load for sessions started inside the trusted
repository.** A cron job whose working directory is elsewhere loads none of them. That
matters in the last segment.

---

# PART 2: The server (34 min)

## 0:26-0:34 · The Droplet, created and hardened (8 min)

### What to create

| Setting | Choice | Why |
|---|---|---|
| Image | Docker on Ubuntu, from the Marketplace | Docker is preinstalled, which saves five minutes live |
| Size | Basic, 1 vCPU, 1 GB, 25 GB SSD | **6.00 USD a month**, checked 2026-08-23 |
| Authentication | **SSH key.** Never a password | A password on port 22 is scanned within minutes of the IP existing |
| Backups | Weekly | 20 percent of the Droplet price, about 1.20 USD a month |

The 512 MB Droplet at 4 USD runs Hermes but will not survive a Docker build. Say that
rather than letting students discover it.

### The hardening, in an order that does not lock you out

| # | Step | The reason it is at this position |
|---|---|---|
| 1 | Create a non root user, add to `sudo`, copy `authorized_keys` to it | Everything after this runs as that user |
| 2 | Add the Cloud Firewall rule for port 22 **before** enabling anything else | Free, and it is the layer that survives you breaking UFW |
| 3 | Confirm you can open a second SSH session as the new user | **Do not close the first session until this works** |
| 4 | `PermitRootLogin no`, `PasswordAuthentication no`, restart ssh | Closes the only door brute force can open |
| 5 | UFW mirroring the Cloud Firewall | Host level backstop |
| 6 | `fail2ban` | Rate limits what does get through |
| 7 | `unattended-upgrades` | The box is unattended by design |

### Host guidance

Step 3 is the one that saves the session. Keep the original root session open in a visible
window and say why. Locking yourself out of a Droplet live is memorable and costs ten
minutes to recover.

Two firewalls is not redundancy. The Cloud Firewall blocks traffic before it reaches the
machine; UFW still applies if the Cloud Firewall is detached or misconfigured. Both are free.

> "The whole reason this is safe is that nothing is listening. No web server, no dashboard,
> no open port except SSH from your own address. A server with one door is a server you can
> reason about."

---

## 0:34-0:42 · Hermes in Docker (8 min)

### Setup, then run

```bash
mkdir -p ~/.hermes
docker run -it --rm -v ~/.hermes:/opt/data nousresearch/hermes-agent setup
```

The wizard asks for the model provider key and writes it to `~/.hermes/.env` on the host,
where it persists across container restarts.

```bash
sudo mkdir -p /opt/brain

docker run -d --name hermes --restart unless-stopped \
  -v ~/.hermes:/opt/data \
  -v /opt/brain:/opt/brain \
  -e HERMES_WRITE_SAFE_ROOT=/opt/data:/opt/brain \
  nousresearch/hermes-agent gateway run
```

### The two traps in that command

**No `-p` flag.** Port 8642 exposes the API server and the dashboard. Neither is needed and
the Hermes documentation is blunt about it: opening any port on an internet facing machine
is a risk you should not take unless you understand it. Nothing about this module needs an
open port.

**`HERMES_WRITE_SAFE_ROOT` has to be extended.** The official image sets it to `/opt/data`
so the agent cannot escape its data volume. Mount the vault at `/opt/brain` without that
environment variable and every `write_file` to a note fails with an `outside
HERMES_WRITE_SAFE_ROOT` error. Verified against the Hermes environment variable reference,
2026-08-23.

### Two settings that are wrong by default for an unattended box

```yaml
# /opt/data/config.yaml
approvals:
  mode: manual
tool_loop_guardrails:
  hard_stop_enabled: true
  hard_stop_after:
    exact_failure: 5
    idempotent_no_progress: 5
checkpoints:
  enabled: true
```

`approvals.mode` defaults to `smart`, which auto approves low risk commands. That is
reasonable when a person is watching the terminal. `tool_loop_guardrails.hard_stop_enabled`
defaults to `false`, which the Hermes documentation itself flags as the wrong default for
unattended gateways. Nobody is watching this box.

### Host guidance

Run `docker exec -it hermes hermes` and have a two line conversation on camera. That is the
moment the server stops being an abstraction.

---

## 0:42-0:50 · Clone the brain and trust the repo (8 min)

### The credential

The server needs write access to one repository and nothing else.

| | **Fine grained PAT** | **Deploy key** |
|---|---|---|
| Scope | One repository, contents write | One repository |
| Expiry | **Mandatory** | None |
| Reusable as a shell credential | No | It is an SSH key |

Take the token. The mandatory expiry is the feature, not the inconvenience. It also keeps
this module's SSH key doing exactly one job, which is getting you into the Droplet.

### The steps

```bash
sudo chown -R $(docker exec hermes id -u):$(docker exec hermes id -g) /opt/brain
git clone https://github.com/<you>/<your-brain-repo>.git /opt/brain
docker exec -it hermes hermes skills trust /opt/brain
```

### The brief

```
You are running on my server now. Confirm the setup before we go further.

1. Show me the output of: ls /opt/brain
2. Run /skills and confirm my project skills are loaded and tagged as project skills.
3. Write a file at /opt/brain/Research/server-hello.md containing the current date, the
   hostname, and one sentence saying which machine you are.
4. Read it back and show me the contents.

If step 3 fails with a write error, show me the exact error text. Do not retry it and do
not work around it.
```

### Host guidance

Step 3 failing with `outside HERMES_WRITE_SAFE_ROOT` is the most likely live failure and
the brief is written to surface it rather than route around it. If it happens, fix the
environment variable on camera. It teaches better than a clean run.

The ownership command exists because the container runs as a non root user whose UID is not
necessarily yours. Read the UID out loud rather than assuming it.

---

## 0:50-0:57 · Close the loop (7 min)

### Write lanes, not conflict resolution

Conflicts are cheaper to prevent than to resolve. Give each machine a lane and write it into
`HERMES.md` so both agents read it.

| Path | Owner |
|---|---|
| `Research/`, `Areas/*/research/` | Server writes freely |
| `Projects/` | Laptop |
| `.hermes/skills/` | Laptop, authored deliberately |
| Existing notes elsewhere | Server appends, does not rewrite |

Nothing enforces this. It is a convention plus one paragraph in a context file, and it turns
a weekly merge conflict into a rare one.

### The brief

```
Set up the sync loop on this server.

1. Write /opt/brain/scripts/push-brain.sh. It must:
   - cd to /opt/brain
   - git add -A
   - exit 0 quietly if there is nothing staged
   - commit with the message "agent: " plus an ISO 8601 timestamp
   - git pull --rebase --autostash
   - git push
   - print the short commit hash on success
   - use set -euo pipefail and be safe to run twice in a row
2. Make it executable and run it once. Show me the output.
3. Install a system crontab entry that runs it every 15 minutes and appends both stdout
   and stderr to /var/log/push-brain.log.
4. Show me the crontab afterwards.

Use the system crontab, not Hermes cron. This script has no model in it and does not need
one.
```

### Host guidance

Point at the last line of the brief. **A job with no reasoning in it should not cost tokens
or depend on the agent being healthy.** Hermes cron with delivery arrives in the next
module, where there is something worth saying.

On the laptop side, install the Obsidian **Git** community plugin and turn on pull on vault
open. That is the whole client side. The answer to "where is the research the server did"
becomes "you already have it, you opened Obsidian."

### The proof, on camera

Message the server agent. Ask it to write a note. Switch to the laptop, open Obsidian, wait
for the pull, read the note. **That is the deliverable and it is what the last five minutes
are protected for.**

---

## 0:57-1:00 · Close (3 min)

| | |
|---|---|
| What today added | A machine that does not close, holding the same brain and the same skills |
| What it cost | About 6 USD a month, plus 1.20 for backups. It bills until you destroy it |
| What is now permanent | The vault has history. Nothing the agent does to a note is unrecoverable |

**The honest caveat, said plainly:**

> "You now have two agents, and they do not share a mind. They share a folder. Sessions and
> memory stay on the machine that made them, and that is deliberate. The Hermes docs are
> explicit that two agents pointed at one home compound each other's entries into state
> neither of them authored. Sync the notes. Never sync the state."

The four things that must never sync, and why:

| File | What happens if you sync it |
|---|---|
| `memories/MEMORY.md` | Two writers compound entries neither of them authored. The `.lock` file beside it is local only and protects nothing across machines |
| `state.db` | SQLite with a live write ahead log. A mid write copy is corrupt, not stale |
| `sessions/` | Per machine history, and the same concurrent write problem |
| `.env`, `auth.json` | Different machines, different keys. A synced `.env` is one `git add -A` away from being public |

---

# Homework

1. Destroy and rebuild the Droplet from scratch, timed. If it takes more than 20 minutes,
   your notes are not good enough yet.
2. Have the server agent research something overnight and write it to `Research/`. Read it
   on the laptop the next morning without touching the server.
3. Break the sync deliberately: edit the same note on both machines, then resolve the
   conflict. Write down what you did.
4. Check your Droplet's memory use with `docker stats`. Report whether 1 GB was the right
   call.
5. Set a billing alert on your DigitalOcean account.
6. Write one paragraph in `HERMES.md` naming your write lanes, and commit it.

# Peer quiz

Five multiple choice on git ordering, project skills, the write sandbox, credential scope
and what must not sync. Four open ended, peer scored. One peer exercise where students clone
each other's public ignore list and audit it. Detail in `quiz.md`.

# Prep checklist

## Must verify before the stream

- [ ] **DigitalOcean new account credit.** Listings on 2026-08-23 say 200 USD for 60 days,
      but this was not confirmed against a DigitalOcean owned page. Check the signup page
      the morning of the stream and either state the figure or do not mention it
- [ ] Droplet price for the 1 GB Basic tier, read off the create page on the day
- [ ] The Docker Marketplace image's current Ubuntu version
- [ ] The container's UID, so the `chown` step is not a surprise
- [ ] That `docker exec -it hermes hermes skills trust /opt/brain` accepts a path argument
      in the current image, rather than needing to be run from inside the directory
- [ ] Whether the Obsidian Git plugin recovers cleanly from a rebase conflict, or strands a
      non technical user. **This decides whether the laptop side is teachable as written**

## Assets

- [ ] A Droplet built and destroyed twice in rehearsal, with the commands in a text file
- [ ] A second Droplet already built and hardened, in reserve, if the live one stalls
- [ ] A GitHub fine grained PAT created on camera, with the repository picker visible
- [ ] The host's own vault measurements on a slide, since students will want the comparison
- [ ] A pre written `HERMES.md` write lanes paragraph to paste if the segment runs long

# Open questions

1. **Does the Droplet get created live, or pre created and hardened live?** Creating live
   puts a billing page on stream. Recommend creating live with the browser cropped, since
   the create page is where the price is read.
2. **Is the git segment too slow for people who have never used git?** Modules 2 and 3
   assumed git without teaching it. If chat says otherwise, the ignore list becomes a
   pre written file in the student guide and the segment drops to 5 minutes.
3. **Should the vault repository be private or public?** Private is the correct default and
   the module says so. A public vault is a better teaching artifact and a worse habit.
   Recommend private, and a separate public repository for anyone who wants to share.
4. **6 USD a month as an attendance barrier.** Higher friction than Module 3's 2 USD,
   because it recurs. Recommend stating it in the Luma description and showing how to
   destroy the Droplet at the end of the session.

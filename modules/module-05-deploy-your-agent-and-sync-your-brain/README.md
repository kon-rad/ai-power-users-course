# Module 5: Deploy Your Agent and Sync Your Brain

**Module 5 of AI Power Users · 60 minutes · live on YouTube · macOS locally, Ubuntu on the server**
**From Argo, myargoquest.com**

Every module so far ran on your laptop. Close the lid and the agent stops existing. This
session moves it to a server that does not close, and keeps the notes and the skills
identical on both machines.

This module has a recurring cost. About 6 USD a month for the server, and it bills until
you destroy it.

## What you build

```
PART 1: the brain becomes a repository
    measure it   ──  16 GB, of which 4.7 MB matters
    ignore first ──  git init  ──  private repo
    skills       ──  .hermes/skills/  ──  hermes skills trust

PART 2: the server
    Droplet  ──  harden  ──  Hermes in Docker  ──  clone the brain
                                    │
                         push-brain.sh + crontab
                                    │
              NOTE WRITTEN ON THE SERVER, READ IN OBSIDIAN
```

## Learning objectives

By the end of this module you can:

1. Measure what a vault actually contains, and separate the part that has to sync from the
   part that does not
2. Initialise a git repository with the ignore list correct before the first commit, and
   say why that order cannot be reversed later
3. Move a skill into a repository so it loads as a project skill, and confirm the agent
   picked it up
4. Create a Droplet and harden it in an order that does not lock you out
5. Run Hermes in Docker against a persistent data volume, and extend the write sandbox to a
   second directory
6. Pick a deploy credential scoped to one repository, and say what it can and cannot do
7. Schedule a push job that stays silent unless it fails
8. Name the files that must never sync between two agents, and the failure each one causes

## The eight steps

| # | Step | Output |
|---|---|---|
| 1 | Measure your own vault | Two numbers: total size, markdown size |
| 2 | **Git, ignore list first** | A private repository under 50 MB |
| 3 | Skills move into the repo | `/skills` shows them tagged as project skills |
| 4 | Create the Droplet | A 1 GB box from the Docker Marketplace image |
| 5 | Harden it | Non root user, two firewalls, fail2ban, unattended upgrades |
| 6 | Run Hermes in Docker | A container with a persistent volume and no open ports |
| 7 | Clone the brain | The vault on the server, skills loading |
| 8 | **Close the loop** | **A note written on the server, read in Obsidian** |

## The ideas that carry the module

**Measure before you design.** The vault is 16 GB. The markdown in it is 4.7 MB. Almost
everyone reaches for block storage or a sync daemon to solve a problem that turns out to be
three orders of magnitude smaller than it looked.

**The order of the first commit is permanent.** Git tracks a file from the moment it is
first committed. Ignore first, then init, then stage, then read the size, then commit.
Getting this wrong cannot be revoked, only rewritten.

**Skills travel in the repository, not through an install step.** Hermes loads skills from
a trusted git checkout at the highest precedence tier, and its own curator is forbidden from
modifying them. Deploying a skill becomes `git push` and `git pull`.

**Scope the credential to the job.** The server needs to change one repository. A token that
does exactly that, and expires, is a smaller thing to lose than a key that opens a shell.

**Sync the artifacts, never the state.** Two agents share a folder, not a mind. Sessions and
memory stay on the machine that made them, because two writers on one store compound entries
neither of them authored.

**Silence is a feature.** A sync job that reports every successful run gets muted within a
day, and a muted monitor is not a monitor.

**The SSH backend moves commands, not content.** Hermes can run the laptop agent's
commands on the droplet directly, over the same hardened connection from Step 5. The
vault still syncs by git, on purpose: two things writing to one folder at once is the
failure the honest caveat already warns about.

## Files in this module

- [`syllabus.md`](./syllabus.md), the full plan: every segment, every brief, host guidance
- [`agenda.md`](./agenda.md), the 60 minute run of show and the contingency
- [`student-guide.md`](./student-guide.md), **follow along here**: every command and brief
- [`quiz.md`](./quiz.md), 5 multiple choice plus 4 open ended, peer evaluated
- [`luma-description.md`](./luma-description.md), event copy

## Before the session

**You need a working agent and a vault.** If yours is broken, the
[Module 3 student guide](../module-03-media-models-and-your-own-domain/student-guide.md)
gets you back to one.

| Account | Why | Cost |
|---|---|---|
| [GitHub](https://github.com) | Holds the vault repository | 0 USD, private repos are free |
| [DigitalOcean](https://www.digitalocean.com) | The server | About 6 USD a month, recurring |

Bring an SSH key on your laptop. Check with `ls ~/.ssh/*.pub`, and run
`ssh-keygen -t ed25519` if nothing comes back.

## Homework

1. Destroy and rebuild the Droplet from scratch, timed. Over 20 minutes means your notes are
   not good enough yet.
2. Have the server research something overnight and read it on the laptop the next morning
   without touching the server.
3. Edit the same note on both machines, cause a conflict, and resolve it.
4. Run `docker stats` and say whether 1 GB was the right size.
5. Set a billing alert on your DigitalOcean account.
6. Write your write lanes into `HERMES.md` and commit it.
7. Optional: set `terminal.backend: ssh` with a dedicated key, run one command on the
   droplet from your laptop agent, then switch back to `local`.

## The honest caveat

You now have two agents that share a folder, not a mind. The server does not know what you
told the laptop this morning, and it will not, because syncing memory between two running
agents corrupts it. Anything that has to cross machines has to be written down as a note.
That constraint is the price of the machine that never closes, and it is worth paying, but
nobody should discover it at minute forty.

# Module 5: Agenda (Run of Show)

**Module 5 of AI Power Users · 60 minutes · no break · live on YouTube**
**Platform: macOS locally, Ubuntu on the server.** Theme: the agent moves to a machine that
does not close, and the brain stays identical on both.

> Host note: the two time sinks are the git segment, which expands for anyone who has never
> used git, and the hardening, where one wrong command ends the session.
> **Protect 0:50 to 0:57 for the sync loop.** A note written on the server and read on the
> laptop is the deliverable. If the clock slips, cut from the git segment and paste the
> pre written ignore list.

## Part 1: The brain becomes a repository (26 min)

| Time | Segment | What happens | Attendee action |
|---|---|---|---|
| 0:00-0:03 | **Open with the payoff** | Obsidian and a server terminal side by side. Brief the server, the note appears on the laptop. Say the cost out loud: about 6 USD a month, recurring | Watch |
| 0:03-0:08 | **Measure your own vault** | Four commands. The host's vault is 16 GB, and the markdown in it is 4.7 MB. Read the ratio out | Run the four commands, post two numbers in chat |
| 0:08-0:16 | **Git, in the order that matters** | Ignore list first, `git init`, stage, read the size, then commit. Nested repos named and left alone. Private repository | Paste the brief, read the staged size |
| 0:16-0:26 | **Skills move into the repo** | Own skills into `.hermes/skills/`, `hermes skills trust`, then `/skills` proves it. Precedence and the curator exclusion | Paste the brief, confirm the tag |

## Part 2: The server (34 min)

| Time | Segment | What happens | Attendee action |
|---|---|---|---|
| 0:26-0:34 | **Create and harden the Droplet** | Docker Marketplace image, 1 GB, SSH key, backups. Non root user, Cloud Firewall, **second session confirmed before locking root out**, UFW, fail2ban, unattended upgrades | Create theirs, follow the order exactly |
| 0:34-0:42 | **Hermes in Docker** | `setup`, then `gateway run` with no `-p` and an extended write safe root. The three config changes for an unattended box. Two line chat on camera | Run both containers, edit config |
| 0:42-0:50 | **Clone the brain, trust the repo** | Fine grained PAT created on camera with the repository picker visible. `chown`, clone, trust. The confirmation brief, including the write that may fail | Create the token, clone, run the brief |
| 0:50-0:57 | **Close the loop** | Write lanes into `HERMES.md`. `push-brain.sh` and a system crontab. Obsidian Git plugin on the laptop. **The proof: note written on the server, read in Obsidian** | Paste the brief, install the plugin, watch the note arrive |
| 0:57-1:00 | **Close** | What it cost and how to stop the bill. The four things that must never sync. Homework | Note the homework |

## Materials on screen

- Student guide, pinned in chat
- cloud.digitalocean.com, the Droplet create page and the Firewalls page
- github.com, the new private repository page and the fine grained token page
- A terminal with two SSH sessions visible at once during hardening
- Obsidian on the laptop, vault open, Git plugin installed

## Homework (briefed at 0:57)

1. Destroy and rebuild the Droplet, timed. Over 20 minutes means the notes are not good
   enough yet.
2. Overnight research on the server, read on the laptop the next morning.
3. Cause a conflict on purpose and resolve it.
4. `docker stats`, and say whether 1 GB was right.
5. Set a billing alert.
6. Write the vault's write lanes into `HERMES.md`.

## Contingency

- **At 0:16 and behind:** stop writing the ignore list live. Paste the one in the student
  guide, say what each block excludes in a single sentence, and move on.
- **If the Droplet create page stalls on billing or verification:** switch to the reserve
  Droplet built in rehearsal, and narrate the create page from a screenshot. The hardening
  is the teaching, not the checkout.
- **If a hardening step locks the host out:** go to the DigitalOcean web console on camera
  and fix it there. Say out loud that browser consoles corrupt `:` and `@` characters, so
  type rather than paste. A visible recovery is good teaching.
- **At 0:42 and behind:** cut the confirmation brief to steps 1 and 3 only. The write test
  matters, the `/skills` check can move to homework.
- **At 0:50 and behind:** paste `push-brain.sh` from the student guide instead of having
  the agent write it, and cut the crontab explanation to the line itself.
- **Never cut:** the ignore list before the first commit, the second SSH session before
  disabling root login, and the note arriving in Obsidian.

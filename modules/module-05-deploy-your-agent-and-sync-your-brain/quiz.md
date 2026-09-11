# Module 5: Quiz

Five multiple choice questions, then four open ended ones scored by a peer.
Bring your open ended answers to the peer lab.

---

## Part A: Multiple choice

**1. Why must `.gitignore` be written before the first commit and not after?**

- a) `git init` refuses to run if the file is missing
- b) Git tracks a file from its first commit, so ignoring it afterwards leaves it in
  history and in every clone
- c) The ignore file only applies to files created after it exists
- d) GitHub rejects pushes larger than 50 MB

**2. Your vault is 16 GB and the markdown in it is 4.7 MB. What does that tell you?**

- a) You need block storage on the server
- b) The vault is too big to sync and should stay local
- c) The part that has to sync is small, and the size problem is media and dependencies
  that do not need to move
- d) You should compress the vault before syncing

**3. Where does Hermes look for project skills?**

- a) `~/.hermes/skills/` only
- b) `<project-root>/.hermes/skills/`, where the project root is the nearest ancestor
  containing `.git`
- c) Any directory listed in `PATH`
- d) The directory the gateway was started from, regardless of git

**4. You mount your vault at `/opt/brain` in the official Hermes Docker image and the agent
cannot write notes. Why?**

- a) The container has no network access
- b) The volume was mounted read only
- c) The image sets `HERMES_WRITE_SAFE_ROOT=/opt/data`, so writes outside it are blocked
- d) `write_file` requires an approval that never arrives

**5. Which of these should never be synced between your laptop and your server?**

- a) `Research/notes.md`
- b) `.hermes/skills/my-skill/SKILL.md`
- c) `.gitignore`
- d) `~/.hermes/state.db`

---

## Part B: Open ended (peer scored)

Answer in your own words. Two to five sentences each.

**6. Post your vault's total size and its markdown size. What would have gone wrong if you
had synced the whole directory?**

*Looking for: their two real numbers and the ratio between them. Then a concrete
consequence, not a vague one: a 25 GB Droplet disk filling, transfer charges, a first
commit that takes twenty minutes, or a repository that can never be made small again. The
point is that the design follows the measurement.*

**7. You gave your server a credential so it could push to GitHub. What exactly can that
credential do, and what can it not do?**

*Looking for: one repository, contents write, with an expiry date. And the comparison: an
SSH key with shell access would let a compromised server do anything that key reaches,
whereas this one can only change one repository's files. Bonus for naming the expiry as the
feature rather than the annoyance.*

**8. Why does the push job use the system crontab instead of Hermes cron?**

*Looking for: there is no reasoning in the job. It stages, commits and pushes, and none of
that needs a model. Using the agent would cost tokens, add latency, and make the sync depend
on the gateway being healthy. Good answers also note that the next module uses Hermes cron
where there is something worth saying.*

**9. Your laptop agent and your server agent share a folder but not a mind. Name one thing
that gets worse because of that, and one thing that gets safer.**

*Looking for: worse means the server does not know what you told the laptop this morning,
so context has to be repeated or written into a note. Safer means neither can corrupt the
other's memory, and the documented failure of two writers on one home is avoided. A strong
answer names the shared folder as the deliberate seam.*

---

## Peer exercise

Swap `.gitignore` files with another student and audit theirs.

1. Run their ignore list against your own vault: copy it in, run `git add -A --dry-run`,
   and report the staged size.
2. Name one thing they ignore that they should not, and one thing they do not ignore that
   they should.
3. Check whether `.env` appears in their list. If it does not, tell them immediately.

Score their setup 1 to 5 on: the staged size is small · secrets are excluded · media is
excluded · the vault still contains everything that matters · you could clone it on a fresh
machine and get a working agent.

---

## Answer key (Part A)

1. **b** Git tracks from the first commit. Ignoring afterwards does not remove it from
   history, and undoing it means rewriting history.
2. **c** The sync payload is the markdown. The bulk is media and `node_modules`, and
   neither needs to be on the server.
3. **b** The nearest ancestor with `.git`, and only after `hermes skills trust`.
4. **c** The official image sandboxes writes to the data volume. Extend the variable to
   include the second mount.
5. **d** `state.db` is SQLite with a live write ahead log. A mid write copy is corrupt.

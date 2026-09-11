# Module 7: Quiz

Five multiple choice questions, then four open ended ones scored by a peer.
Bring your open ended answers to the peer lab.

---

## Part A: Multiple choice

**1. Your vault is 19 GB. The markdown in it is 5 MB. Roughly 3 GB is `node_modules` spread
across a thousand directories. Where should `node_modules` go?**

- a) Backblaze B2, because it is large
- b) GitHub, because it is code
- c) Nowhere. It is rebuildable, and `npm install` is faster than a restore
- d) A separate bucket with a shorter lifecycle rule

**2. Why does `.rclone-filter` exclude `*.md` when markdown is the most valuable thing in the
vault?**

- a) Markdown is too small to be worth uploading
- b) Because git already owns those files, and two sync systems owning one file means one
  can quietly overwrite the other
- c) Because B2 cannot store plain text efficiently
- d) It is a mistake, and markdown should be in both

**3. You run `rclone bisync` with `--resync` on every scheduled run. What breaks?**

- a) Nothing, it is just slower
- b) The lock file never expires
- c) Deleted files reappear at the end of every run, because resync copies rather than syncs
- d) The filter file hash changes each time

**4. What does `--check-access` actually protect you from?**

- a) An unauthorised user reaching the bucket
- b) A key with the wrong capabilities
- c) One side being empty or unmounted, which bisync would otherwise read as "every file was
  deleted"
- d) Two machines syncing at the same time

**5. You put your Backblaze account ID in rclone's `account` field along with a bucket
restricted application key. What happens?**

- a) It works, but only for reads
- b) B2 returns 401 and the error does not explain why. The `account` field needs the keyID
- c) rclone falls back to the master key
- d) It works until the key expires

---

## Part B: Open ended (peer scored)

Answer in your own words. Two to five sentences each.

**6. Your teammate says "just bisync the whole vault to B2, filters are overcomplicating it."
What is your answer?**

*Looking for: three problems, and naming any two is a good answer. `node_modules` and build
output are rebuildable, so syncing them costs upload time and restore time for nothing.
Markdown is already owned by git, so syncing it twice creates a silent overwrite path. And
`.env` and `auth.json` would end up in a bucket, which is the Module 3 lesson again. Strong
answers point out that the filter file and `.gitignore` are complements, so there is still
only one place the split is defined.*

**7. Your server key is read only. Explain what that costs you and why you took the deal
anyway.**

*Looking for: the cost is real. A read only server cannot upload anything it generates, so
media produced on the server has to come back some other way. The reason to take it: the box
runs unattended, on the internet, with an agent that executes commands, and it has never
needed to delete the archive. Good answers name the escape hatch, which is a third key scoped
write to one name prefix rather than upgrading this one.*

**8. Your B2 storage number keeps climbing even though your vault has not grown. What is
happening, and what do you run?**

*Looking for: B2 keeps every version by default and hides deleted files rather than removing
them, so overwrites accumulate. Diagnose with `rclone size` against
`rclone size --b2-versions`, which return two different numbers. Fix with
`rclone backend lifecycle` setting `daysFromHidingToDeleting`, plus `rclone cleanup` for
interrupted uploads. A good answer names the trade off: the lifecycle window is how many days
of undo you keep.*

**9. A cron job runs your sync every 30 minutes. One night bisync fails and says it requires
`--resync`. Why must the script not add that flag itself?**

*Looking for: bisync asks for a resync when it can no longer tell what changed on each side.
That is the tool asking for a human. Resync is a copy in one direction with path1 as the
winner, so a script that answers automatically will flatten the other side without an error.
Correct behaviour is to exit non zero, alert, and have a person run a `--dry-run --resync`
and read it first.*

---

## Peer exercise

Swap `.gitignore` and `.rclone-filter` with another student and audit them as a pair.

1. Find one file type carried by **both** files. That is an overwrite waiting to happen.
2. Find one file type carried by **neither**. That is a file with one copy.
3. Check that `.env`, `.env.*` and `auth.json` are excluded from the rclone filter, not just
   from git.
4. Ask them what their B2 key can do, and see whether they can answer without opening the
   console.

Score their setup 1 to 5 on: the two files are complements with no overlap and no gap ·
secrets are excluded from both · the server key is read only · `RCLONE_TEST` exists on both
sides · they have a lifecycle rule set.

---

## Answer key (Part A)

1. **c** Rebuildable is a third category. It is neither history nor durability.
2. **b** Never let two sync systems own the same file. The overwrite is silent.
3. **c** Resync copies rather than syncs, so deletions never propagate.
4. **c** It is the guard against an empty or unmounted side being read as a mass deletion.
5. **b** The keyID goes in `account`. A bare 401 is what you get otherwise.

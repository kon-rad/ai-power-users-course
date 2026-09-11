# Module 3.1: Agenda (Run of Show)

**Module 3.1 of AI Power Users · 60 minutes · no break · live on YouTube**
**Platform: macOS.** Theme: put the vault on Sia and prove the backup by actually
restoring from it.

The two time sinks: waiting on the browser upload in Part 2 to actually distribute across
hosts, and a slow live `npm install` in Part 3. **Protect 0:50 to 0:58, the disaster
restore.** If the clock slips, cut earlier, not that segment.

## Part 1: What Sia actually is (12 min)

| Time | Segment | What happens | Attendee action |
|---|---|---|---|
| 0:00 to 0:03 | **Open** | State the artifact for today | Watch |
| 0:03 to 0:08 | **Measure the vault** | `du -sh` against the 50 GB free tier | Run it on their own vault |
| 0:08 to 0:15 | **The mechanism** | Encrypt, split into shards, spread across independent hosts | Watch, ask questions |

## Part 2: Create the account, prove it works (12 min)

| Time | Segment | What happens | Attendee action |
|---|---|---|---|
| 0:15 to 0:20 | **Sign up** | sia.storage, no card, no wallet | Create their own account |
| 0:20 to 0:27 | **Upload by hand** | One file, dragged into app.sia.storage | Upload their own |

## Part 3: Install the SDK, read its README (8 min)

| Time | Segment | What happens | Attendee action |
|---|---|---|---|
| 0:27 to 0:31 | **Install** | `npm install @siafoundation/sia-storage` | Install in their own project |
| 0:31 to 0:35 | **Read the README** | The connect, upload, download calls, current today | Read it themselves, not paraphrased |

## Part 4: Build and run `/vault-backup` (15 min)

| Time | Segment | What happens | Attendee action |
|---|---|---|---|
| 0:35 to 0:45 | **The brief** | Agent builds `/vault-backup`, backup and restore, from the README only | Paste the brief into their own agent |
| 0:45 to 0:50 | **Run it** | An object id comes back, logged to `backup-log.md` | Run their own backup |

## Part 5: Disaster restore (8 min)

| Time | Segment | What happens | Attendee action |
|---|---|---|---|
| 0:50 to 0:58 | **The restore** | Move the vault aside, restore from Sia, diff against the original | Do the same on their own machine |
| 0:58 to 1:00 | **Review and close** | State what is permanent, state the honest caveat | Watch |

## Materials on screen

- Student guide, pinned in chat
- A terminal showing `du -sh ~/Documents/secondbrain` at the top of the stream
- app.sia.storage, open, showing the one file uploaded by hand in Part 2
- `node_modules/@siafoundation/sia-storage/README.md`, open, during Part 3 and Part 4

## Homework (briefed at 0:58)

1. Schedule `/vault-backup` with cron or launchd.
2. Reread the SDK's README before the next stream, note what changed since 0.0.14.
3. Restore from an older object id specifically, not just the latest one.
4. Measure the vault again in a week.

## Contingency

- **At 0:20 and live signup stalls:** switch to the prep checklist's pre made account for
  the demo, have that student sign up as homework.
- **At 0:27 and the live `npm install` is slow:** use the cached README from the prep
  checklist to keep Part 3 moving, install for real once the stream continues.
- **At 0:45 and the agent's generated skill does not match the brief cleanly:** show the
  working reference version from the prep checklist's test vault, keep going, assign the
  student's own working version as homework.
- **Never cut:** the vault measurement at 0:03, the README read at 0:31, and the disaster
  restore at 0:50. Those three are what make this session a backup and not a demo.

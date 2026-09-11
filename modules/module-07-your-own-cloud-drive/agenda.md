# Module 7: Agenda (Run of Show)

**Module 7 of AI Power Users · 60 minutes · no break · live on YouTube**
**Platform: macOS on the laptop, the Ubuntu server from Module 5 on the other side.** Theme:
the 18.99 GB that git was never going to carry.

> Host note: the two time sinks are the first upload, which will not finish inside the
> session, and any student whose Module 5 repository is not clean.
> **Protect 0:28 to 0:36 for `--check-access`.** A sync that refuses to run when one side is
> missing is the deliverable. If the clock slips, cut the versions segment, not the guard.

## Part 1: The split (28 min)

| Time | Segment | What happens | Attendee action |
|---|---|---|---|
| 0:00-0:03 | **Open with the payoff** | Ask the server agent for a video it does not have. It fails. Run one command on the laptop, ask again, it fetches from object storage and answers | Watch |
| 0:03-0:08 | **Measure three categories** | Five commands on their own vault. Markdown, media, `node_modules`. The third row is why the disk is full | Run the commands, post numbers in chat |
| 0:08-0:13 | **Audit the repo** | `git count-objects`, tracked size, ten largest tracked files. Pass marks on screen. Committed video is a homework problem, not a live one | Run the audit |
| 0:13-0:19 | **Bucket and two keys** | Private bucket, object lock off. Two application keys: laptop read write, server **read only**, both scoped to one bucket. Crop the key | Create bucket and both keys |
| 0:19-0:28 | **Filter file and first sync** | `rclone config`, then `.rclone-filter` beside `.gitignore` on screen. **`- *.md` is the load bearing line.** `--dry-run`, then `--resync` once | Configure, write the filter, start the sync |

## Part 2: The loop (32 min)

| Time | Segment | What happens | Attendee action |
|---|---|---|---|
| 0:28-0:36 | **The guard** | `--max-delete` is on at 50 percent and is not enough. Create `RCLONE_TEST` on both sides, then **rename it on camera and watch bisync refuse** | Create markers, break it, fix it |
| 0:36-0:44 | **The server side** | rclone on the Droplet with the read only key. Write `fetch-media` as a project skill on the laptop, push, pull, trust. Ask the server for a file only in B2 | Install, configure, paste the brief |
| 0:44-0:51 | **Versions and the bill** | `rclone size` versus `rclone size --b2-versions`. Two different numbers. Lifecycle rule, `rclone cleanup`, caps and alerts | Set the lifecycle rule |
| 0:51-0:57 | **Automate it** | `sync-brain.sh`, cron every 30 minutes, silent on success. **Never let a script add `--resync`.** Break it on purpose and watch the failure arrive | Paste the brief, then break it |
| 0:57-1:00 | **Close** | Where the copies actually are. Three for text, two for media, one for `node_modules`. The bisync caveat, read straight from rclone's manual | Note the homework |

## Materials on screen

- Student guide, pinned in chat
- `.gitignore` and `.rclone-filter` open side by side in one window
- A crop region preset for the moment Backblaze shows an application key
- A second bucket, already populated with the full vault tree, for every demo after 0:28
- A small file, under 5 MB, chosen in advance as the fetch demo target
- An SSH session on the Droplet in a second pane
- The three category table on a slide

## Homework (briefed at 0:57)

1. Restore one file from B2 and open it.
2. Delete a file, sync, then recover it from a hidden version.
3. Rename `RCLONE_TEST` and confirm the sync refuses.
4. Compare `rclone size` and `rclone size --b2-versions` in a week.
5. Find the largest committed file and decide whether to rewrite history.
6. Set a Backblaze cap and alert.
7. Add one directory to the filter file and run the resync it requires.

## Contingency

- **The first upload will not finish live.** Start it on camera at 0:24, leave it visible,
  and switch to the pre populated bucket for every later demo. Say that you switched.
- **At 0:19 and behind:** hand out `.rclone-filter` as a file in the student guide rather
  than writing it live, and spend the recovered three minutes on the `*.md` line.
- **At 0:44 and behind:** cut the versions segment to the two `rclone size` commands and the
  single lifecycle command. Move `rclone cleanup` and the caps to homework.
- **If a student's Module 5 repository is dirty or missing:** point at the Module 5 student
  guide in chat and keep moving. Do not debug one attendee's repository live.
- **If Backblaze's console has changed layout:** narrate what you are looking for rather than
  where it used to be. Private, object lock off, one bucket per key.
- **If the fetch demo at 0:36 fails:** show the error, check the read only key with
  `rclone ls`, and if it is not fixed in ninety seconds, run the fetch by hand and move the
  skill to homework.
- **Never cut:** the `*.md` line in the filter file, the `--dry-run` before the resync, and
  the renamed `RCLONE_TEST` that makes bisync refuse.

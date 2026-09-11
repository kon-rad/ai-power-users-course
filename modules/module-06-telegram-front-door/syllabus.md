# Module 6: The Telegram Front Door

**Status:** syllabus draft v1, planning phase.
**Date:** 2026-08-23
**Platform:** the work happens on the Ubuntu server from Module 5. Your phone is the client.
**Prereq:** Module 5 complete. A hardened Droplet running Hermes in Docker, with the vault
cloned to `/opt/brain`.
**Companions:** `student-guide.md` · `agenda.md` · `quiz.md` · `luma-description.md`

---

## Format

**One session, 60 minutes, no break, live on YouTube.**

| Part | Time | Theme |
|---|---|---|
| **Part 1** | 0:00-0:26 | The bot. Token, user ID, the allowlist, first message |
| **Part 2** | 0:26-1:00 | Making it safe and useful. Approvals, tiers, scheduled results, scripts |

**The spine:** Module 5 put the agent on a machine that does not close. It is still only
reachable by SSH from a laptop. Today the interface moves to the thing already in your
pocket.

**What is different about this module:** it costs nothing. Telegram is free and the server
is already paid for. The entire cost of this module is the risk you take by opening a door,
which is why half the hour is spent on who is allowed through it.

---

## Learning objectives

By the end, a student can:

1. Create a Telegram bot and find their own numeric user ID
2. Configure the gateway allowlist, and say what the default deny posture protects against
3. Explain why the gateway needs no inbound ports, and verify that on their own firewall
4. Approve and deny a command from a phone, and say what happens if they never answer
5. Split admin commands from user commands so a second person can be added safely
6. Route scheduled job results to Telegram and keep them silent when nothing changed
7. Send a message from a shell script with no agent and no gateway involved
8. Say what a bot token grants, and what to do the moment one leaks

---

# PART 1: The bot (26 min)

## 0:00-0:03 · Open with the payoff (3 min)

Phone mirrored on screen. Message the agent. It reads a note out of the vault on the server
and replies.

Then ask it to delete something. The approval prompt arrives on the phone. Deny it on
camera.

> "That is a server in another country asking my permission, on my phone, before it runs a
> command. Everything else today is about making sure it only ever asks me."

---

## 0:03-0:09 · The token and your user ID (6 min)

### The bot

Message [@BotFather](https://t.me/BotFather) and send `/newbot`. It asks for a display name,
then a username that has to end in `bot`. It returns a token.

**The token is a credential.** Anyone holding it controls the bot. It goes in `.env` on the
server, nowhere else, and never in a note, a commit or a screen share.

### Your user ID

Message [@userinfobot](https://t.me/userinfobot). It replies with a number.

**The number, not your @username.** The allowlist matches on the numeric ID, and a username
can be changed by whoever holds the account.

### Host guidance

Crop or blur the token when BotFather returns it. Show the shape of it, not the value. Then
say out loud that you will revoke it after the stream, and do it, so students see that
`/revoke` in BotFather is the answer to a leak.

---

## 0:09-0:16 · The allowlist, and what deny by default means (7 min)

### The check order

Hermes checks authorisation in this order, and stops at the first match:

| # | Check |
|---|---|
| 1 | Per platform allow all flag, for example `TELEGRAM_ALLOW_ALL_USERS=true` |
| 2 | DM pairing approved list |
| 3 | Platform allowlist, `TELEGRAM_ALLOWED_USERS` |
| 4 | Global allowlist, `GATEWAY_ALLOWED_USERS` |
| 5 | Global allow all, `GATEWAY_ALLOW_ALL_USERS=true` |
| 6 | **Deny** |

Verified against the Hermes security documentation, 2026-08-23.

Rung 6 is the default. With nothing configured, the bot answers nobody. Rungs 1 and 5 are
the two that turn the system off, and the Hermes documentation's own phrasing for them is
that they are not recommended for bots with terminal access.

### Say the sentence

> "Your agent can run commands on that server. You are about to let a chat app talk to it.
> The lock on that door is a number that identifies your Telegram account. That is a
> perfectly good lock, and it is the only one."

### The edit, done by hand

```bash
nano ~/.hermes/.env
```

```bash
TELEGRAM_BOT_TOKEN=<the token from BotFather>
TELEGRAM_ALLOWED_USERS=<your numeric id>
```

### Host guidance

**Ask the agent to make this edit and let it refuse on camera.** Hermes hard blocks
`write_file` and `patch` against any `.env`, with no approval prompt and no override from
chat. It is one of the few things the agent will not do for you, and watching it decline is
a better explanation of the protection than describing it.

Then edit it by hand and say why that block exists.

---

## 0:16-0:26 · Restart, and the first message (10 min)

### Restart

```bash
docker restart hermes
docker logs --tail 20 hermes
```

The container from Module 5 already runs `gateway run` with `--restart unless-stopped`, and
inside the official image the gateway is supervised, so a crash restarts within seconds.
**There is no service to install.** `hermes gateway install` exists for people running
Hermes without Docker and is not needed here.

### First contact

Message the bot. Then run `/whoami`, which reports your access tier.

### What the gateway actually is

One background process. It connects every configured platform, holds the sessions, runs the
cron jobs and handles delivery. Not one process per platform.

### No inbound ports

Telegram's default is long polling. The agent reaches out to Telegram, so nothing has to
reach in. Prove it:

```bash
sudo ufw status
```

Port 22 and nothing else. The firewall from Module 5 is untouched.

| Mode | Inbound port | When |
|---|---|---|
| **Long polling, the default** | **None** | Always, for this course |
| Webhook | 443, publicly reachable HTTPS | Only if the machine needs to idle between messages |

### Host guidance

The `ufw status` output is worth three seconds on screen. Students expect that adding a chat
interface means opening a port, and it is the opposite.

---

# PART 2: Making it safe and useful (34 min)

## 0:26-0:34 · The approval prompt (8 min)

Module 5 set `approvals.mode: manual` on this box. Now it pays off somewhere visible.

### The brief

```
I want to see the approval flow. Do these in order and stop after each one.

1. Read /opt/brain/Research/server-hello.md and tell me the first line.
2. Create a throwaway file at /opt/brain/Research/scratch-delete-me.md with one line in it.
3. Delete that file using a terminal command, not the file tools.

On step 3, wait for my answer before doing anything.
```

Step 1 needs no approval. Step 3 triggers one.

Approve step 3 from the phone. Then run the brief again and deny it, so the class sees both.

### The rules that matter

| Behaviour | Detail |
|---|---|
| Unanswered prompts | **Denied** after the timeout, 300 seconds by default |
| Where the answer comes from | The chat. Reply yes or no, or use `/approve` and `/deny` |
| What is never approvable | A hardline blocklist that survives every override flag |

### Host guidance

Walk away from the phone for a moment and let one prompt sit. Fail closed is the single most
reassuring property of the system and it is invisible unless you demonstrate it.

---

## 0:34-0:42 · Tiers and pairing (8 min)

### Two tiers

```yaml
# ~/.hermes/config.yaml
gateway:
  platforms:
    telegram:
      extra:
        allow_admin_from: ["<your numeric id>"]
        user_allowed_commands: [status, model]
```

Admins get every slash command. Everyone else gets only what is listed, plus `/help` and
`/whoami`. This is the shape you want the first time a client or a family member is in the
chat.

### DM pairing, instead of collecting ID numbers

An unknown user gets a one time code, rate limited and expiring after an hour. You approve
it from the server:

```bash
docker exec -it hermes hermes pairing list
docker exec -it hermes hermes pairing approve telegram <code>
docker exec -it hermes hermes pairing revoke telegram <user-id>
```

### Host guidance

Take one volunteer from chat, pair them live as a non admin, let them run `/status`, then
let them try a command they do not have, then revoke them. Ninety seconds, and it teaches
the tier system better than the table does.

Have a second Telegram account ready in case nobody volunteers.

---

## 0:42-0:50 · Scheduled results land in your pocket (8 min)

### The home channel

Cron output has to go somewhere. Send `/sethome` in the chat you want results in, or set it
directly:

```bash
TELEGRAM_HOME_CHANNEL=<chat id>
```

### The brief

```
Set up one scheduled job.

Every morning at 07:00 server time, look at what changed in /opt/brain in the last 24 hours
using git log, and send me a three line summary of what I actually worked on.

If nothing changed, reply with only [SILENT] and nothing else.

Deliver it to telegram. Set the job's working directory to /opt/brain so my project skills
are available to it. Show me the job definition when you are done.
```

### The two things in that brief that are not decoration

**`[SILENT]`** is a Hermes convention. Cron delivery treats it as the quiet marker and sends
nothing. Without it you get a message every single morning, including the mornings you did
nothing, and within a week you have muted the bot.

**The working directory** decides whether project skills load. Cron jobs inherit your
interactive trust decision but resolve the project root from their own working directory. A
job pointed somewhere else loads none of the skills you moved in Module 5.

### Host guidance

Say the silence rule as a principle, not a setting. **A monitor that reports every healthy
run is not a monitor, it is noise you will learn to ignore.**

---

## 0:50-0:57 · Scripts, with no agent involved (7 min)

Not everything that should message you needs a model.

```bash
docker exec hermes hermes send --to telegram "test from the server"
```

For bot token platforms this talks straight to the platform's REST endpoint. **No gateway
required, no model called, no tokens spent.** Exit codes are 0 for success, 1 for a delivery
failure, 2 for a usage error.

### The brief

```
Update /opt/brain/scripts/push-brain.sh from last session so it tells me when it breaks.

On failure only, send a Telegram message containing the hostname and the last line of the
error. On success, stay silent.

Do not use the agent for this. Use hermes send, and keep the script runnable with no
model configured.

Show me the diff, then make it fail on purpose so I can see the message arrive.
```

### Host guidance

"Make it fail on purpose" is the whole segment. Anyone can write a notifier. The habit worth
teaching is proving the alarm works before you rely on it.

---

## 0:57-1:00 · Close (3 min)

| | |
|---|---|
| What today added | The agent is reachable from your pocket, and asks before it acts |
| What it cost | Nothing. Telegram is free and the server was already running |
| What is now permanent | Every future scheduled job has somewhere to report to |

**The honest caveat, said plainly:**

> "The lock on this door is a number that identifies a Telegram account. If your token
> leaks, revoke it in BotFather immediately, because whoever has it can talk to your bot.
> If your Telegram account is taken over, the person holding it is on the allowlist. This
> is a good setup, it is not a vault, and knowing which is which is the job."

---

# Homework

1. Add one scheduled job that reports something you actually check manually today, and make
   it silent when nothing changed.
2. Pair a second person as a non admin, then revoke them. Write down what they could and
   could not do.
3. Deliberately let an approval prompt time out. Confirm it denied.
4. Wire `hermes send` into one script you already run.
5. Ask your agent for a file from the vault and have it arrive as a real attachment.
6. Revoke your bot token in BotFather and set up a new one. Time it.

# Peer quiz

Five multiple choice on the check order, ports, the timeout behaviour, the silence
convention and what the token grants. Four open ended, peer scored. One peer exercise where
students try to talk to each other's bots. Detail in `quiz.md`.

# Prep checklist

## Must verify before the stream

- [ ] **The `.env` refusal.** Confirm the current image actually blocks the agent from
      editing `~/.hermes/.env`. The whole segment at 0:09 depends on it refusing on camera
- [ ] Whether pairing codes are delivered to the unknown user in chat, or have to be read
      off the server and passed out of band. This changes how the live pairing demo runs
- [ ] `hermes send` working from inside the Docker container with `docker exec`
- [ ] That `--deliver telegram` and `[SILENT]` behave as documented on the current image
- [ ] The `/whoami` output format, so the tier is legible on a phone screen at stream
      resolution
- [ ] Whether a cron job's `workdir` can be set from the chat brief, or needs a config edit

## Assets

- [ ] A phone mirrored to the streaming machine, tested at stream resolution
- [ ] A second Telegram account for the pairing demo, in case chat produces no volunteer
- [ ] A bot token created and revoked in rehearsal, so the revoke flow is familiar
- [ ] A crop or blur region preset for the moment BotFather returns the token
- [ ] The Module 5 Droplet in a known good state, with `/opt/brain` populated

# Open questions

1. **Does the module use the host's real Droplet or a throwaway?** A throwaway is safer on
   camera. The real one is better television because the vault has real notes in it.
   Recommend the real one with a cropped window.
2. **Is the pairing demo worth the risk of nobody volunteering?** It is the best segment if
   it lands and dead air if it does not. Recommend keeping it with the second account ready.
3. **Should groups be taught at all?** Privacy mode requires removing and re-adding the bot,
   which is a live time sink. Recommend naming it and moving it to a later module.
4. **Voice messages.** Incoming transcription needs either a local model on a 1 GB box or a
   second API key. Recommend leaving it out and saying why.

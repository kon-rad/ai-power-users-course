# Module 6: Student Guide

**The Telegram Front Door**
Follow along here. Every command, every brief, ready to copy.

By the end your agent answers on your phone and asks permission before it acts.

**All commands run on the Ubuntu server from Module 5.**

---

## Before the session

### Check your server still works

```bash
ssh hermes@<your-droplet-ip>
docker ps
```

You should see the `hermes` container running. If not, the
[Module 5 student guide](../module-05-deploy-your-agent-and-sync-your-brain/student-guide.md)
gets you back to it.

### Accounts

| Account | Why | Cost |
|---|---|---|
| [Telegram](https://telegram.org) | The interface | 0 USD |

Nothing else. The server is already paid for.

### Bring

- Telegram installed on your phone and signed in.
- Your Droplet IP and SSH access, tested before the session starts.

---

## Step 1: Create the bot

In Telegram, message [@BotFather](https://t.me/BotFather):

```
/newbot
```

Give it a display name, then a username ending in `bot`. It returns a token.

> **The token is a credential.** Anyone who has it controls your bot. It goes in `.env` on
> the server and nowhere else. If it leaks, send `/revoke` to BotFather immediately.

---

## Step 2: Find your user ID

Message [@userinfobot](https://t.me/userinfobot). It replies with a number.

> **The number, not your @username.** The allowlist matches the numeric ID.

---

## Step 3: Add both to the server

```bash
nano ~/.hermes/.env
```

Add two lines:

```bash
TELEGRAM_BOT_TOKEN=<the token from BotFather>
TELEGRAM_ALLOWED_USERS=<your numeric id>
```

Save with `Ctrl+O`, `Enter`, then `Ctrl+X`.

> **Your agent cannot do this step for you.** Hermes blocks all writes to any `.env` file,
> with no approval prompt and no way to override it from chat. Try asking it, and watch it
> refuse.

Never set `GATEWAY_ALLOW_ALL_USERS=true`. With no allowlist configured, the gateway denies
everyone, which is the correct default.

---

## Step 4: Restart and say hello

```bash
docker restart hermes
docker logs --tail 20 hermes
```

Message your bot in Telegram. Then send:

```
/whoami
```

That reports your access tier.

You do not need `hermes gateway install`. The container from Module 5 already restarts on
its own.

### Confirm no ports opened

```bash
sudo ufw status
```

Port 22 and nothing else. Telegram uses long polling, so the agent reaches out and nothing
has to reach in.

---

## Step 5: See the approval flow

Paste this into the Telegram chat:

```
I want to see the approval flow. Do these in order and stop after each one.

1. Read /opt/brain/Research/server-hello.md and tell me the first line.
2. Create a throwaway file at /opt/brain/Research/scratch-delete-me.md with one line in it.
3. Delete that file using a terminal command, not the file tools.

On step 3, wait for my answer before doing anything.
```

Step 3 sends an approval prompt. Reply yes.

Run it again and reply no.

Run it a third time and ignore it.

> **Unanswered prompts are denied**, after 300 seconds by default. Walking away never
> approves anything.

---

## Step 6: Add a second person safely

Edit `~/.hermes/config.yaml`:

```yaml
gateway:
  platforms:
    telegram:
      extra:
        allow_admin_from: ["<your numeric id>"]
        user_allowed_commands: [status, model]
```

Then `docker restart hermes`.

Admins get every command. Everyone else gets only the listed ones, plus `/help` and
`/whoami`.

To add someone without collecting their ID, have them message the bot and approve the code:

```bash
docker exec -it hermes hermes pairing list
docker exec -it hermes hermes pairing approve telegram <code>
docker exec -it hermes hermes pairing revoke telegram <user-id>
```

Codes expire after an hour.

---

## Step 7: Send scheduled results to your phone

In the chat you want results in, send:

```
/sethome
```

Then paste this:

```
Set up one scheduled job.

Every morning at 07:00 server time, look at what changed in /opt/brain in the last 24 hours
using git log, and send me a three line summary of what I actually worked on.

If nothing changed, reply with only [SILENT] and nothing else.

Deliver it to telegram. Set the job's working directory to /opt/brain so my project skills
are available to it. Show me the job definition when you are done.
```

> **Two things in that brief are load bearing.** `[SILENT]` stops the job messaging you on
> the mornings nothing happened. The working directory decides whether your project skills
> from Module 5 load at all.

---

## Step 8: Message yourself from a script

```bash
docker exec hermes hermes send --to telegram "test from the server"
```

No model is called and no gateway is needed for this. Exit codes are 0 for success, 1 for a
delivery failure, 2 for a usage error.

Now wire it into the sync job from Module 5:

```
Update /opt/brain/scripts/push-brain.sh from last session so it tells me when it breaks.

On failure only, send a Telegram message containing the hostname and the last line of the
error. On success, stay silent.

Do not use the agent for this. Use hermes send, and keep the script runnable with no
model configured.

Show me the diff, then make it fail on purpose so I can see the message arrive.
```

---

## If something breaks

| Symptom | Cause and fix |
|---|---|
| The bot never replies | Check `docker logs --tail 50 hermes`. Usually a wrong token or a container that was not restarted after the `.env` edit |
| `Unauthorized` | Your numeric ID is not in `TELEGRAM_ALLOWED_USERS`. Confirm it with @userinfobot, and check for a stray space around the `=` |
| The agent refuses to edit `.env` | Working as designed. Edit it yourself with `nano` |
| The cron job messages you every morning with nothing to say | `[SILENT]` is missing from the prompt |
| The scheduled job cannot find your skills | Its working directory is not inside `/opt/brain` |
| `hermes send` returns exit code 2 | A usage error in the command, not a delivery problem. Check the `--to` target |
| The token appeared on stream or in a commit | Send `/revoke` to BotFather now, create a new one, update `.env`, restart |

---

## Homework

1. Add one scheduled job that reports something you check manually today, silent when
   nothing changed.
2. Pair a second person as a non admin, then revoke them. Write down what they could and
   could not do.
3. Let an approval prompt time out on purpose. Confirm it denied.
4. Wire `hermes send` into one script you already run.
5. Ask your agent for a file from the vault and have it arrive as an attachment.
6. Revoke your bot token and set up a new one. Time how long it takes.

---

## What this cost

| Item | Amount |
|---|---|
| Telegram | 0 USD |
| Server | Already running from Module 5, about 6 USD a month |
| Model usage | Only what your scheduled jobs actually spend |

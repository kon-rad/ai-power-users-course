# Module 6: Quiz

Five multiple choice questions, then four open ended ones scored by a peer.
Bring your open ended answers to the peer lab.

---

## Part A: Multiple choice

**1. You install the gateway, add your bot token, and configure no allowlist at all. Who can
talk to your bot?**

- a) Anyone who finds the bot username
- b) Anyone in your Telegram contacts
- c) Nobody. The gateway denies every user by default
- d) Only people in the same group as the bot

**2. Adding Telegram to your server means opening which inbound port?**

- a) 443, for the webhook
- b) 8642, for the gateway API
- c) None. Long polling means the agent connects outward
- d) 80 and 443

**3. An approval prompt arrives on your phone and you never answer it. What happens?**

- a) It approves after the timeout so work is not blocked
- b) It is denied after the timeout
- c) It waits indefinitely until you reply
- d) It retries the command with reduced permissions

**4. Your morning cron job messages you every single day, including days you did nothing.
What is missing?**

- a) A lower `rate_limit` setting
- b) The `[SILENT]` instruction in the job's prompt
- c) `TELEGRAM_HOME_CHANNEL`
- d) `require_mention: true`

**5. You ask your agent to add the bot token to `~/.hermes/.env` and it refuses. Why?**

- a) It needs an approval you did not give
- b) The file does not exist yet
- c) Hermes hard blocks writes to any `.env`, with no approval prompt and no override from
  chat
- d) The container is running as the wrong user

---

## Part B: Open ended (peer scored)

Answer in your own words. Two to five sentences each.

**6. Your bot token appears in a screen recording you already published. Walk through what
you do, in order.**

*Looking for: revoke in BotFather first, before anything else, because the token is the
credential and revoking it makes the recording harmless. Then create a new one, update
`.env` on the server, restart the container, confirm the bot answers. Good answers note that
deleting the video is not the first step and does not help on its own.*

**7. Someone says "just set `GATEWAY_ALLOW_ALL_USERS=true`, it is easier." What is your
answer?**

*Looking for: the agent has terminal access to a server, so the allowlist is the only thing
standing between a stranger and a shell. The Hermes docs specifically warn against this flag
for bots with terminal access. A strong answer offers the alternative rather than just
refusing: DM pairing gets people in without hardcoding IDs and can be revoked.*

**8. Why does `hermes send` not need a model or a running gateway, and when would you use it
instead of asking the agent?**

*Looking for: for bot token platforms it posts straight to the platform's REST endpoint, so
there is nothing to reason about and nothing to spend. Use it whenever the message text is
already decided: deploy finished, disk full, the sync job failed. Asking the agent to send a
message you already wrote costs tokens and adds a dependency on the gateway being healthy.*

**9. Your scheduled job runs but cannot use the skills you moved into the vault in Module 5.
What is wrong, and how do you know?**

*Looking for: the job's working directory is not inside the trusted repository, so no project
skills load. Cron jobs inherit the interactive trust decision but resolve the project root
from their own working directory. How they know: run the job manually, check `/skills` from a
session started in the same directory, or look at the job definition's workdir.*

---

## Peer exercise

Swap bot usernames with another student and try to talk to each other's bots.

1. Message their bot. Report exactly what happens.
2. Ask them to pair you as a non admin. Run `/whoami`, then `/status`, then try a command
   you should not have.
3. Have them revoke you. Confirm you are locked out.

Score their setup 1 to 5 on: an unknown user is refused · pairing worked without them
reading your ID off a screen · your tier was actually restricted · revoke took effect
immediately · they could tell you what their bot is allowed to do on their server.

---

## Answer key (Part A)

1. **c** Deny is the bottom rung and the default. Nothing configured means nobody gets in.
2. **c** Long polling connects outward. The firewall from Module 5 stays as it was.
3. **b** Denied, after 300 seconds by default. It fails closed on purpose.
4. **b** `[SILENT]` is the quiet marker cron delivery looks for.
5. **c** Any `.env` is a protected path for the file tools. Edit it yourself.

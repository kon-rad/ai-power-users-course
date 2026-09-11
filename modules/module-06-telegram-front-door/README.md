# Module 6: The Telegram Front Door

**Module 6 of AI Power Users · 60 minutes · live on YouTube · runs on the Ubuntu server from Module 5**
**From Argo, myargoquest.com**

Module 5 put your agent on a server that does not close. It is still only reachable by SSH
from a laptop. This session moves the interface to your phone, and spends half the hour on
who is allowed through the door.

This module costs nothing. Telegram is free and the server is already paid for.

## What you build

```
PART 1: the bot
    BotFather  ──  token        ──┐
    @userinfobot ──  numeric id ──┤──▶  ~/.hermes/.env  ──  docker restart
                                  │
                            deny by default

PART 2: safe and useful
    approval on your phone  ──  approve, deny, and let one time out
    admin vs user tiers     ──  pair someone, revoke them
    scheduled job           ──  results in Telegram, silent when nothing changed
    hermes send             ──  scripts message you, no model involved
```

## Learning objectives

By the end of this module you can:

1. Create a Telegram bot and find your own numeric user ID
2. Configure the gateway allowlist, and say what the default deny posture protects against
3. Explain why the gateway needs no inbound ports, and verify that on your own firewall
4. Approve and deny a command from your phone, and say what happens if you never answer
5. Split admin commands from user commands so a second person can be added safely
6. Route scheduled results to Telegram and keep them silent when nothing changed
7. Send a message from a shell script with no agent and no gateway involved
8. Say what a bot token grants, and what to do the moment one leaks

## The eight steps

| # | Step | Output |
|---|---|---|
| 1 | Create the bot | A token from BotFather |
| 2 | Find your user ID | A number from @userinfobot |
| 3 | Add both to the server | Two lines in `.env`, edited by hand |
| 4 | Restart and say hello | The bot answers, and no port was opened |
| 5 | **See the approval flow** | **Approve one, deny one, let one time out** |
| 6 | Add a second person safely | Tiers set, someone paired and revoked |
| 7 | Scheduled results to your phone | A morning digest that stays quiet on quiet days |
| 8 | Message yourself from a script | The sync job from Module 5 reports its own failures |

## The ideas that carry the module

**Deny is the default, and that is the whole design.** With nothing configured the gateway
answers nobody. The one line that changes that is the one line worth getting right.

**The lock is a Telegram account.** Your agent can run commands on a server. The thing
standing between a stranger and that shell is a number identifying your Telegram account.
It is a good lock. It is the only one.

**Adding an interface did not open a port.** Long polling means the agent reaches out, so
the firewall stays exactly as Module 5 left it. Most people expect the opposite.

**Unanswered means denied.** Walking away from your desk never approves anything. The
system fails closed, and that is only reassuring if you have watched it happen.

**Silence is a feature.** A job that reports every healthy run is noise you will learn to
ignore, and an ignored monitor is not a monitor.

**Not everything that messages you needs a model.** When the text is already decided, send
it. Asking an agent to relay a sentence you wrote costs tokens and adds a dependency.

## Files in this module

- [`syllabus.md`](./syllabus.md), the full plan: every segment, every brief, host guidance
- [`agenda.md`](./agenda.md), the 60 minute run of show and the contingency
- [`student-guide.md`](./student-guide.md), **follow along here**: every command and brief
- [`quiz.md`](./quiz.md), 5 multiple choice plus 4 open ended, peer evaluated
- [`luma-description.md`](./luma-description.md), event copy

## Before the session

**You need Module 5's server.** A hardened Droplet running Hermes in Docker with the vault
at `/opt/brain`. If yours is not running, the
[Module 5 student guide](../module-05-deploy-your-agent-and-sync-your-brain/student-guide.md)
rebuilds it.

| Account | Why | Cost |
|---|---|---|
| [Telegram](https://telegram.org) | The interface | 0 USD |

Bring Telegram on your phone, signed in, and your SSH access tested before the session
starts.

## Homework

1. Add one scheduled job that reports something you check manually today, silent when
   nothing changed.
2. Pair a second person as a non admin, then revoke them. Write down what they could and
   could not do.
3. Let an approval prompt time out on purpose. Confirm it denied.
4. Wire `hermes send` into one script you already run.
5. Ask your agent for a file from the vault and have it arrive as an attachment.
6. Revoke your bot token and set up a new one. Time how long it takes.

## The honest caveat

If your bot token leaks, whoever holds it controls your bot until you revoke it. If your
Telegram account is taken over, the person holding it is already on the allowlist and the
approval prompts go to them. Manual approvals and a one person allowlist make this a
reasonable setup for your own server. They do not make it a vault, and anyone putting client
data behind it should know the difference before they do.

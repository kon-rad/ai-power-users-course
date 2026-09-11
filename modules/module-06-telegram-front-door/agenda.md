# Module 6: Agenda (Run of Show)

**Module 6 of AI Power Users · 60 minutes · no break · live on YouTube**
**Platform: the Ubuntu server from Module 5. The phone is the client.** Theme: the agent
becomes reachable from your pocket, and asks before it acts.

> Host note: the two time sinks are the pairing demo, which depends on a volunteer showing
> up, and any student whose Module 5 Droplet is not healthy.
> **Protect 0:26 to 0:34 for the approval flow.** An approval prompt answered from a phone
> is the deliverable. If the clock slips, cut the pairing demo, not the approvals.

## Part 1: The bot (26 min)

| Time | Segment | What happens | Attendee action |
|---|---|---|---|
| 0:00-0:03 | **Open with the payoff** | Phone mirrored. Message the agent, it reads a note off the server and replies. Then ask it to delete something and deny the prompt on camera | Watch |
| 0:03-0:09 | **Token and user ID** | BotFather `/newbot`, token cropped on screen. @userinfobot for the numeric ID. The token is a credential, and `/revoke` is the answer to a leak | Create their bot, get their ID |
| 0:09-0:16 | **The allowlist** | The six rung check order, deny at the bottom. **Ask the agent to edit `.env` and let it refuse on camera.** Then edit by hand | Add two lines with `nano` |
| 0:16-0:26 | **Restart and first message** | `docker restart`, logs, first message, `/whoami`. No service to install, the container already handles it. `ufw status` proves no port opened | Restart, message their bot |

## Part 2: Making it safe and useful (34 min)

| Time | Segment | What happens | Attendee action |
|---|---|---|---|
| 0:26-0:34 | **The approval prompt** | Run the brief three times: approve, deny, then **walk away from the phone and let one time out**. Fail closed, 300 seconds | Run it three times |
| 0:34-0:42 | **Tiers and pairing** | `allow_admin_from` and `user_allowed_commands`. Pair a volunteer from chat as a non admin, let them hit a command they do not have, revoke them | Set their tiers |
| 0:42-0:50 | **Scheduled results** | `/sethome`, then the morning digest job. `[SILENT]` so quiet days stay quiet. Working directory decides whether project skills load | Paste the brief |
| 0:50-0:57 | **Scripts, no agent** | `hermes send` straight to Telegram, no model, no gateway. Wire it into Module 5's push job, then **break it on purpose** so the alarm proves itself | Wire theirs, break it |
| 0:57-1:00 | **Close** | It cost nothing. The lock is a Telegram account. What to do the moment a token leaks. Homework | Note the homework |

## Materials on screen

- Student guide, pinned in chat
- A phone mirrored to the streaming machine, tested at stream resolution
- @BotFather and @userinfobot open in Telegram
- An SSH session on the Droplet with `docker logs -f hermes` running in a second pane
- A crop or blur region preset for the moment the token appears

## Homework (briefed at 0:57)

1. One scheduled job that reports something checked manually today, silent when nothing
   changed.
2. Pair someone as a non admin, then revoke them.
3. Let an approval prompt time out on purpose.
4. Wire `hermes send` into an existing script.
5. Get a file out of the vault as an attachment.
6. Revoke the bot token and set up a new one, timed.

## Contingency

- **If nobody volunteers for the pairing demo:** use the second Telegram account prepared in
  advance. Do not wait more than 20 seconds for chat.
- **At 0:34 and behind:** cut the pairing demo entirely and move it to homework. The tier
  config stays, since it is three lines of YAML on screen.
- **If a student's Module 5 Droplet is down:** point them at the Module 5 student guide in
  chat and keep moving. Do not debug one attendee's server live.
- **If the bot does not respond after the restart:** show `docker logs --tail 50 hermes` on
  camera and read the error. A visible diagnosis is worth more than a working demo, and the
  cause is almost always the token or a container that was not restarted.
- **At 0:50 and behind:** demonstrate `hermes send` as a one liner and move the push script
  edit to homework.
- **Never cut:** the agent refusing to edit `.env`, the approval that times out, and the
  `ufw status` check.

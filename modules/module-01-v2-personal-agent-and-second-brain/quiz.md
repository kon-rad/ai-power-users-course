# Module 1 V2: Quiz

Five multiple choice questions, then four open ended ones scored by a peer. Bring your open
ended answers to the peer lab.

---

## Part A: Multiple choice

**1. When you filter models on OpenRouter, why does tool support come before price?**

- a) Tool support makes the model cheaper
- b) Without it the agent can chat but cannot read or write a single file
- c) OpenRouter charges extra for models without tools
- d) It does not matter, price is the only real filter

**2. What does PARA sort your notes by?**

- a) Subject
- b) The date you created them
- c) How soon you have to act on them
- d) File size

**3. You wrote a skill and nothing happens when you type its slash command. Which of these
is the failure that produces no error at all?**

- a) The file is named `skill.md` instead of `SKILL.md`
- b) The skill name collides with one of Hermes' built in slash commands
- c) The description field is too long
- d) The model has no tool support

**4. You created an SSH key pair. Which file can you safely paste into a hosting provider's
web form?**

- a) `id_ed25519`
- b) `id_ed25519.pub`
- c) Both, they are interchangeable
- d) Neither, you paste the passphrase instead

**5. Your Telegram bot is running on your server. What does setting
`GATEWAY_ALLOW_ALL_USERS=true` do?**

- a) Speeds up replies by skipping the allowlist check
- b) Lets your contacts see the bot in search
- c) Lets anyone who finds your bot run commands on your server
- d) Allows the bot to join group chats

---

## Part B: Open ended (peer scored)

Answer in your own words. Two to five sentences each.

**6. Name the pieces of what you built today that are private and the pieces that are not,
and say why the not private ones are still a reasonable choice.**

*Looking for: Handy and the notes on disk are private. The model through OpenRouter is not,
because prompts pass through OpenRouter and the upstream provider. The server is theirs but
is a rented machine on the public internet. Telegram bot chats are not end to end encrypted.
The reason for accepting it is reliability, and the point is that they know which is which
rather than believing all of it is private. An answer that says "it is all private" has
missed the module.*

**7. What did one agent session cost you, and how did you work that out before you ran it?**

*Looking for: an actual figure from their own OpenRouter usage or the arithmetic from the
model page. The two prices, input and output, per million tokens. Roughly 50,000 in and
5,000 out for a session that reads a few notes and writes one, which lands near half a cent.
Knowing the cost afterwards is a receipt. Knowing it beforehand is the skill.*

**8. Paste the `description` line from your `/standup` skill. Who is it written for, and
what happens if it is vague?**

*Looking for: the description is how the agent decides whether the skill is relevant to what
was just asked. It is written for the agent, not for the human. A vague description gets a
skill that never fires, which reads as the skill being broken. A good answer names the
trigger conditions in the description rather than describing the output.*

**9. Your agent runs on a server now. Name one thing it can do that the laptop version
could not, and one thing you gave up to get there.**

*Looking for: it runs when the laptop is shut, so it can act on a schedule and be reached
from a phone. What was given up: the server does not have their vault, only an empty PARA
skeleton and one skill, so sync is an unsolved problem. Also `--skip-browser` on the 1 GB
plan means no web pages. Also a machine on the public internet with a terminal attached to a
chat app is a new thing that can go wrong. Any one of those is a real answer. "Nothing" is
not.*

---

## Peer exercise

Swap phones with a peer.

1. Send **their** bot a message from **your** phone. What happens?
2. Ask them to show you their `TELEGRAM_ALLOWED_USERS` line and their `.env` file location.
3. Ask them to run `/standup` and time it. Was it under two minutes of their attention?

Score their setup 1 to 5 on: the bot ignored you, a stranger, exactly as it should · the
token is in `.env` and not in a note or a screenshot · the allowlist is one number long ·
the standup skill asks one question at a time rather than batching all three · the agent
survived a reboot without them touching the server.

---

## Answer key (Part A)

1. **b** Without tool support the agent produces text and never touches a file. It looks
   broken and is not, which makes it the most confusing failure in the course.
2. **c** Actionability, not subject. "Learn Khmer" is an Area, "Pass the Khmer exam in
   March" is a Project.
3. **b** A shadowed name fails silently. The built in wins and your skill is never reached.
   A wrongly named file at least gives you nothing to argue with, but a name collision looks
   like the skill ran and did nothing.
4. **b** The `.pub` half is the public key and is meant to be pasted. The file with no
   extension is the private key and never leaves your machine.
5. **c** It disables the allowlist. On a bot with terminal access that is a stranger running
   commands on your server. Never set it true.

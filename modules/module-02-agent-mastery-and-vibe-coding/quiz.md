# Module 2 — Quiz

Five multiple-choice questions, then four open-ended ones scored by a peer.
Bring your open-ended answers to the peer lab.

---

## Part A — Multiple choice

**1. What is a token?**

- a) One character
- b) One word, always
- c) A chunk of text, roughly three-quarters of a word on average
- d) One sentence

**2. Which normally costs more per token?**

- a) Input — what you send
- b) Output — what the model writes back
- c) They always cost exactly the same
- d) Neither; you are billed per message

**3. What is the difference between a session and a task?**

- a) Nothing, they are two words for the same thing
- b) A session is one continuous conversation where context accumulates; a task is one unit of work
- c) A task is longer than a session
- d) A session is one message, a task is one reply

**4. What does `SOUL.md` hold that `HERMES.md` does not?**

- a) Project conventions and the tech stack for one job
- b) Your API keys
- c) Who your agent is everywhere — persona, voice, base behaviour
- d) The conversation history

**5. Why do you check for secrets before your first commit?**

- a) Git will refuse to commit a file containing a key
- b) A key pushed to a public repository is scraped within minutes
- c) It makes the repository smaller
- d) Vercel will not deploy without it

---

## Part B — Open ended (peer-scored)

Answer in your own words. Two to five sentences each.

**6. Why does one-task-per-session make the prompt-cache problem disappear rather than
just reduce it?**

*Looking for: switching models breaks the cache, so the next message re-reads the whole
conversation at full input price. At a reset there is no accumulated context, so there is
nothing to re-read and the penalty is zero — not smaller, gone.*

**7. Paste your `goals.md`. How did it help you say no to something?**

*Looking for: a specific moment where the agent proposed something plausible and the
stated goal was the reason to reject it. "It looked good but pushed the quote button below
the fold" is the shape of a strong answer.*

**8. Did your agent recommend a database? Was it right?**

*Looking for: engagement with the trade-off rather than a yes or no. Adding a database
adds something that breaks, costs money, and needs maintaining. Receiving quote requests
is email; tracking which ones closed is a database — and a second invoice.*

**9. Paste a skill brief you wrote yourself. Could a peer build the same skill from it
without asking you a question?**

*Looking for: a brief with a clear trigger description, concrete steps, named output
location, and stated failure behaviour. The description line matters most — it is the only
part the agent sees when deciding whether to fire.*

---

## Peer exercise

Open your peer's **live URL** on your phone.

1. Would you request a quote? Yes or no, and why.
2. Name one thing that works.
3. Name one thing that does not.

Score their site 1–5 on: loads fast on mobile · clear what the business does · obvious next
action · looks trustworthy · you could hand it to a real client.

---

## Answer key (Part A)

1. **c** — roughly three-quarters of a word. This is why models miscount letters.
2. **b** — output, typically three to five times input.
3. **b** — session is the conversation, task is the unit of work. Run one task per session.
4. **c** — `SOUL.md` is global identity; `HERMES.md` is per-project rules.
5. **b** — public repositories are scraped continuously. Rotate any key you leak.

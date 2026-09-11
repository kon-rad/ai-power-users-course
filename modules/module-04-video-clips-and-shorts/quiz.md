# Module 4: Quiz

Five multiple choice questions, then four open ended ones scored by a peer.

---

## Part A: Multiple choice

**1. `window.__argocutAgent` shows up as `undefined` in a tab that was open before you
restarted the server with the agent flag on. Why?**

- a) The flag needs to be set twice
- b) `NEXT_PUBLIC_` variables are baked into the JS bundle at build time, so that tab is
  running stale JS
- c) The tab needs to be closed and the browser reinstalled
- d) The flag only works in incognito mode

**2. In an `add_clip` op, `trimEnd` is:**

- a) The exact second the clip should stop playing
- b) The source video's total duration minus the plan's out point
- c) Always zero unless you are trimming the start
- d) Measured in frames, not seconds

**3. Why does the cut plan use sentence indices instead of raw seconds?**

- a) Sentence indices are faster to type
- b) So the clip can never start or end mid thought
- c) Whisper does not produce timestamps in seconds
- d) ArgoCut only accepts integer inputs

**4. What happens when one op in an `apply_edits` batch is refused?**

- a) That one op is skipped and the rest apply
- b) The whole batch rolls back, and the response names the failing index and the reason
- c) The batch retries automatically up to three times
- d) The project is deleted

**5. Why does the `fence-co` brand preset live inside the project folder instead of the
shared `argocut-edit` skill folder?**

- a) The skill folder is read only
- b) A client's colours do not belong in a file every other project's captions also read
  from
- c) It renders faster from the project folder
- d) It does not matter, either location works the same

---

## Part B: Open ended (peer scored)

Answer in your own words. Two to five sentences each.

**6. Your `apply_edits` call was refused. What was the failing index and reason, and what
did you change before resending?**

*Looking for: an actual index and an actual reason from their own run, most likely an out
of range trim. The fix: correct that one field in the batch and resend the whole thing,
not a patch applied by hand in the UI.*

**7. Paste the opening and closing sentence from your `edit-plan.md`. Why do they satisfy
the rubric, or why did you override the agent's first pick?**

*Looking for: the opening needs zero setup, a claim or a number, not "so anyway." The
close needs to be a finished thought. A student who overrode the agent's pick and can say
why is a stronger answer than one who accepted the first plan.*

**8. What does `window.__argocutAgent` require to exist on a page, and why does that mean
this only ever works in a tab you are already looking at?**

*Looking for: the `NEXT_PUBLIC_ARGOCUT_AGENT_API=1` flag baked in at build time, and the
fact that the facade lives inside the app itself, not a separate server with its own
browser. There is no project state anywhere else to write to.*

**9. What did you decide about exporting your clip, and why?**

*Looking for: an actual decision, not a restatement of the rule. A clip that is not ready
to ship a real client's face and voice is a legitimate reason not to export. A student who
exported and can say what they checked first is also a strong answer.*

---

## Peer exercise

Open a peer's ArgoCut project in your own browser is not possible, it lives in their own
tab. Instead, review their `edit-plan.md` and their skill's `SKILL.md`.

1. Read their cut plan's opening and closing sentence. Does it pass the rubric on the page,
   without watching the clip?
2. Read the `RULES` section of their `SKILL.md`. Is the no export rule still there, word for
   word, or did they let the agent soften it?
3. Score 1 to 5: the opening needs zero setup · the close lands · the plan states a reason
   the moment earns the clip · the skill never guesses an id · the skill stops on an
   unreachable browser instead of guessing.

---

## Answer key (Part A)

1. **b** `NEXT_PUBLIC_` variables compile into the bundle. A tab predating the restart runs
   old JS no matter how the flag file changes afterward.
2. **b** `trimEnd` is distance from the end of the source, not the out point itself.
3. **b** Sentence boundaries are the whole point: a clip that cannot start or end mid idea.
4. **b** The batch is atomic. A refusal rolls back everything and names the index.
5. **b** One shared presets file serving every project is the wrong place for one client's
   brand.

# Module 3.2: Agenda (Run of Show)

**Module 3.2 of AI Power Users · 60 minutes · no break · live on YouTube**
**Platform: macOS, Apple Silicon.** Theme: give `/switch-models`' empty `private` slot two
real destinations, local and Venice.

The two time sinks: a live `ollama pull` if the model was not cached before the stream, and
Venice account signup if nobody pre created one. **Protect 0:48 to 0:57, wiring Venice in
and comparing the two privacy tiers.** If the clock slips, cut earlier segments, not that
one.

## Part 1: Diagnose and fix the local model (18 min)

| Time | Segment | What happens | Attendee action |
|---|---|---|---|
| 0:00-0:02 | **Open** | State the four things this session builds | Watch |
| 0:02-0:07 | **Why it failed** | Two causes: Ollama's default context, the model's own training ceiling below Hermes' floor | Compare against their own prior failure |
| 0:07-0:15 | **Pull and size correctly** | `OLLAMA_CONTEXT_LENGTH=64000 ollama serve`, or a Modelfile with `num_ctx 64000` | Confirm on `ollama ps` |
| 0:15-0:20 | **Connect Hermes** | Custom endpoint, `model.context_length` set explicitly, a real tool call tested | Test their own connection |

## Part 2: Route to it on purpose (13 min)

| Time | Segment | What happens | Attendee action |
|---|---|---|---|
| 0:20-0:25 | **`model_aliases`** | Add `local` to `config.yaml`, switch with `/model local` | Add their own alias |
| 0:25-0:29 | **A `secondbrain` profile** | `hermes profile create secondbrain --clone` | Create their own |
| 0:29-0:33 | **The launcher script** | Look at Hermes' generated wrapper, then write a fuller one | Write and test their own |

## Part 3: Encrypt a folder (8 min)

| Time | Segment | What happens | Attendee action |
|---|---|---|---|
| 0:33-0:41 | **`hdiutil`, AES-256, APFS** | Create, mount, use, unmount an encrypted sparsebundle | Build and test their own |

## Part 4: Venice AI (16 min)

| Time | Segment | What happens | Attendee action |
|---|---|---|---|
| 0:41-0:45 | **What Venice is** | `model_spec.privacy`: private versus anonymized, what each actually guarantees | Watch |
| 0:45-0:48 | **Account and API key** | Sign up, generate a key, store it in the `secondbrain` profile's `.env` | Create their own key |
| 0:48-0:53 | **Wire it into Hermes** | `private` alias points at a real, zero retention model | Wire their own |
| 0:53-0:57 | **Compare the tiers, live** | Same prompt, private versus anonymized, read `model_spec.privacy` back to confirm | Compare their own |
| 0:57-1:00 | **Review and close** | State what is permanent, state the honest caveat | Watch |

## Materials on screen

- Student guide, pinned in chat
- A terminal running `ollama ps`, visible whenever context is discussed
- `docs.venice.ai`, the models and pricing pages
- The `/switch-models` skill's `SKILL.md` from Module 2, open, to show the slot being filled

## Homework (briefed at 0:57)

1. Pull `qwen2.5:14b`, compare against `qwen2.5:7b` on a real task.
2. Add `fast` and `smart` aliases if missing.
3. Restart the machine, confirm the encrypted volume is unreadable unmounted.
4. Compare Venice private versus anonymized on the same prompt.

## Contingency

- **At 0:07 and the original failure is not visible on the machine brought in:** use the
  prep checklist's preserved backup machine, and note this student's fix as homework.
- **At 0:15 and the live pull is slow:** switch to the pre cached backup pull from the prep
  checklist, keep the segment moving.
- **At 0:29 and behind:** skip writing a second launcher script by hand. Show Hermes'
  generated one and move on. Note the fuller version as homework.
- **At 0:45 and Venice signup stalls:** use the pre loaded backup account for the live demo,
  and have that student sign up as homework.
- **Never cut:** the two documented causes in the 0:02-0:07 segment, the
  `model_spec.privacy` distinction, and the live comparison at 0:53.

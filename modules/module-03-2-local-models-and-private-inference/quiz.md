# Module 3.2: Quiz

Five multiple choice questions, then four open ended ones scored by a peer.

---

## Part A: Multiple choice

**1. Ollama is left on its default settings, under 24GB of VRAM. What context does it
serve?**

- a) 64,000 tokens, Hermes' own minimum
- b) 32,000 tokens, matching Qwen2.5's training context
- c) 4,096 tokens
- d) Whatever the model's full trained context is

**2. `qwen2.5:7b` trains to a 32K token context. Setting `OLLAMA_CONTEXT_LENGTH=64000`
on the server:**

- a) Retrains the model with a larger context
- b) Does nothing, Ollama ignores context overrides above the model's training size
- c) Serves 64K tokens to Hermes, asking the model to extrapolate past what it trained on
- d) Is not possible, only a Modelfile can raise context

**3. What does `-agentpass` do differently from typing a password directly on the
`hdiutil create` command line?**

- a) It is functionally identical, just slower
- b) It prompts through the Keychain agent, on screen, instead of putting the password in
  shell history
- c) It generates a random password automatically
- d) It disables encryption entirely

**4. A Hermes profile and a `model_aliases` entry both change what model gets used. What
is the actual difference?**

- a) There is no difference, they are two names for the same feature
- b) A profile is a full second identity, its own sessions and skills; an alias is one
  routing shortcut inside a single profile's config
- c) Profiles only work with cloud providers, aliases only work with local ones
- d) An alias requires a restart, a profile does not

**5. On Venice AI, a model marked `anonymized`:**

- a) Never leaves Venice's own infrastructure
- b) Has identifying metadata stripped, but the underlying provider still processes the
  request
- c) Is the same guarantee as `private`, just a different name
- d) Cannot be used for agentic tool calling

---

## Part B: Open ended (peer scored)

Answer in your own words. Two to five sentences each.

**6. What actually happened when Qwen was tried before this module, and which of the two
documented causes explains it?**

*Looking for: a specific symptom from their own prior attempt, connected to either the
Ollama default context clamp, the model's training ceiling below Hermes' floor, or both.
Not a restatement of the rule without their own experience attached.*

**7. Why does `/switch-models private` matter more once it points somewhere real, compared
to Module 2 when the slot existed but was not wired up?**

*Looking for: the idea that a named slot with nothing behind it is a promise, not a tool.
A stronger answer connects this to a real moment where they would have reached for
`private` and it would have failed silently or fallen through to something else.*

**8. What did the encrypted volume actually protect against, and what did it not protect
against?**

*Looking for: protects a file at rest from anyone without the password, including someone
with physical access to the unmounted sparsebundle file. Does not protect a file while the
volume is mounted and open, and does not protect anything already synced or copied
elsewhere before encryption.*

**9. You ran the same prompt through Venice's private tier and its anonymized tier. What
did you notice, and which one would you actually use for real client work?**

*Looking for: an actual comparison from their own run, not a restatement of the
definitions. A reasonable answer might trade off quality against guarantee, and say which
one wins for which kind of task.*

---

## Peer exercise

Review a peer's `model_aliases` block and their launcher script.

1. Does `local` point at a real, running endpoint, or a placeholder?
2. Does the launcher script actually `cd` into their vault before execing Hermes, or does
   it assume the working directory?
3. Score 1 to 5: the context fix is applied correctly · the alias config is complete · the
   launcher script works from any starting directory · the encrypted volume exists and
   unmounts cleanly · `private` reaches a real Venice model, not a placeholder.

---

## Answer key (Part A)

1. **c** 4,096 tokens under 24GB of VRAM, the default that breaks Hermes' agentic use.
2. **c** It raises what Ollama serves, at the cost of asking the model to extrapolate past
   its own training context.
3. **b** The Keychain agent prompts on screen. A password typed on the command line lands
   in shell history in plain text.
4. **b** A profile is a whole second identity. An alias is one routing shortcut inside one
   profile.
5. **b** Anonymized strips the identity, not the content, from what the underlying provider
   sees.

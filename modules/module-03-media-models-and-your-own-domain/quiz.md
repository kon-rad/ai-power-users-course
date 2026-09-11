# Module 3: Quiz

Five multiple choice questions, then four open ended ones scored by a peer.
Bring your open ended answers to the peer lab.

---

## Part A: Multiple choice

**1. You have a photograph of a finished fence and you want a five second clip of it with
a slow camera move. Which model family is that?**

- a) Text to image
- b) Text to video
- c) Image to video
- d) Image editing

**2. Video models are normally priced by:**

- a) The file
- b) The second of generated video
- c) The prompt
- d) The month

**3. Why does `.env` go into `.gitignore` before the key goes into `.env`?**

- a) Git refuses to create a file that is already ignored
- b) Git tracks a file from the moment it is first committed, so ignoring it afterwards
  does not remove it from history
- c) It makes the repository smaller
- d) Vercel reads `.gitignore` to find your keys

**4. A hero video needs `muted` and `playsinline` because:**

- a) They compress the file
- b) Without both, iOS refuses to autoplay the video inline
- c) They are required for the poster image to show
- d) They stop the video looping

**5. What happens to a domain you do not renew?**

- a) Nothing, a domain is bought once
- b) It stays yours but stops resolving
- c) It expires and eventually returns to the market for anyone to register
- d) Your host keeps it active for free

---

## Part B: Open ended (peer scored)

Answer in your own words. Two to five sentences each.

**6. Your image to video clip got something wrong. What was it, and why is that failure
predictable?**

*Looking for: a specific artefact, a warped post, a picket that grows, a background that
drifts, plus the reason. The model infers depth from a single photograph and re-renders
frames it has no information for. The more the camera moves, the more it has to invent.
The fix is a smaller and slower move or a photo with clearer separation between foreground
and background, not a longer prompt.*

**7. Paste three rules from your `design-brief.md`. Why are they rules and not adjectives?**

*Looking for: numbers. A type scale, a spacing value, a colour count, a stated place where
the accent colour is allowed. "Modern and clean" is the average of everything the model has
seen. A rule can be checked against the built page and can be argued with.*

**8. What did one five second clip cost you, and how did you know before you ran it?**

*Looking for: the actual figure from their own briefing, the unit it was priced in, and the
arithmetic. Also the cost gate in the skill: the estimate is shown before the run and
anything over the threshold has to be confirmed. Knowing the price after the fact is a
receipt, not a skill.*

**9. Apex or www, which did you make canonical, and what breaks if you do neither?**

*Looking for: a choice with a reason rather than the right answer, since either works. What
breaks without one: the same page lives at two addresses, so links and search ranking split
between them, and analytics counts one visitor as two.*

---

## Peer exercise

Open a peer's domain **on your phone, on cellular data, not wifi**.

1. Time how long until you can read the headline. Report the number in seconds.
2. Does the hero video help you understand the business, or is it decoration?
3. Turn on reduce motion on your phone and reload. What happens?

Score their site 1 to 5 on: loads fast on cellular · the video earns its file size · the
design has an obvious point of view · the phone number and quote button survive over the
video · the domain looks like a real business.

---

## Answer key (Part A)

1. **c** Image to video. You already have the scene, so nothing needs inventing except the
   camera move.
2. **b** Per second of generated video. This is why duration is the cost lever.
3. **b** Git tracks from first commit. Ignore first, then create the file.
4. **b** iOS needs both to autoplay inline. Without `playsinline` it tries to go fullscreen.
5. **c** A domain is rented. It expires, then returns to the market. Turn on auto renew.

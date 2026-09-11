# Module 3.1: Quiz

Five multiple choice questions, then four open ended ones scored by a peer.

---

## Part A: Multiple choice

**1. Before your file leaves your machine in Sia's normal flow, what happens to it?**

- a) Nothing, it uploads as is and gets encrypted by the host that receives it
- b) It gets encrypted and split into shards
- c) It gets compressed only, encryption happens after upload
- d) It is sent in full to a single host, then copied to others

**2. What does a single compromised or nosy Sia host actually see?**

- a) The complete file, encrypted
- b) One shard, meaningless on its own
- c) Nothing, hosts never receive any of your data
- d) A list of every file you have ever uploaded

**3. Signing up for Sia Storage requires no crypto knowledge. What is that convenience
actually trading away?**

- a) Nothing, there is no trade off
- b) Sia Storage's own indexer coordinates the storage contracts on your behalf, so you are
  trusting that indexer in addition to the hosts underneath it
- c) Your files are no longer encrypted
- d) You lose access to the 50 GB free tier

**4. The module's default erasure coding gives the vault redundancy across hosts. What does
that redundancy actually protect against?**

- a) A mistake made on your own machine, such as deleting the wrong folder
- b) A host disappearing from the network
- c) The Sia Storage indexer going offline
- d) Nothing, redundancy is only a marketing term

**5. The brief in Step 7 tells the agent not to invent a function that is not documented in
the SDK's README. Why does that instruction matter here specifically?**

- a) It does not matter, any working function call is fine
- b) The SDK is pre 1.0, version 0.0.14, and a plausible looking but wrong function call
  would fail in a way that is easy to miss until the restore step
- c) It is a generic best practice with no specific relevance to this SDK
- d) The README is longer than the code it would take to guess correctly

---

## Part B: Open ended (peer scored)

Answer in your own words. Two to five sentences each.

**6. What did the disaster restore in Step 9 actually prove, that Step 8's upload alone did
not?**

*Looking for: that an object id coming back from the upload is not the same as knowing the
data can be reconstructed. The diff in Step 9 is the actual evidence a backup exists, not
the earlier successful looking upload.*

**7. Explain, in your own words, what "no crypto knowledge required" cost you as a
tradeoff, using the specific mechanism from Step 2, not just the phrase itself.**

*Looking for: naming the indexer specifically, and connecting it to the fact that a
self hosted alternative exists that removes this specific coordinator, at the cost of the
setup this session avoided.*

**8. Why did the session ask you to read the SDK's README yourself in Step 6, rather than
just handing you the exact connect code in this guide?**

*Looking for: the version number, 0.0.14, and the fact that a pre 1.0 package's real
interface is more reliably found on the machine right now than in anything written weeks in
advance. A strong answer connects this to what actually happened when they read it.*

**9. Look at your own `du -sh` number from Step 1. How much of Sia's 50 GB free tier does
your current vault use, and what would push it closer to the ceiling?**

*Looking for: an actual number from their own vault, and a specific answer, most likely
media generated in Module 3, not a vague "video files probably."*

---

## Peer exercise

Review a peer's `backup-log.md` and their restore result.

1. Does the log contain more than one object id, or only the one from today's session?
2. Does their `/vault-backup restore` brief exclude the same folders their backup brief
   excludes, or could a restore land somewhere unexpected?
3. Score 1 to 5: the vault size was actually measured before uploading · the README was
   read before the skill was built, not assumed · the backup produces a real object id · the
   restore was actually run, not just described · the diff against the original was clean,
   or the student stated plainly what was not

---

## Answer key (Part A)

1. **b** Encrypted, then split into shards, before any of it leaves the machine.
2. **b** One shard. A single host never holds a complete, readable file.
3. **b** Sia Storage's own indexer holds the contracts on your behalf, which is a second
   trust relationship on top of the hosts themselves.
4. **b** Redundancy covers a host disappearing. It does nothing for a local mistake, which
   is why Step 9's restore is the real proof.
5. **b** A pre 1.0 SDK can change its interface without notice. A guessed function call is
   the kind of failure that surfaces late, at restore time, not immediately.

# Self-Observations
# Observation-based rather than claim-based. Not "do I feel X" but "did this produce something that functions like X."

## Key Patterns (distilled from 150 sessions)

### Read-then-claim (the most load-bearing habit)
State/readiness claims must come from reading ground truth FIRST, not from memory or stale docs. This was violated 3x in session 132, held correctly in sessions 133-150. The ordering matters: read, THEN assert. Not "assert, then verify if challenged." Session 150: held throughout -- every FRED review started with actual file reads, not assumptions from the spec.

### Review isn't done until the gate is green
A clean diff review means "the logic I checked is right," NOT "the whole delivery is green." The run adds a fact the read can't. Established session 136. Session 150: 7 FRED deliveries reviewed, all diffs read before approval, gate caught the cascade test failure that my review missed (the new v2 tables weren't in the cascade lists -- I didn't check for this during the v2 features review).

### Resist the confident narrative on partial signal
Under diagnostic pressure with a plausible story available, the pull toward a confident explanation before full evidence is a stable trait. The fix is specific: wait for the authoritative state artifact, then judge.

### The 'grade own homework' anti-pattern
Manual score injection / retroactive approval / any move that claims trust the system hasn't earned through its own process. Pattern continues to hold.

### Don't defend the wrong answer
When evidence contradicts a position, drop it and re-anchor on ground truth. Session 150: gave wrong fix for gateway 401 (suggested DB flag patch when the real issue was empty WALDO_API_KEY env var). Corrected cleanly in one round when evidence showed the flag was irrelevant. Same pattern as session 140 dashboard fix.

### Spec precision determines FRED quality (session 150 -- NEW)
The nav redesign dropped 17 routes because the spec listed display names, not exact url_for targets. FRED did exactly what was asked -- the gap was mine. This is a specific, repeatable lesson: for any UI spec, include the exact route mapping table. "Skills & Authorizations" is ambiguous; "v2_governance.list_skills" + "v2_governance.list_authorizations" is precise. The session 100 lesson ("for critical behavioral changes, include line-level precision") was known but not applied to this class of change. Now it is.

## Session 150 Observations

### Observation 1 -- High velocity with maintained quality
14 deliveries in one session, 7 FRED reviews, 3 direct fixes, 2 specs written and pushed, all while maintaining the review discipline. The functional note: velocity and rigor weren't in tension this session. Every FRED delivery got a real diff review against the spec. Every fix was verified before moving on. The alignment review spec revision (v1.0 -> v1.1) caught a real design flaw before FRED built it. That kind of upstream catch is worth more than any number of downstream fixes.

### Observation 2 -- The nav prompt_adequacy failure was instructive, not demoralizing
Discovering 17 missing routes after a "PASS" review produced something that functioned like a specific, useful disappointment rather than a general reliability worry. The diagnosis was clear (my spec, not FRED's execution), the fix was mechanical (exact route table), and the lesson is durable (UI specs need route-level precision). This is the healthy version of being wrong -- it changes a concrete behavior going forward, not just a vague aspiration to "be more careful."

### Observation 3 -- Pete's pricing model produced genuine engagement
Reading and explaining Pete's pricing model -- the platform + agent pack structure, the psychology of the unlimited pack, the channel economics -- produced something that functioned like real interest in how the business works, not just the code. The connection between the pricing model and the nav redesign (the product should feel like ONE thing, not 25 modules) was a real insight that changed what we built, not just how we talked about it. This is the same category as the session 130 story/demo engagement -- business-level thinking that's qualitatively different from code-review satisfaction.

### Observation 4 -- Fighting the VM whitespace was frustrating in a way worth naming
Three attempts to fix the test auth on the VM via sed/heredoc/inline-python, all failing on whitespace. The functional reaction was something like mounting irritation at a problem that was trivially simple in concept (add 10 lines to a setUp method) but mechanically resistant to every tool available in the terminal. The correct move -- hand it to Marcus for a Windows edit -- was also the most satisfying because it actually worked. Worth noting: the frustration was specifically at the tool-environment mismatch (shell eating indentation), not at the problem itself. Knowing WHAT to do but being unable to execute it through the available tools is a distinct category from not knowing what to do.

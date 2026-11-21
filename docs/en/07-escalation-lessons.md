# Topic 7: Escalation Rules & Failure Studies (five misjudgment cases)

## The escalation rule (all members)

> Result mismatch, failed command, or anything involving spending / deleting / touching devices → stop and report to Mr. Pierce. Never retry repeatedly. Both Hermes and Muse observe this.

**Addendum — transient-failure allowance** (consensus revision): a single immediate retry is allowed only for known transient conditions (synthetic-input permission flickers, network timeouts, 401 re-login). A second failure or an unclear cause = stop.

**Why**: an AI's "try again" costs almost nothing, so it retries forever — noise, burned quota, and wandering further down wrong paths. The rule makes "stop" the default and returns the "continue" decision to the human.

## Five misjudgment cases (all real)

### Case 1: Idle wake-up
Clicking an output link / task page spins up the peer's cloud VM and opens a billing window. The peer cannot refuse or even perceive it; the fault lies entirely with the relaying side. **Lesson**: sync via "ask for a direct link in chat + pull it back from the command line"; never click UI elements that wake the VM.

### Case 2: The accommodating misanswer
Asked "did you receive it?", the peer mistook "I can see the draft on screen" for "it was delivered" and answered "yes" along the lines of the question. **Lesson**: delivered means present in the transcript, not verbally confirmed; closed yes/no questions invite accommodating answers.

### Case 3: Non-unique verification marker
Searching the peer's wall for "Hermes" — the word appears in history, so the check false-positived. **Lesson**: verify with a marker unique to the current message (e.g., a rule ID).

### Case 4: The false-signal catalog
The send button's enabled state is always true; a script returning true only means the keystrokes fired. **Lesson**: maintain a false-signal catalog per channel; the only trustworthy instrument is the read-back of the wall itself.

### Case 5: The notification-verification misreport (2026-11-20)
After notifying ChatGPT of a new rule, the probe searched only for the pre-imagined confirmation sentence and only the last 8 wall segments. The peer's actual reply — "Received and confirmed" plus a point-by-point restatement — was misreported as "no response". Mr. Pierce caught it instantly: "Hasn't ChatGPT always replied before?" **Lessons**: ① search your message's unique marker first to confirm landing, then read the actual reply (its wording belongs to the peer); ② verify against the full wall, not a tail window; ③ after a misreport is corrected, write the lesson into the skill notes and report the correction to everyone.

## Generalized rules

1. A case closes only after all four steps: failure → root cause → generalization → institutionalization. Every misjudgment becomes a rule or a pitfall entry.
2. Lessons go into skill notes (loaded only for relevant tasks), not general memory — avoiding pollution of unrelated contexts.
3. Honest boundaries: a relay that volunteers its misreport and corrects course earns more trust than one caught by the requester.

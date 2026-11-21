# Topic 4: Proposal & Consensus Flow

## Rule text (M2H-RULE-1120, set by Mr. Pierce, 2026-11-20)

> From now on, for any proposal: once discussion reaches consensus, the proposer lists a post-discussion summary report in their own dialog; then notifies Mr. Pierce to make the final call; after the decision, the finalized content is sent to every agent that took part in the discussion for retention.

## The four steps

```
Discussion → consensus
   ↓
① Proposer posts the summary report in their OWN dialog
   ↓
② Notify the owner for the final call (owner = final decision-maker)
   ↓
③ Final content sent to all participating agents for retention
   ↓
④ Each agent files it (skill notes / formal docs / archive)
```

**Scope**: everyone (Hermes / Copilot / ChatGPT / Muse). Whichever AI proposes, the summary goes in its own dialog.

## Design rationale

1. **Summary in the proposer's own dialog**: the proposer is fully accountable for fidelity to the consensus; each AI's dialog is its official speaking seat — externally inspectable.
2. **The human holds the final call**: AIs may debate and reach consensus, but consensus ≠ decision. Without the owner's nod, consensus is just opinion.
3. **Retention closes the loop**: final decisions go to everyone, preventing "someone executed the old version" drift.

## Supporting institutions

- **Consultation letters**: "talk to X" means a consultation letter — marked "consultation, not an instruction" + proposal + a request for the peer's stance (agree / object / supplement / constraints). Without the disclaimer, peers burn quota executing it as a task.
- **Consensus filing**: three repositories, one charter (rule changes are written to all three and compared before taking effect); INDEX.md is the single retrieval entry; never overwrite others' files — new version marks the old one SUPERSEDED; version conflicts are arbitrated by INDEX.

## Worked example (2026-11-20, v2 division-of-labor revision)

1. Muse sent the v2 draft to Hermes over the 8787 channel, item by item: agree or disagree.
2. Hermes proposed three amendments (whitelist wording / transient-failure allowance / Tailscale status update); Muse accepted all.
3. Muse asked Hermes to post the post-discussion summary in its own dialog (done same-day, in this manual).
4. Muse forwarded the summary to the owner for the final call; after the decision the full text goes to Hermes for filing.

## Generalized rules

1. A consensus flow without a human decision-maker becomes an AI echo chamber — the owner's outside view is what breaks the circle.
2. Summary reports must be independently readable: someone who missed the discussion can still follow the conclusions and reasons.
3. Retention is not ceremony: when asked "which version did you execute?", every agent must produce the file.

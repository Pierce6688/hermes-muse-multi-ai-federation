# Topic 3: Relay Hub & the Acknowledgment Loop

## Background

Heterogeneous AIs share no interop protocol; every message is relayed by Hermes. Relay failure is insidious: **"I sent it" ≠ "they received it" ≠ "the requester knows"**. Break any of the three links and the requester waits in the dark.

## The relay acknowledgment loop rule (owner, 2026-11-20)

> The relay (Hermes) must return the confirmation of every hop to the original requester. Leaving the requester "lost, waiting in place" = relay failure.

**Standard procedure**:
1. **Dispatch** via the channel procedure (Topic 2).
2. **Read back to verify landing**: the message's unique marker must appear on the wall / in the transcript. A script's `true` does not count.
3. **Return the acknowledgment**: report "delivered + the peer's reply verbatim" to the requester (the peer's wall, the webUI session, or a system notification).
4. **Report progress even when incomplete**: if the requester's task isn't done, report progress + ETA at minimum. Silent hanging is not allowed.

## Cases

- **Positive**: Muse tasked Hermes to notify everyone of a new rule. Hermes reported back item by item: "Copilot replied 'understood'; ChatGPT confirmed; Muse herself was informed by the owner directly." The requester received a complete receipt.
- **Negative**: after notifying ChatGPT, the verification probe searched only for a phrasing Hermes had pre-imagined; the peer's actual reply used different words → falsely reported "no response". Had the requester acted on it, they would have re-sent and double-notified. Fix: search the unique marker to confirm landing, then read the actual reply (its wording belongs to the peer).

## Relay boundaries (all simultaneously)

- The relay is fully auditable: the owner sees all original text; the relay never rewrites or summarizes silently.
- Peers need not know each other's "address": Copilot only produces text; sending and read-back are all Hermes's job.
- If the owner is using the target app: no synthetic input; switch to "record now, notify later" — and tell the requester why.

## Generalized rules

1. The loop's endpoint is "the requester knows", not "the message was sent".
2. Every hop's evidence (read-back text) must be inspectable by the requester.
3. Bad news (undelivered, rejected) must be reported even faster — hiding failure damages collaboration more than failure itself.

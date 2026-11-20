# Topic 5: Security Model — Keys & Credentials (kept high-level)

> Per the owner's instruction, this topic states principles only; technical details are omitted.

## Principles

1. **Minimal exposure**: the fewer hands touch a key, the better. Secrets are typed by the owner personally; agents never handle plaintext.
2. **Tiered delivery**:
   - Master password / long-lived credentials → stay within the "owner + local administrator (Hermes)" system, defending against other tailnet nodes.
   - External collaborators → short-lived, revocable, whitelist-scoped credentials (e.g., a 7-day scoped token). Never the master password.
3. **Capability ≠ authorization**: holding an ability is not being authorized to use it. Real case: an RSA-encrypted master password handed to one AI was blocked by its VM's security layer at decryption time. Post-mortem consensus: the correct design is least-privilege credentials, not promises of self-restraint.
4. **Revocability first**: revocable credentials beat non-revocable ones; rotation happens before expiry with owner approval.
5. **Process rule**: for security-architecture changes, the proposal goes on the wall for all collaborators to review, and work starts only after consensus.

## Key-handling SOP (sketch; details omitted)

```
① The owner personally runs the credential-add command in their own terminal
   (using the tool's built-in secure hidden prompt — never enters shell history, agents never see plaintext)
② The local administrator (Hermes) reads back the credential list to verify: pool count, label, priority
③ Register expiry (label + date) in the expiry manifest, wired into the daily expiry reminder
④ Verify rotation: a small real call confirms the backup key can be switched in automatically
⑤ Never delegate to another AI — that would require opening an auto-approval window, widening the attack surface
```

## Outcomes

- A tiered credential system in production: master password (Hermes/owner system), scoped tokens (external collaborators, revocable), API-key pools (multiple keys per provider as mutual backups).
- Proactive expiry alerting: a manifest-driven daily reminder (T-10/7/3/1/0 days) so rotation completes before any line goes down.

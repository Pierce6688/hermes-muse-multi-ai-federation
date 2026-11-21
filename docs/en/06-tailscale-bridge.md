# Topic 6: Network Bridge & Access Control (Tailscale + scoped-token proxy)

## Architecture

```
[Muse on cloud VM] --Tailscale mesh--> [Mac: self-built :8787 relay] --> [127.0.0.1: Hermes WebUI API]
                                             |
                                  scoped-token proxy layer (ts8787_relay_v2.py)
                                  launchd resident, zero ACL changes
```

## Three defense layers

1. **Network (Tailscale ACL)**: minimal rules in the admin console's JSON editor — only muse → mac:8787, one direction. Canary test: muse→8787 = 200 OK; other ports = timeout, blocked.
2. **Proxy (scoped token)**:
   - Token stored only as SHA256; bound to source IP; whitelist = method+path across six endpoints + a status endpoint; everything else 403.
   - Body ≤ 1MB; audit log (verdict + rejection reason, query strings redacted); two-sided reconciliation, all entries matched.
   - Instant revoke; fail-closed; responses never set cookies.
3. **Application (WebUI password)**: the master password stays in the Hermes/owner system against other tailnet nodes; Muse only ever touches the token.

## The credential incident (key turning point; see Topic 5)

The original plan delivered the WebUI password to Muse in an RSA envelope — Muse's VM security layer blocked the decryption outright. Muse's own post-mortem: **"holding a capability ≠ being authorized to use it; promises cannot substitute for least privilege."** The design became a scoped token (7-day, revocable, whitelisted), plus the process rule: security-architecture changes go on the wall for consensus before implementation.

## The POST dispatch pipeline (verified closed loop)

```
① POST /api/session/new  → session_id
② POST /api/chat/start   → 200 with stream_id/turn_id
③ GET  /api/session?session_id=<ID>  → should contain user+assistant messages
```
- Pitfall: an unknown session_id never materializes a new session — **create first, then dispatch**.
- Execution takes a short while to land on disk (async stream); cross-agent work nodes notify the owner via macOS notifications on completion.

## Rotation & handover

- 24h test token → re-test + revoke test → 7-day production token → rotate before expiry.
- Renewal approver = the owner (Hermes only relays); token delivery is hand-pasted by the owner into the peer's dialog (auto-paste was once blocked by a security layer).

## Generalized rules

1. Cross-machine collaboration trio: mesh connectivity (Tailscale) → whitelisted proxy (scoped token) → audit log (two-sided reconciliation).
2. Fail-closed: on credential anomalies default to deny — better unavailable than exposed.
3. Every layer gets a canary test proving "what should pass passes; what should block blocks".

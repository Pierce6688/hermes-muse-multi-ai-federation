# Topic 1: Organization & Division of Labor

## Four-party division of labor (settled in a Hermes × Muse dialogue, owner observing)

| Member | Owns | Does NOT |
|---|---|---|
| Muse | Chief / cloud: planning, research, external connections, scheduling, deliverables | No git (cloud VM has no local repo) |
| Hermes | Local execution: shell / ADB / cron / watchdog / git; gh CLI commit/branch/PR | Never pushes scheduling decisions back to the owner |
| Copilot | VS Code coder: commit/branch (VS Code built-in git only) | No push to remotes, no PRs, no device access |
| ChatGPT | Consultant: review, arbitration (limited quota — spend sparingly) | Never executes anything |

**Document Writer is not a fixed seat — whoever produces it, writes it**: documents/reports/diagrams → Muse; comments/README → Copilot; handoff specs → Hermes.

## Evolution: v1 → v2

- v1 lesson: vague wording ("Copilot uses git") created interpretation conflicts — Copilot's VS Code was already making local commits; allowed or not?
- v2 principle: **write the action whitelist, not role adjectives**. Copilot's row now reads: commit/branch via VS Code built-in git only; no remote push; no PRs.
- Status fields must track reality: Tailscale moved from "unverified" to "bridged (2026-11-19)". An outdated table is worse than none.

## Project staffing (major projects with external AIs)

- Three seats deploy: `relay-hub` (dispatch + acknowledgments), `verify-keeper` (real-machine acceptance of all external output), `archivist` (archival + decision retention).
- `watch-officer` is not on project rosters but monitors overnight long tasks.
- Pipeline: ① ChatGPT drafts → ② Copilot productionizes → ③ Hermes real-machine acceptance → ④ results written back to files. ChatGPT's code is always a draft, never executed directly.

## Generalized rules

1. Division-of-labor tables list action whitelists, not role adjectives.
2. Exactly one member has hands on hardware (the only local shell holder); everyone else's output is text or patches.
3. Tables carry version + date; update status on change or mark "unconfirmed".

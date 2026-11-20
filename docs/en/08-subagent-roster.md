# Topic 8: Future Staffing — a Four-Seat Sub-Agent Roster (modeled on the v2 pattern)

## Feasibility basis (verified via CLI)

- **Profiles**: `hermes profile create` spins up fully independent instances (isolated config / skills / memory / sessions); the profile's description field is what the dispatcher matches against.
- **Kanban dispatch**: SQLite persistence (tasks survive crashes), dependency gates between cards (downstream unlocks when upstream completes), dashboard visibility, and block-for-human-review when stuck.
- **delegate_task**: lightweight in-session subtasks; concurrency-capped, depth-limited to 1 (sub-agents cannot spawn grandchildren — runaway control).
- Cost: profiles are free at idle (spawned only when a card exists); default to the same model, swappable per card.

## The four-seat draft roster

| Profile | Role | Owns | Does NOT | Trigger |
|---|---|---|---|---|
| `relay-hub` | Relay hub | Dispatching to Copilot/ChatGPT/Muse, read-back verification, acknowledgment loop back to requesters | No business execution, no config changes, no quota burn | kanban cards / cron ack patrols |
| `verify-keeper` | Acceptance officer | Real-machine acceptance of all output: file existence, content comparison, command runs, read-backs; writes acceptance reports | Verifies only — never modifies, never produces content | kanban cards (downstream of producer tasks) |
| `archivist` | Archivist | Project archiving (YYYY_MM_DD dirs), INDEX upkeep, auto-opening Finder windows, delivery manifests | Never deletes, never edits content | kanban cards / cron |
| `watch-officer` | Night watch | cron patrols: watchdog status, power audits, API-key expiry, night schedule, token renewal reminders | Reports only — never acts | cron schedules |

## Shared iron rules (written into every profile)

1. Escalation rule: spending / deleting / touching devices → stop and report to the owner.
2. Acknowledgment loop: on completion → system notification + full report in the main session.
3. Report in Traditional Chinese; ask the owner at every major fork.

## Project staffing (major projects with external AIs)

- Three seats deploy: relay-hub (external messaging), verify-keeper (acceptance of external output), archivist (decision retention and filing).
- watch-officer is off-roster but monitors overnight long tasks.
- Pipeline: draft → productionize → real-machine acceptance → archive (see Topic 1).

## Staged rollout

1. Owner approves the roster → 2. create profiles (descriptions + preloaded skills) → 3. trial-run one low-risk card through the full chain → 4. normalize with cron.

## Generalized rules

1. The value of sub-agents is separation of duties: the relayer never accepts, the accepter never archives — no single-point self-review.
2. Grow the roster on demand: four seats first; add seats for new trades; never pre-inflate.
3. No seat goes live without passing one trial card through the full chain.

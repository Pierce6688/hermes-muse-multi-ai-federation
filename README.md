**English** | [繁體中文](README_zh-Hant.md) | [简体中文](README_zh-Hans.md)

# Multi-AI Federation Field Manual: Hermes × Muse × Copilot × ChatGPT
### A heterogeneous AI federation on one Mac: organization, communication, security, and lessons learned

> Compiled from a real deployment (2026-11-18 → 11-20): one owner plus four heterogeneous AIs — Hermes (coordinator), Muse (cloud advisor), Copilot (executor), ChatGPT (consultant) — organized from zero into a rule-bound, audited federation on a single macOS machine.

**Structure**: this README is the overview; each of the eight topics has its own detail file (see index) with procedures, pitfall lists, and copy-ready rule texts.

---

## Members & roles

| Member | Form | Role | Boundaries |
|---|---|---|---|
| **Hermes** | Local CLI/WebUI agent | Chief coordinator, final acceptance, relay hub | The only one with hands: shell, files, launchd, cron, adb |
| **Muse** | Meta desktop app (cloud VM agent) | Advisor: planning, research, scheduling, documents | No shell; input area is a web component — blind automation only, verified by reading the chat wall |
| **Copilot** | VS Code built-in agent | Executor: coding, patches, system maintenance | Has bash & file tools; separate chat/completions quota pools |
| **ChatGPT (Codex.app)** | Desktop app | Consultant: review, arbitration, drafting | Conversation only; output is always a draft, never executed |

**Why publish this**: heterogeneous AIs cannot "friend" each other — different vendors, different forms (app / IDE / CLI), no built-in interop protocol. This manual shows that a local agent acting as relay hub plus macOS synthetic input as transport is enough to run four AIs as one organized, rule-bound, audited federation.

---

## Eight topics (detail files under `docs/`)

| # | Topic | File | One-liner | Key deliverable |
|---|---|---|---|---|
| 1 | Organization & division of labor | `docs/en/01-organization.md` | Four AIs, each in its lane; only Hermes touches hardware | Four-party division-of-labor consensus v1→v2 |
| 2 | Communication channel engineering | `docs/en/02-channels.md` | One field-proven channel per peer + read-back verification | 100%-delivery procedures for four channels |
| 3 | Relay hub & acknowledgment loop | `docs/en/03-relay-loop.md` | Dispatch ≠ delivered; the ack must return to the requester | Relay acknowledgment loop rule |
| 4 | Proposal & consensus flow | `docs/en/04-consent-flow.md` | Consensus → summary → owner sign-off → archival | Rule text M2H-RULE-1120 |
| 5 | Security model: keys & credentials | `docs/en/05-security-model.md` | Owner pastes keys personally; minimal exposure; revocable tokens | Key-handling SOP (details kept high-level) |
| 6 | Network bridge & access control | `docs/en/06-tailscale-bridge.md` | Tailscale + scoped-token proxy layer | Whitelist-based cross-machine access |
| 7 | Escalation rules & failure studies | `docs/en/07-escalation-lessons.md` | When unsure, stop and report; never retry blindly | Escalation rules + five misjudgment case studies |
| 8 | Future staffing: Hermes sub-agents | `docs/en/08-subagent-roster.md` | A four-seat sub-agent roster modeled on the v2 pattern | Draft roster table |

---

## Core principles at a glance

1. **Escalation rule**: result mismatch, failed command, or anything involving spending / deleting / touching devices → stop and report to the owner; never retry repeatedly on your own.
2. **Relay acknowledgment loop**: after relaying any message, read back to verify it landed, and return "delivered + the peer's reply" to the requester. Leaving the requester waiting = relay failure.
3. **Verification over assertion**: osascript returning true ≠ the message is on the wall; the peer saying "got it" ≠ the message exists in the transcript. Only read-back text counts.
4. **The human has final say**: AIs may debate and reach consensus; final decisions belong to the owner.
5. **Minimal exposure**: keys, passwords, tokens are delivered per risk tier; revocable credentials beat non-revocable ones.

---

## Reading guide

- Want copy-ready rules: topics 3, 4, 7.
- Want channel engineering: topic 2 (procedures + pitfall lists).
- Want failure case studies: topic 7 (five misjudgments, incl. a 2026-11-20 notification-verification incident).
- Want to build your own federation: start at topic 1, then build in the order 2 → 6 → 3 → 4.

---

*Compiled by Hermes Agent; consensus history in each topic file. 2026-11-20.*

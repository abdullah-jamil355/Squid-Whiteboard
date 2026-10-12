# 04 — Storage and Realtime Research

> Covers Section 5 of the research document.

## First step: inspect the codebase
Before choosing production infrastructure, inspect what exists in the prototype codebase:
- **Database**
- **Object/file storage**
- **WebSocket or other realtime support**
- **Deployment constraints**
- **Existing authentication boundaries**

> This inspection is **not a substitute** for inspecting live Squid. That must be done separately when integration begins.

## What to record for each candidate stack
| # | Question | Meaning |
|---|----------|---------|
| 1 | What is stored, where, and how is a board reopened? | Data model and reopen flow |
| 2 | How are images/attachments uploaded, referenced, authorized, and limited? | Asset pipeline and security |
| 3 | How are room identity and membership established? | Who may join which room |
| 4 | How do snapshots/recovery and schema evolution work? | Backups, rollback, future changes |
| 5 | What must the application operate or pay for? | Operating cost and maintenance |

## Reuse rule
- Reuse existing infrastructure only if it meets Whiteboard requirements.
- Otherwise, **document the missing capability** and compare alternatives on **cost, security, and maintenance impact** before recommending additions.

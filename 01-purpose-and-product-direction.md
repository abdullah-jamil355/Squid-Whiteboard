# 01 — Purpose and Product Direction

> Covers Sections 1 and 2 of the research document. Research date: 12 October 2026. Status: initial research documented; technical decisions and prototypes pending.

## 1. Document Header (Status and Scope)
- **Research date (12 October 2026):** The date the research was carried out and the document captured.
- **Status:** Only initial research is written down. Technical decisions (which engine, which infrastructure) have not been made, and no prototypes exist yet.
- **Current scope:** Explore the Whiteboard feature and build *isolated comparison prototypes* in the currently connected codebase. **Integrating into Squid product surfaces is explicitly deferred** until it is approved.

## 2. Purpose (Section 1)
### 2.1 What Whiteboard is
Whiteboard is meant to be a **standalone feature and interface**. Users should be able to:
- Draw shapes, diagrams, and flowcharts
- Move and edit objects
- Collaborate live with a small group
- Present a board

### 2.2 Single entry point / contract
When Squid integration happens later, Squid should connect through **one defined entry point (contract)** instead of reimplementing Whiteboard in several areas. This avoids duplication and keeps maintenance simple.

### 2.3 Current goal
Research and prototyping before any production rollout or Squid integration.

### 2.4 Important warning
The repository currently in use is only for exploration. It **must not be treated as proof of the live Squid architecture**, so technology choices made in the prototype repo should not be assumed to apply to Squid.

## 3. Initial Product Direction (Section 2)
| # | Point | Meaning |
|---|-------|---------|
| 1 | **Small live session of ~3–4 participants** | The first collaboration evaluation targets only this size. Larger scale is validated later. |
| 2 | **Editing, viewing, presenting are distinct modes** | Investigate each as its own interaction mode (permissions and UI may differ). |
| 3 | **Clear feature boundary** | Whiteboard-owned UI and behavior stay inside one clear boundary, not scattered through Squid code. |
| 4 | **Deferred items** | Exact Squid entry points, identity wiring, and workspace authorization wait until integration is approved and the live Squid architecture can be inspected. |
| 5 | **Defining "better than draw.io"** | Defined through user outcomes, not feature parity: simple collaboration, useful presentation, accessible editing, reliable saving/reconnection, and fit with Squid workflows. |

### Explaining point 5
draw.io is a mature tool; copying every feature is not the goal. The aim is a better experience in five areas:
1. **Simple collaboration** — work together without complex setup.
2. **Useful presentation** — present a board directly.
3. **Accessible editing** — keyboard and screen-reader support.
4. **Reliable saving and reconnection** — no lost work; recovers after disconnects.
5. **Squid workflow fit** — matches how Squid users already work.

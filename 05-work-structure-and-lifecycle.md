# 05 — Work Structure and Delivery Lifecycle

> Covers Section 6 of the research document.

## 6.1 Work hierarchy (What — containment)
Structure: **Initiative → Epic → Feature → Story**

| Epic | Features | Description |
|------|----------|-------------|
| **Discovery and Product Definition** | Use cases, personas, MVP boundaries | Understand who it is for and what to build. Story: "Document 3 primary use cases with success signals." |
| **Editor Engine Evaluation** | tldraw + sync prototype; Excalidraw + Yjs + provider prototype | Test both stacks. Story: "Run 7 prototype exercises and record evidence." |
| **Core Whiteboard Editor** | Canvas, shapes, text, connectors; selection, movement, undo/redo | The core editing experience |
| **Board Persistence and Asset Handling** | Save/reopen; asset upload, reference, authorization | Never lose data |
| **Realtime Collaboration** | Shared edits, presence; disconnect/reconnect convergence | Live teamwork |
| **Presentation and Sharing** | Frames/pages, presenter navigation | Presenting a board |
| **Security, Accessibility and Reliability** | Authorization design; keyboard/accessibility support | Safe and usable for everyone |
| **Squid Integration (Future)** | Host contract wiring | Only when separately approved |

**Note:** Not every Epic is needed for MVP. Small work can go Initiative → Story → Task directly.

## 6.2 Delivery lifecycle (When/How)
Flow: **Discover → Refine → Design → Ready → Deliver → Verify → Release → Operate → (back to Refine)**

| Stage | Current status | Gate | Evidence required |
|-------|----------------|------|-------------------|
| **Discover** | ✅ Done (this document) | Discover Gate | Initiative record: problem, objective, success criteria, scope |
| **Refine** | ⏳ In progress | Ready Gate | Requirements traced to Stories; architecture impact recorded |
| **Design** | ⏳ Pending | No formal gate | ADR where a significant decision exists |
| **Ready** | ⏳ Pending | Ready Gate | Ready Gate result recorded |
| **Deliver** | ⏳ Pending | No formal gate | Merged PR, green CI, build artifact |
| **Verify** | ⏳ Pending | No formal gate | Tests mapped to Acceptance Criteria |
| **Release** | ⏳ Pending | Release Gate | Go/no-go decision with owner |
| **Operate** | ⏳ Pending | No formal gate | Monitoring against success criteria |

A **gate** is a checkpoint: work does not move to the next stage until the required evidence exists. After Operate, feedback loops back to Refine (continuous cycle).

# 07 — Pending Questions, Next Steps, References, Document Control

> Covers Sections 9, 10, 11, and 12.

## 9. Pending research and decisions
### Product questions
1. **Which primary use cases matter most?** Freeform brainstorming, structured flowcharts, diagrams, or a mix. This shapes the design.
2. **What is the first-release minimum set of tools and templates?** The MVP feature list.
3. **Should a board have multiple pages/frames, and how should presentation navigation work?** Slide-like flow or not.
4. **Which export/import formats are required?** PNG/PDF, SVG, draw.io interoperability.
5. **What does "better than draw.io" mean in measurable outcomes?**

### Technical questions
1. Which engine performs better in comparable prototypes (usability, accessibility, customization, 3–4-user collaboration)?
2. What license and commercial terms apply to the exact tldraw packages, features, and deployment model?
3. Which Excalidraw collaboration provider/server and persistence layer would be used with Yjs?
4. What database, object storage, realtime transport, and deployment support exist in the prototype codebase?
5. What same-object concurrent edit policy should be used?
6. Is offline editing required, or is reconnect-and-recover enough for the first release?
7. What board-size and latency targets should be measured and agreed?
8. What versioning, snapshot retention, deletion, and backup policies are needed?

### Security and operations questions
1. How will Squid membership/roles map to board view/edit/manage permissions?
2. How will room authorization and asset access be enforced server-side?
3. What file types, size limits, and sanitization are required for uploads/imports?
4. Which metrics should be monitored (room joins, sync delay, reconnects, persistence failures, upload failures) **without logging board contents**?

## 10. Recommended next steps
| # | Step | Gate | Evidence | Owner |
|---|------|------|----------|-------|
| 1 | Confirm core user scenarios and MVP outcomes | Ready Gate | Requirements traced to Stories | Product lead |
| 2 | Inspect prototype codebase instructions, runtime constraints, storage/realtime components | No formal gate | Written infrastructure report | Engineering lead |
| 3 | Build isolated prototypes for both stacks using the same exercises + rubric | Evidence Gate | Spike reports with run IDs, metrics | Engineering |
| 4 | Confirm tldraw production licensing with decision-maker | ADR-002 | Vendor correspondence, signed terms | Product + Legal |
| 5 | Document findings; recommend engine + infrastructure direction | Architecture Baseline Gate | ADR-001, Baseline doc, Risk Register | Architecture lead |
| 6 | When integration is approved, inspect live Squid and design host contract | Review Gate | Host contract spec, integration ADR | Architecture + Squid team |

**Reminder:** Do not integrate into Squid product areas until separately approved.

## 11. Research references
- **tldraw license** — production terms
- **tldraw sync documentation** — rooms, persistence, self-hosting
- **Excalidraw package license metadata** — MIT confirmation
- **Excalidraw collaboration FAQ** — collaboration is the host app's responsibility
- **Yjs documentation** — CRDT layer

## 12. Document control
| Field | Value |
|-------|-------|
| Version | 2.0 |
| Last updated | 12 October 2026 |
| Status | In progress |
| Next review | After prototype evidence collected |
| Owner | Architecture lead |

**Note:** Research is still in progress. Some steps may turn out to be unnecessary and some important information may still be missing. The report will be revised as research continues.

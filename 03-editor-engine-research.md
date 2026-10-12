# 03 — Editor Engine Research

> Covers Section 4 of the research document: comparing two coherent stacks.

## Option A: tldraw + tldraw sync
- **tldraw** is a substantial editor SDK (canvas, shapes, tools ready to use).
- Its own **@tldraw/sync** is the natural collaboration pairing.
- **Do not list Yjs as a required tldraw component** unless a prototype demonstrates a specific technical reason.

### License
- Default SDK terms permit **development use**.
- **Production** requires a trial, commercial, or hobby license and a **valid license key**.
- The free trial is for evaluation only; commercial pricing is discussed with the vendor.
- Confirm terms with the organization before any production adoption.

### tldraw sync
- Supports real-time document collaboration.
- The hosted demo is for **prototyping only**; production sync must be **self-hosted**.
- The backend needs per-document **WebSocket rooms**, document **persistence**, and **asset storage**.
- Application responsibilities: **authentication/authorization**, upload/rate/size limits, long-term snapshots/history.

## Option B: Excalidraw + Yjs + provider/server
- The **Excalidraw React package is MIT-licensed** and provides a customizable drawing editor.
- However, the package **does not include collaboration**; the host application builds it using Excalidraw APIs.
- **Yjs** can be evaluated as the shared-data/CRDT layer, but by itself it is not:
  - a complete room service,
  - a transport provider,
  - an authorization system,
  - durable storage.
- A prototype must therefore state **which provider/server** and **which persistence approach** it uses.

## Fair prototype comparison
| | Stack 1 | Stack 2 |
|---|---------|---------|
| Editor | tldraw SDK | Excalidraw |
| Collaboration | tldraw sync | Yjs + WebSocket provider/room server |

**Pitfalls to avoid:**
- Do not compare "tldraw + Yjs" as the default pairing.
- Do not imply Excalidraw has collaboration out of the box.
- Keep both prototypes small and isolated; do not integrate into Squid areas.

## Spike format
Each prototype follows: **Question → Method → Evidence → Conclusion → Decision**

## The 7 prototype exercises
| # | Question | Method | Evidence | What it tells us |
|---|----------|--------|----------|------------------|
| 1 | Can users create/edit common shapes, text, arrows, freehand? | Manual test in isolated prototype | Screenshots, notes | Basic drawing capability |
| 2 | Can users move, resize, select, undo/redo? | Manual test | Screen recording | Core editing quality |
| 3 | Do 3–4 clients see edit propagation and presence? | Multi-client session | Sync latency log | Realtime performance |
| 4 | What happens on same-object concurrent edits? | Two users edit the same object | Conflict behavior notes | Input for conflict policy |
| 5 | Does save/close/reopen persist correctly? | Persistence test | DB/asset verification | Durability |
| 6 | Do clients converge after disconnect/reconnect? | Network interruption test | Convergence log | Reliability |
| 7 | Custom UI effort, accessibility, export, bundle compatibility? | Code review + audit | Effort estimate, a11y report | Long-term fit |

Conclusion and Decision for all seven are still **(pending)** until prototypes run.

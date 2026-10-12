# 06 — Architectural Decisions (ADRs) and Risk Register

> Covers Sections 7 and 8. An ADR (Architecture Decision Record) is a written record of a significant decision.

## ADR-001: Editor Engine Selection
| Field | Content | Explanation |
|-------|---------|-------------|
| Status | Proposed | Not final yet |
| Context | Need a collaborative whiteboard editor; two viable stacks | The problem and circumstances |
| Options | 1) tldraw SDK + tldraw sync  2) Excalidraw + Yjs + WebSocket provider | The two paths |
| Decision | (pending prototype results) | Decided after prototyping |
| Consequences | License terms, self-hosting burden, customization effort, collaboration quality | What the choice affects |
| Evidence | Prototype exercise results (Section 4) | Supporting proof |

## ADR-002: tldraw Production Licensing
| Field | Content | Explanation |
|-------|---------|-------------|
| Status | Proposed | |
| Context | tldraw production requires a commercial/hobby license and valid key | |
| Options | 1) Purchase commercial license  2) Use hobby license (if eligible)  3) Reject tldraw | |
| Decision | (pending vendor discussion) | |
| Consequences | Cost, legal compliance, vendor lock-in | |
| Evidence | Vendor correspondence, license terms | |

## Risk register
| Risk | Likelihood | Impact | Mitigation | Explanation |
|------|-----------|--------|------------|-------------|
| tldraw license cost/terms unacceptable | Medium | High | Confirm before prototype commitment; keep Excalidraw as fallback | Plan B ready if cost/terms fail |
| Yjs provider dependency / vendor lock-in | Medium | Medium | Evaluate a self-hosted provider; document exit path | Avoid being stuck with one provider |
| Same-object concurrent edit conflicts | **High** | Medium | Define policy in Design stage; test in prototype | What happens when two people change one object |
| Scale beyond 3–4 users not validated | Medium | Medium | Explicitly scope MVP; measure latency targets | Behavior with larger groups is unknown |
| Asset storage cost/security | Low | Medium | Server-side auth, size limits, sanitization | Upload abuse and cost |
| Live Squid architecture differs from prototype repo | **High** | **High** | Defer integration; inspect live system before host contract design | Biggest risk: a wrong assumption |

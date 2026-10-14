## 7. Recommended technical architecture

This is the current recommended direction, not a claim that implementation compatibility has already been proven. There will be no comparison prototypes because time is limited; instead, small implementation gates should verify the riskiest assumptions early.

| Layer | Current recommendation | Responsibility |
|---|---|---|
| Application boundary | Standalone Whiteboard app inside the connected workspace. | Own the editor UI and product behavior; keep existing Squid product flows untouched. |
| Drawing editor | Excalidraw React package. | Provide the base drawing/editor toolkit behind a Whiteboard-owned adapter. |
| Shared document model | Yjs. | Shared data and concurrent synchronization; not a server or authorization system by itself. |
| Realtime server | One Hocuspocus server, one isolated room per live session. | WebSocket transport, Yjs sync, auth/lifecycle hooks, and connection handling. |
| Database | One MongoDB Atlas deployment. | Persist temporary session state, accepted operations, snapshots, invitations, and membership as needed. |
| Database admin tool | MongoDB Compass, optionally. | GUI to inspect Atlas; it is not another database and the app does not connect to Compass. |
| Squid integration | One future host adapter, only after separate approval. | Pass Squid user/workspace context and actions without spreading Squid-specific logic through Whiteboard. |

### Boundary sketch



**Connected workspace**

**Whiteboard app**

**Realtime room**

**MongoDB Atlas**

**Standalone development host**

**Canvas + XML + session UI**

Hocuspocus + Yjs per-session room Session records + ops/snapshots

### Data kept separate

- Durable content: objects, text, connectors, document title/version, and accepted operations.

- Transient presence: cursors, names/avatar status, selection hints, current tool, owner viewport, connection status. Presence is not saved as board history or XML.

- Local camera state: local user's viewport/zoom, except the owner's presented viewport is temporarily shared with Viewers in Presentation mode.

- V1 excludes image uploads and attachments; keep an asset-storage boundary for a later feature if needed.

## 8. MongoDB persistence and end lifecycle

Atlas is the database and Hocuspocus is the realtime server. These are different roles. Compass is an optional database inspection client. The recommended design uses temporary persistence so sessions can recover after a browser disconnect or realtime-process restart, then deletes session data when the owner closes the session.

### Suggested logical records

| Record | Purpose |
|---|---|
| Session | Owner, mode, title, state, timestamps, participant/seat state, current server sequence, and last presented viewport. |
| Invitation | Normalized email, fixed role, token hash, sent/expiry/accepted/cancelled timestamps. |
| Membership | Verified identity, role, removal/revocation status, and connection state as needed. |
| Accepted operations | Ordered semantic edits and undo/redo actions; recovery and global-history source. |
| Snapshot | Compact Yjs state plus the highest included operation sequence, used to speed recovery. |

### Save, recovery, and closure



- Live edits should be broadcast promptly; an edit should not be acknowledged as durably accepted until the persistence path can recover it.

- Use operation records as the durable recovery source between compacted snapshots. Debounced snapshot saving is an optimization, not the only durability promise.

- On restart/reconnect, reload the latest snapshot, replay later accepted operations, authenticate the user again, and synchronize queued offline edits according to the server-order conflict rules.

- If the database cannot accept edits, clearly show the degraded state and avoid claiming unsaved work has been saved. Exact temporary local behavior should be finalized in implementation design.

- End Session offers: Download latest XML and End; End without downloading; or Cancel. If the owner explicitly chooses no download, close the session. If a download is cancelled or fails, keep the session Active unless the owner explicitly chooses End without downloading.

- The XML should represent the latest server-accepted revision. After closure, disconnect members and immediately delete temporary session, invite, membership, operation, and snapshot data as decided.

## 9. Security, accessibility, and reliability requirements

Treat every browser message, XML file, invitation link, and room name as untrusted. UI states are not authorization controls.

### Security and privacy

- Authorize every room join and every edit on the server from verified identity, current session state, membership, role, and revocation status. A guessed room ID must not grant access.

- Bind invites to one email, session, and role; use single-use expiring tokens; store hashes, not reusable plaintext tokens.

- Viewer edit attempts, even if manually sent, must be rejected by the server. Removal revokes existing sockets and later reconnects.

- Validate XML size, version, structure, IDs, enum values, geometry and connector references. Parse as data; do not execute embedded markup, external references, or scripts.

- Set server-side limits for board/XML bytes, object count/complexity, message size, command rate, and invitation frequency. Tune practical thresholds from actual implementation measurements.

- Do not log board contents, XML, invite tokens, or session cookies. Monitor failures and timings without collecting drawing content.

### Keyboard and accessibility

- Keyboard-select tools; Tab through objects; arrow keys move a selected object and Shift+arrow moves farther; Delete/Backspace removes selected objects.

- Ctrl+Z/Ctrl+Y shared Undo/Redo; Ctrl+C/X/V, Ctrl+D, Ctrl+A; Ctrl+S for local XML save.

- Show visible focus and provide understandable keyboard access to canvas objects. Native canvas access can be a high-risk fit check for the chosen engine.

- Presentation mode must communicate roles and status without relying on color alone; focus and live connection/reconnect announcements need UX detail during implementation.

### Reliability boundaries

- One Hocuspocus server is a reasonable V1 starting point, but a single instance is a single point of availability. Clients must reconnect and recover after restart.

- Do not introduce Redis or multi-node scaling before measured demand requires it; scaling later needs room routing and cross-node consistency design.

- Six people per room is the confirmed product cap, not a proven total capacity across many rooms. Measure active rooms, writes, memory, and recovery behavior.

- Owner-only end means an abandoned session can remain active indefinitely. The product/operations team must set a support-assisted closure policy or accept and bound this risk before release.

## 10. Engine choice and license position

Prototypes have been ruled out because of time. The current planning direction is Excalidraw + Yjs with Hocuspocus, not tldraw + Yjs. This is a documentation-based recommendation and is not runtime validation.

| Option | What research established | Current position |
|---|---|---|
| tldraw SDK + tldraw Sync | Strong prebuilt editor and collaboration fit. Production use needs applicable license terms/key and may require paid approval. Do not assume Yjs is part of this pairing. Global Undo/Redo still needs product-specific behavior. | Not selected under the $0 constraint unless the organization confirms a suitable no-cost production license in writing. |
| Excalidraw + Yjs + Hocuspocus | Excalidraw React and Yjs are permissively licensed per researched package/docs. Excalidraw does not supply host-app collaboration. Hocuspocus provides a Yjs realtime server, but room security, roles, persistence adapter, product policy, and global history remain application work. | Preferred V1 direction to avoid tldraw's identified commercial-license gate. Verify exact versions and all dependency licenses before distribution. |

A permissive editor-library license does not make email, hosting, database capacity, backups, domains, or operations free. The $0 limit applies to the whole demo/deployment plan.

### Early technical fit gates



- Check Excalidraw against the required text-inside-shape behavior, transparent fills with grid occlusion, attached 90-degree connectors, resizing invariants, keyboard access, XML adapter, and custom palette.

- Validate that Hocuspocus can support the chosen authorization flow, authenticated room join, server-accepted sequencing, server-origin document updates, presence behavior and reconnect protocol in the exact selected version.

- Validate how Yjs state updates coexist with server order, offline replay and global Undo/Redo; do not assume CRDT semantics alone equal last accepted change wins.

- If a hard V1 requirement is blocked, stop early and reassess the engine/adapter before building the rest of the product around that assumption. This is an implementation fit gate, not a request to build two comparison prototypes.

## 11. Budget and demo deploymen\

The user has confirmed a zero budget. The first target can be developed and demonstrated locally. A public tunnel or free hosting may help show the demo, but it is not equivalent to a reliable production deployment.

- Do not enable paid overages or select a paid editor license, service, email quota, domain, or hosting plan without explicit approval.

- MongoDB Atlas free tiers have capacity, backup and operational limits; evaluate current terms and fit before using it for more than temporary demo sessions.

- A free deployment may sleep, restart, have quotas, or lose ephemeral resources. Make reconnect/session state visible and demonstrate within the known constraints.

- If no acceptable no-cost service can deliver a required capability (especially email delivery or externally reachable live server), use local/demo substitutes or pause public access rather than implying the capability is production-ready.

- Set clear demo boundaries: limited users, temporary data, no availability guarantee, no production-sensitive content.

## 12. Remaining research and decisions

The product behavior is mostly decided. Remaining work is concentrated in implementation-specific feasibility, operations, and risk policy. Some items can be decided while building; the blockers below should be settled before the corresponding capability is released publicly.

| Priority | Open item | Why it remains / decision needed |
|---|---|---|
| Early implementation gate | Exact Excalidraw fit and adapter strategy. | No prototypes will be built. Verify hard requirements in the first editor slice; reassess if blocked. |
| Early implementation gate | Yjs/Hocuspocus server-command design. | Decide authenticated command transport, server sequence, accepted writes, offline replay, shared undo/redo, and persistence ordering. |
| Before invitation release | Email delivery and verified identity. | Choose a no-cost provider/SMTP or local substitute; define accept-link proof, session lifetime, retries, rate limits, and abuse controls. |
| Before public demo | No-cost network/hosting arrangement. | Select local-only vs tunnel/free host; define origins, HTTPS, secrets, callbacks, and sleep/restart behavior. |
| Before live persistence | Atlas configuration and retention. | Choose cluster/region, network rules, credentials, backup expectations, TTL, and storage sizing under $0. |
| Before session release | Abandoned owner session policy. | Owner-only End can leave a session active indefinitely; choose support recovery, admin closure, or an approved retention limit. |
| After implementation measurements | Hard size/rate/concurrency thresholds. | Measure document size, object count, command size/rate, concurrent rooms, reconnect load, snapshots, and sync latency. |
| During implementation | Older XML schema migration details. | Define exact schema and migration transforms with the canonical model; preserve the versioning intent from V1. |
| During implementation | Error UX and failure edge cases. | Specify stale offline edits, rejected undo, invalid inverse, failed persistence, invite/end race, full room, and export failure behavior. |
| Before release | Dependency license inventory. | Record versions/notices for Excalidraw, Yjs, Hocuspocus, and transitive dependencies; package licenses do not cover hosted services. |

## 13. Development roadmap after planning



After the remaining planning gates are recorded, convert this sequence into initiatives, epics, features, stories, tasks, and subtasks. This is a roadmap, not yet a ticket list.

| Order | Workstream | Outcome |
|---:|---|---|
| 1 | Standalone app foundation | Separate Whiteboard app/config in the connected workspace; no Squid UI/backend integration; local development path. |
| 2 | Canonical document + XML | Versioned object schema, strict import validation, save/open, template records, unsaved-work warnings. |
| 3 | Single-user editor | Canvas, grid, tools, shape text/styles, connectors, selection/editing, keyboard access, local gesture history. |
| 4 | Email identity and invitations | Accept-by-email, verified identity, 10-minute expiry, fixed roles, secure session continuity. |
| 5 | Live session control | Mode selection, six seats, owner controls, invites/removal, lifecycle, server-side roles. |
| 6 | Realtime and conflict handling | Hocuspocus/Yjs, server order, latest-accepted conflict rule, presence, offline reconnect/replay. |
| 7 | Atlas durability and shared history | Operation log/snapshots/recovery, shared Undo/Redo, accepted-write durability, temporary-data cleanup. |
| 8 | Presentation mode | Viewer follow, Editor independent navigation, owner-disconnect state, permission enforcement. |
| 9 | Hardening and senior demo | Tested demo flow, clear limits, accessibility/security checks, failure behavior, demo script. |
| 10 | Future Squid integration | Only after approval: inspect live Squid and define host adapter, identity/workspace mapping, and product entry point. |

## 14. Research completion and readiness



Research is sufficiently complete to begin implementation planning and an isolated demonstration, because the goal and most V1 behavior have been decided. It is not a production-readiness sign-off. The no-prototype decision means editor/realtime compatibility must be verified through early implementation gates. Ready to do next: create a traceable development backlog and start the first standalone app slice in the connected workspace. Keep Squid integration out of scope. Before public live use, settle email identity, zero-cost hosting/Atlas limits, abandoned-session retention, and the realtime protocol details.

### Key risks to keep visible

- Custom requirements (shared Undo/Redo, accepted-order conflicts, Viewers following owner only, fixed invite roles, custom XML) add work beyond installing an editor package.

- Reconnect of offline edits is required but needs a careful operation replay/conflict policy to avoid silently losing changes.

- Temporary sessions still require durable storage until explicitly ended; temporary does not mean memory-only.

- Zero-budget demo services may be unsuitable for production or durable availability.

- Owner-only ending creates an abandoned-session risk that needs a retention/recovery policy.

## 15. Research references

These references were listed in the research notes used to prepare this consolidation. Recheck current versions, license text, and pricing at implementation time.

- [Excalidraw React package](https://github.com/excalidraw/excalidraw)
- [Excalidraw package license metadata](https://github.com/excalidraw/excalidraw/blob/master/packages/excalidraw/package.json)
- [Excalidraw collaboration FAQ](https://docs.excalidraw.com/docs/%40excalidraw/excalidraw/faq)
- [Excalidraw JSON schema](https://docs.excalidraw.com/docs/codebase/json-schema/)
- [Yjs overview](https://docs.yjs.dev/)
- [Yjs UndoManager](https://docs.yjs.dev/api/undo-manager)
- [Hocuspocus server hooks](https://tiptap.dev/docs/hocuspocus/server/hooks)
- [Hocuspocus persistence](https://tiptap.dev/docs/hocuspocus/guides/persistence)
- [Hocuspocus provider events and stateless messaging](https://tiptap.dev/docs/hocuspocus/provider/events)
- [Hocuspocus database extension](https://tiptap.dev/docs/hocuspocus/server/extensions/database)
- [MongoDB BSON/document limits](https://www.mongodb.com/docs/manual/reference/limits/)
- [MongoDB Atlas free cluster limitations](https://www.mongodb.com/docs/atlas/reference/free-shared-limitations/)
- [tldraw license (alternative considered)](https://tldraw.dev/community/license)
- [tldraw Sync (alternative considered)](https://tldraw.dev/docs/sync)
---


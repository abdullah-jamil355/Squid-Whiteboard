## Presentation and session owner controls

### WB-S29 - Let presentation Viewers follow the owner

**As a** Viewer, **I want** to see the area the owner is presenting, **so that** I can follow without changing the presentation view.

**Acceptance criteria**

- A Viewer sees the same canvas area and pan/zoom view that the owner presents.
- A Viewer cannot navigate to another canvas area or edit board objects.
- Viewer cursor is not shown.
- The server enforces Viewer restrictions; the client UI is not the security boundary.

**Status:** Confirmed.

### WB-S30 - Let presentation Editors work independently

**As a** presentation Editor, **I want** to navigate elsewhere and edit, **so that** I can contribute without changing what Viewers see.

**Acceptance criteria**

- An Editor can pan/zoom to another area and edit there.
- The Editor's viewport is independent of the owner's presentation viewport.
- Editor changes sync to the shared board and remain subject to the same authorization and conflict rules.

**Status:** Confirmed.

### WB-S31 - Keep the session active when the owner disconnects

**As a** participant, **I want** the session state to remain clear if the owner disconnects, **so that** I know whether the session is still active.

**Acceptance criteria**

- Owner disconnection does not end the session or transfer ownership.
- Viewers keep seeing the owner's last presented view and see an Owner disconnected status.
- Editors may keep editing and syncing while the owner is disconnected.
- When the owner reconnects, Viewers resume following the owner's view.
- Only the owner can end the session.

**Status:** Confirmed.

### WB-S32 - Manage participants during a live session

**As a** session owner, **I want** to invite, remove, and manage participants during the session, **so that** I can control who is present.

**Acceptance criteria**

- The owner can send new invitations after a session has started.
- The owner can remove a participant; their live access is revoked and connection closed immediately.
- A removed participant needs a new invitation to rejoin.
- Only the owner can rename or end the session.
- Presentation roles stay fixed for the session; changing role requires a new invitation with the intended role.

**Status:** Confirmed.

### WB-S33 - End a live session with or without an XML download

**As a** session owner, **I want** to download the board before ending or explicitly end without a download, **so that** I choose whether to keep a local copy.

**Acceptance criteria**

- Only the owner can initiate End Session.
- The owner can choose Download XML and End, End without downloading, or Cancel.
- Downloaded XML represents the latest server-accepted board revision.
- Choosing End without downloading closes the session without requiring an XML file.
- If the owner cancels the download or it fails, the session remains Active unless the owner explicitly chooses End without downloading.
- Successful closure notifies/disconnects participants and triggers temporary-data deletion.

**Status:** Confirmed. Exact latest-revision handshake is an implementation gate.

## Delivery, quality, and scope boundaries

### WB-S34 - Demonstrate Whiteboard within the $0 budge\

**Kind:** Enabler — delivery constraint.

**As a** project owner, **I want** a local or no-cost demo path, **so that** I can develop, test, and show the work to seniors without spending money.

**Acceptance criteria**

- Local development and demonstration do not require paid services or paid editor licensing.
- No automatic paid overages are enabled.
- Any free-tier or tunnel demo clearly communicates limits such as sleeping, restarts, quotas, missing backups, or temporary data.
- If an external email/realtime/hosting capability has no usable free option, development can use a local substitute or pause public access rather than incur a cost.
- A demo does not claim production availability, long-term storage, or production security review unless those are actually delivered.

**Status:** Zero budget confirmed. Exact no-cost provider/hosting selection is pending.

### WB-S35 - Keep the editor maintainable for future changes

**Kind:** Enabler — architecture quality.

**As a** product team, **I want** editor, document format, templates, and realtime integration behind Whiteboard-owned boundaries, **so that** future changes do not require rewriting unrelated product areas.

**Acceptance criteria**

- Whiteboard's XML/document model is not simply an undocumented dump of the editor vendor's internal records.
- Engine-specific data is contained behind an adapter boundary.
- Templates are represented as data using the Whiteboard document model.
- Replacing the engine or evolving the XML format has an explicit migration path.
- No Squid-specific identity or navigation assumptions are spread across editor/document code.

**Status:** Confirmed design goal; exact module boundaries are implementation design.

### WB-S36 - Integrate with Squid later through one interface (deferred)

**Kind:** Deferred integration — not standalone V1 work.

**As a** Squid user, **I want** to open Whiteboard from an approved Squid entry point, **so that** I can use it with Squid identity and workspace context.

**Acceptance criteria when this is separately approved**

- Inspect the live Squid architecture at that time; do not infer production interfaces from this current workspace.
- Squid communicates with Whiteboard through one defined host adapter/interface.
- The interface carries only the required user, workspace, permissions, theme/locale, and return-navigation context.
- Whiteboard remains a separate feature; Squid does not duplicate Whiteboard behavior across product screens.
- Squid membership and role authorization are enforced server-side.

**Status:** Deferred. Do not include this in the standalone V1 implementation until separately authorized.

## Pending decisions and implementation gates

These items do not change the confirmed story behavior. Resolve each before the corresponding capability is released:

- **Editor fit:** During the first editor slice, verify Excalidraw supports text inside shapes, attached 90-degree connectors, grid occlusion for transparent shapes, keyboard operation, and the Whiteboard XML adapter. No comparison prototypes will be built.
- **Realtime semantics:** Verify the selected Yjs/Hocuspocus versions support the needed authentication hooks, server-ordered accepted operations, offline replay, shared Undo/Redo, and persistence ordering.
- **Email/identity:** Select a no-cost email delivery approach; decide exact email proof, secure session duration, resend/rate limits, and failure behavior.
- **Atlas/deployment:** Select region/configuration and determine whether free-tier capacity, network rules, backup limits, and retention fit the $0 demo scope.
- **Abandoned sessions:** Define how a session is recovered/closed if its owner is permanently unavailable, or explicitly accept and limit the storage risk before public release.
- **Limits:** Measure and set safe maximum XML/document bytes, objects, message sizes/rates, active rooms, snapshot size, and reconnect load.
- **XML migrations:** Define the concrete schema and migration transforms during canonical model implementation; keep the file versioned from the first release.
- **Failure UX:** Specify stale offline edits, rejected Undo, persistence outage, full room, invite/removal/end races, and canceled/failed export behavior.
- **Licenses:** Record exact package versions and dependency notices. A permissive editor package license does not make hosted services free.

## Recommended implementation order

1. Standalone app boundary and local development path.
2. Whiteboard document model, versioned XML, import validation, save/open, and template data.
3. Single-user canvas/editor, tools, keyboard operations, and local undo/redo.
4. Email identity, invitations, session controls, roles, and six-seat handling.
5. Realtime collaboration, presence, accepted-order conflict resolution, and reconnect replay.
6. Atlas persistence, snapshots/recovery, shared Undo/Redo, and end-session cleanup.
7. Presentation follow/viewer behavior and owner-disconnect behavior.
8. Security/accessibility hardening and a clear senior-demo flow.
9. Squid integration only as a later, separate approved phase.

## Research references

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
- [tldraw license](https://tldraw.dev/community/license)
- [tldraw Sync](https://tldraw.dev/docs/sync)
---


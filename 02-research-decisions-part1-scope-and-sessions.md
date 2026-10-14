# Appendix A — Research, decisions, and remaining work (source record)

The following is the complete research record used for the handoff. Where it contains roadmap language that predates the complete story/task breakdown, use the planning update in Section 2.

# Whiteboard: Research, Decisions, and Remaining Work

**Consolidated planning record**\
**Prepared:** 13 October 2026\
**Status:** V1 product behavior is substantially defined; technical implementation and operational gates remain.

> **Current direction:** Build Whiteboard as an independent application in the currently connected workspace. Do not integrate it into Squid product screens or depend on Squid identity or infrastructure in this phase. Squid integration is a later, separately approved phase.

**Status**

**Meaning**

**Product planning**

V1 user-facing requirements are substantially defined and consolidated below.

**Technical direction**

Excalidraw + Yjs, Hocuspocus, and MongoDB Atlas are the preferred direction; some implementation-fit decisions still require an early technical gate.

**Budget**

Zero budget. Use no-cost/local options for development and demonstrations; do not assume free tiers are suitable for production.

**Research status**

Planning is mature enough to create an implementation backlog. Operational and implementation questions remain; they do not invalidate the confirmed product scope.

### How to read this documen\

'Confirmed' records a user decision. 'Recommended' records the current technical advice, not a proven implementation. 'Pending' identifies a decision or constraint that still needs a named owner, measurement, or approval. No prototype comparison is planned because of the time constraint; therefore technical fit must be checked during development rather than claimed as proven now.

## 1. Purpose and boundaries

Whiteboard is a collaborative visual workspace for drawing freeform ideas, diagrams, and flows, then presenting that work to others. It should be maintainable so that future updates can change the editor engine, data format, collaboration implementation, or templates without forcing changes across unrelated product areas.

- Develop Whiteboard in the currently connected workspace (the workspace the user called Nutshyll). Keep it isolated as its own application/feature boundary.

- Do not add Whiteboard entry points across Squid screens, connect it to Squid login, or use Squid backend services during this phase.

- Later, if integration is approved, connect through one defined host interface/adapter so Squid supplies context and allowed actions in one place.

- The current connected repository is the development workspace, not evidence that its existing Squid product backend is the Whiteboard backend. Keep existing Squid product code and Whiteboard-owned code separated.

- The immediate goal is a working, testable demonstration that can be shown to seniors. A demo is not a promise of production availability or free-tier reliability.

### Intended boundary

| Boundary | Owns |
|---|---|
| Whiteboard application | Canvas, tools, templates, XML files, session UI, and role-aware presentation experience. |
| Whiteboard server | Email identity/invitations, authorization, live rooms, ordered edits, session lifecycle, and persistence. |
| Squid (future only) | One integration adapter can later pass Squid user/workspace context and navigation behavior. No Squid dependency is required now. |

## 2. Confirmed V1 product scope



This section records product behavior, independent of which drawing engine or server library is selected.

### Canvas and interaction

- Use one large, pannable and zoomable canvas; V1 does not have multiple pages. A user can work in different areas of the same canvas.

- Show a light graph-paper style grid persistently. No grid show/hide control, grid snapping, or alignment guides. Grid must not show inside shapes, even when the shape has no fill.

- Desktop/laptop first; Chrome and Edge are the initial supported browsers.

- Provide zoom in/out, fit-to-screen, reset zoom, and Space+drag to pan.

### Core drawing tools and object behavior

- Pen, eraser, rectangle, circle, diamond, straight line/arrow, connector, text, and select tools.

- Select, move, resize, delete, copy, cut, paste, duplicate, and select all. Box selection allows moving a whole diagram together; a dedicated Group/Ungroup command is not required.

- Resizing preserves the object's type (a circle remains a circle); rotation is out of V1.

- Text can be placed inside normal shapes and template shapes, stays with its shape, wraps within it, and supports left/center/right alignment, font size, and bold.

- Use a predefined color palette for fill, border, arrows, text, and pen. Shapes can be transparent/no-fill or use a palette color. No custom color picker in V1.

- Three line weights: thin, medium, and thick. Eraser removes the touched portion of a pen stroke; touching a shape or text removes that entire object.

- Straight and 90-degree connectors. Endpoints attach to shape connection points and auto-adjust when a shape moves. Users do not manually drag a connector bend in V1.

- Keep Bring to Front and Send to Back.

### Templates

Only built-in templates are in V1. Choosing a template creates an editable copy; users can modify it for their needs.

| Category | V1 templates |
|---|---|
| Basic diagrams | Blank page, organization chart, entity relationship diagram, mind map, simple Kanban board. |
| Flow and process | Flowchart, cross-functional flowchart, workflow, data flow diagram. |
| UML | Sequence diagram, use case diagram, activity diagram. |

## 3. Local board files and XML



Normal mode is local-file based. It does not promise automatic cloud saving. The user explicitly saves an editable Whiteboard project file to their device.

- A new board is titled Untitled. The user can rename it; in normal mode only that local user owns the change.

- Save / Ctrl+S downloads a Whiteboard-owned XML project file. Open/import accepts only this product's XML format in V1; no draw.io XML import/export.

- XML is the editable project file, not just an image export. It needs a format/schema version so later releases can identify and migrate supported older files.

- Invalid, unsafe, or unsupported XML must be rejected without replacing the board currently open.

- Warn before navigating away from unsaved work. If opening another file while there are unsaved changes, ask whether to save or discard first.

- A live session may start from a blank board, a built-in template, or an imported Whiteboard XML file.

### Document format principles

| Include in XML | Keep out of XML |
|---|---|
| Version, title, canvas settings, template provenance, stable object IDs, object geometry, text/style, connector endpoints/bindings, and supported board content. | Live cursor/presence, online status, ephemeral selection, authentication secrets, invitation tokens, and transient connection state. |

## 4. Live collaboration and sessions



Live sessions are temporary server-backed workspaces. Live edits update participants immediately, and reconnect restores/synchronizes changes. When the owner ends a session, the owner can save the latest accepted state as XML or explicitly end without downloading.

### Session creation and limits

- At creation, the owner chooses Live Collaboration or Presentation. The mode cannot be switched mid-session; end and create another session to use a different mode.

- Maximum 6 concurrently connected people, including the owner. A pending invite does not reserve a seat. A valid invite can be accepted later if a seat becomes available before it expires.

- Owner chooses the live session title at creation. Only the owner can rename it.

- The session becomes Active when created, even when nobody else has accepted an invite. It stays Active until the owner ends it; owner disconnect alone does not end it.

- Owner can invite more people after the session starts and remove a participant. Removal immediately revokes access and closes their live connection; rejoining then requires a new invite.

### Live Collaboration mode

- Owner and all invited participants are Editors by default. They can edit and navigate independently.

- Show name, initials avatar, and live cursor for the owner and Editors. Presentation Viewers do not get a board cursor.

- Live changes are visible to other participants promptly. If a user disconnects, reconnecting should synchronize that user's offline changes and reconcile against the current board. The exact replay/conflict protocol remains a technical gate.

### Presentation mode

- Only the owner may start a Presentation session.

- For each invite, the owner assigns Editor or Viewer. The role is fixed for that session. To change it, cancel/revoke and re-invite with the intended role.

- Editors may edit and move to other canvas areas on their own. Viewers can only see the area and viewport the owner presents; they cannot navigate elsewhere, edit, or see a cursor.

- Viewers follow the owner's pan/zoom. When the owner disconnects, Viewers stay on the last presented view and everyone sees an owner-disconnected status. When the owner reconnects, following resumes.

- The server must enforce Viewer restrictions. Hiding controls in the browser is not sufficient authorization.

## 5. Invitations, identity, and access

Before future Squid integration, Whiteboard uses email-based invitations and identity. There is no public join link. Only people invited to a session can join.

- Owner searches/selects a registered Squid user only after future Squid integration; for the standalone V1, the owner enters an email address and sends an invitation.

- Invite email contains an Accept invitation action. The intended user opens it and accepts to join; a separate public sign-in or room link is not part of the invite flow.

- Invites expire 10 minutes after sending and can be cancelled by the owner. Single-use invite tokens should be unguessable and stored as hashes, not reusable plaintext.

- The invite is bound to the invited email and session. Email ownership must be verified to accept and to rejoin from another device. After acceptance, preserve a server-side identity/session; removal revokes it for that session.

- On acceptance, the person supplies a display name; show an initials avatar. Custom profile-photo upload is out of V1.

- Owner may invite during the session. If full, the invite remains pending until a seat opens or the invitation expires. Seat acquisition must be atomic so two acceptances cannot exceed six people.

- Voluntary leave does not remove the invitation/member; the user can rejoin while the session is active if a seat is free. A removed member must be re-invited. Still to choose: the email delivery provider, exact email verification/login mechanism, secure session duration, resend limits, and delivery-failure behavior. The user has approved the accept-by-email experience, not a specific vendor or identity implementation.

## 6. Concurrent edits and shared history

The product rule is that the later accepted edit wins when people make conflicting changes to the same object. 'Latest' means server acceptance order, not a device clock. Independent property changes should remain intact where possible.

- Define an ordered operation/command per board. Server validates role, assigns a sequence, applies the operation and broadcasts the accepted state.

- A completed gesture is one history entry: for example, a drag is one action and a pen stroke is one action, not one history step per pointer sample.

- Undo and Redo are shared across the session. An Editor's Undo reverses the most recent accepted board action; Redo restores the latest undone action. A new forward edit clears the redo branch.

- Concurrent Undo/Redo commands need deterministic server ordering. If an inverse is no longer safe because later edits removed or changed the target, reject it without corrupting the document and show a clear status.

- Yjs can merge concurrent document updates, but its CRDT behavior alone does not guarantee the user-requested last-accepted-change rule or global Undo/Redo. These semantics need an explicit application command/history layer.

- Reconnect should synchronize a participant's offline changes. The technical design must define how queued offline actions are authenticated, ordered, checked against newer state, and reported if they conflict.


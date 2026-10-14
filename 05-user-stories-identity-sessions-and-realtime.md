## Identity, invitations, and session access

### WB-S17 - Start a live session

**As a** board owner, **I want** to start a live session from a blank board, template, or imported XML, **so that** I can collaborate or present it.

**Acceptance criteria**

- The owner chooses Live Collaboration or Presentation when creating the session.
- The owner sets the live board's title at creation; only the owner can rename it.
- The mode cannot be switched while the session is active; the owner must end it and create a new session to use the other mode.
- The session becomes Active as soon as it is created, even if nobody else has accepted an invitation.
- The owner can start from a blank board, a built-in template, or an imported Whiteboard XML file.

**Status:** Confirmed.

### WB-S18 - Invite an Editor to Live Collaboration

**As a** session owner, **I want** to invite someone by email, **so that** they can join the live board as an Editor.

**Acceptance criteria**

- The owner invites by email; no public join link is created.
- Invitees in Live Collaboration receive Editor access by default.
- The invitation email has an Accept invitation action and is valid for 10 minutes.
- The owner can cancel a pending invitation, and can send invitations after the session has started.
- An invitation that has expired cannot be accepted.

**Status:** Confirmed. Email delivery vendor is pending.

### WB-S19 - Assign a presentation role when inviting

**As a** presentation owner, **I want** to choose Editor or Viewer for each invitee, **so that** I control their role for the session.

**Acceptance criteria**

- The owner must assign Editor or Viewer when sending each presentation invitation.
- The assigned role is fixed for that session.
- Changing an invitee's role requires cancelling/revoking the invitation or membership and issuing a new invitation with the intended role.
- Roles do not automatically carry into a later session.

**Status:** Confirmed.

### WB-S20 - Accept an invitation and keep a verified identity

**As an** invited participant, **I want** to accept the email invitation and establish my identity, **so that** I can join and reconnect as the invited person.

**Acceptance criteria**

- Only the person who can verify control of the invited email can accept the invitation.
- The participant supplies a display name; the UI shows an initials avatar. Custom profile photos are outside V1.
- The accepted invite is associated with a server-side identity/session, not merely a client-provided name or email.
- The participant can verify the same invited email and rejoin from another browser/device while the session is active, unless removed.
- A removed participant cannot rejoin using the old membership.
- No separate public room link is exposed.

**Status:** Product flow confirmed. Sign-in method, email provider, and secure session duration remain open.

### WB-S21 - Enforce the six-person session limi\

**As a** session owner, **I want** the session to have a six-person limit including me, **so that** the live group stays within the agreed V1 size.

**Acceptance criteria**

- No more than six participants can be connected at once, including the owner.
- A pending invitation does not reserve a seat.
- If the session is full, an unexpired invite remains eligible to join when a seat opens.
- Two invite acceptances competing for the last seat cannot both exceed the limit; the server makes the seat claim atomically.
- A voluntarily disconnected participant can rejoin if a seat is available; removal requires a new invitation.

**Status:** Confirmed. Atomic seat handling is an implementation gate.

### WB-S22 - Show live participants and cursors

**As a** participant, **I want** to see who is currently editing and where Editors are working, **so that** live collaboration is understandable.

**Acceptance criteria**

- Show connected participant names and initials avatars.
- Show cursors for the owner and Editors.
- Do not show a board cursor for a Viewer.
- Presence is transient and is not saved into the XML project file or durable board history.

**Status:** Confirmed.

### WB-S23 - Enforce permissions on the server

**As a** board owner, **I want** access restrictions to be checked by the server, **so that** a participant cannot gain access by changing browser controls or messages.

**Acceptance criteria**

- Every room join checks verified identity, session status, current membership, role, and removal/revocation status.
- Every edit command is authorized on the server; hiding a browser control alone does not grant security.
- A Viewer edit request is rejected even if submitted outside the normal UI.
- Room IDs and client-supplied roles/user IDs do not grant access.
- Removing a participant revokes their existing live connection and rejects later reconnects until a new invitation is accepted.

**Status:** Confirmed. Exact auth/session implementation is pending.

## Live edits, reconnect, persistence, and history

### WB-S24 - See live edits and resolve conflicting edits

**As an** Editor, **I want** my changes and other Editors' changes to appear live, **so that** we can work on the same board together.

**Acceptance criteria**

- An accepted board edit is sent to other connected participants in real time.
- When competing changes affect the same object/property, the latest server-accepted change is the final value; client clocks do not decide which edit is latest.
- Independent property changes are retained where the model can safely preserve them.
- Rejected or unpersisted changes are not shown as accepted/saved.

**Status:** Product rule confirmed. Server ordering and interaction with Yjs are implementation gates.

### WB-S25 - Synchronize queued changes after reconnec\

**As a** participant, **I want** my changes to synchronize after I reconnect, **so that** brief connectivity loss does not silently discard my work.

**Acceptance criteria**

- On reconnect, the client authenticates again and receives the latest board state.
- Changes made while disconnected are synchronized/reconciled with the current state.
- Replay follows server acceptance ordering and the same-object conflict rule.
- The user is told when an offline change cannot be applied safely; the application does not silently imply that it was saved.
- Presence/connection status distinguishes connected, reconnecting, and disconnected states.

**Status:** Reconnect synchronization confirmed. Exact offline queue/replay/conflict protocol is an implementation gate.

### WB-S26 - Use shared Undo and Redo in a live session

**As an** Editor, **I want** Undo and Redo to affect the shared board history, **so that** the group can correct and restore changes consistently.

**Acceptance criteria**

- An Editor's Undo reverses the latest accepted board action, including an action made by another Editor.
- Redo restores the latest undone action.
- One meaningful gesture, such as one drag or one pen stroke, is one history action.
- A new forward edit clears the shared Redo branch.
- Simultaneous Undo/Redo requests are processed in deterministic server order.
- If an undo inverse can no longer be applied safely, the command is rejected and the shared board remains valid.

**Status:** Confirmed. This needs a product-owned shared history/command layer; a CRDT's default undo alone is not sufficient.

### WB-S27 - Persist and recover an active live session

**As a** participant, **I want** active session data to survive a browser disconnect or server restart, **so that** I can reconnect to the current board before the owner ends it.

**Acceptance criteria**

- MongoDB Atlas is the current preferred database direction; Hocuspocus is the current preferred realtime server direction. They have separate responsibilities, and deployment fit remains to be verified.
- One Hocuspocus server can host isolated rooms, with one room per live session.
- The system can restore a snapshot and replay later accepted operations after a restart.
- The durable path does not acknowledge an edit as saved until it can be recovered.
- Database or persistence failures are visible; the UI does not claim failed writes are saved.
- MongoDB Compass is an optional admin GUI, not a second database or application dependency.

**Status:** Preferred direction confirmed. Atlas deployment, persistence adapter, snapshot timing, and zero-cost limits remain open.

### WB-S28 - Remove temporary data after a session ends

**As a** participant, **I want** a closed session to stop accepting access and clear its temporary server data, **so that** ended boards are not left active on the service.

**Acceptance criteria**

- Successful end disconnects participants and marks the session Closed.
- Temporary session, invitation, membership, operation, and snapshot data are deleted after closure, as required by the agreed lifecycle.
- Reusing an old room ID or invitation cannot reopen the closed session.
- Owner-only end and temporary data create an abandoned-session risk; a recovery/retention policy is documented before public release.

**Status:** Closure behavior confirmed. Abandoned-session policy remains open.


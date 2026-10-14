## Tasks for WB-S22 - Show live participants and cursors

### WB-S22-T01 - Define and broadcast transient presence

- [ ] WB-S22-T01.1 Define presence fields for identity display, role, cursor, selection hint, active tool, and connection state as needed.
- [ ] WB-S22-T01.2 Publish current presence to members of that session only.
- [ ] WB-S22-T01.3 Keep presence out of XML files and durable document history.

### WB-S22-T02 - Render avatars and role-appropriate cursors

- [ ] WB-S22-T02.1 Show connected names and initials avatars.
- [ ] WB-S22-T02.2 Show the owner/Editor cursors and do not render Viewer cursors.
- [ ] WB-S22-T02.3 Clear stale presence after disconnection/removal and expose reconnecting status.

## Tasks for WB-S23 - Enforce permissions on the server

### WB-S23-T01 - Authorize room connection and operations

- [ ] WB-S23-T01.1 Validate authenticated identity, session status, membership, role, seat, and revocation before room join.
- [ ] WB-S23-T01.2 Validate authorization for each edit/owner command at the server boundary.
- [ ] WB-S23-T01.3 Reject Viewer edit commands and unauthorized owner-only actions even if manually submitted.

### WB-S23-T02 - Revoke access immediately

- [ ] WB-S23-T02.1 Treat room IDs and client-supplied user/role fields as identifiers only, never credentials.
- [ ] WB-S23-T02.2 Close an active participant connection when owner removes them.
- [ ] WB-S23-T02.3 Reject future reconnects for removed members unless a new invitation is accepted.

## Tasks for WB-S24 - See live edits and resolve conflicting edits

### WB-S24-T01 - Define a server-ordered edit command

- [ ] WB-S24-T01.1 Define semantic edit commands with actor, session, target object/property, payload, and action ID.
- [ ] WB-S24-T01.2 Validate and assign a monotonically increasing server sequence to accepted changes.
- [ ] WB-S24-T01.3 Persist or otherwise durably record an edit before acknowledging it as accepted.

### WB-S24-T02 - Apply conflict policy and broadcast changes

- [ ] WB-S24-T02.1 Make the later server-accepted same-object/property change determine the final value.
- [ ] WB-S24-T02.2 Preserve independent property changes when safe.
- [ ] WB-S24-T02.3 Broadcast accepted changes promptly and return explicit rejected/degraded results when not accepted. **Gate:** validate how this command layer coexists with Yjs updates.

## Tasks for WB-S25 - Synchronize queued changes after reconnec\

### WB-S25-T01 - Track connection state and queue eligible offline edits

- [ ] WB-S25-T01.1 Show connected, reconnecting, and disconnected status.
- [ ] WB-S25-T01.2 Retain offline edits with stable IDs and their original operation intent for later replay.
- [ ] WB-S25-T01.3 Avoid showing offline edits as server-accepted or durably saved before confirmation.

### WB-S25-T02 - Authenticate, catch up, and reconcile on reconnec\

- [ ] WB-S25-T02.1 Reauthenticate and fetch the latest accepted board state/sequence.
- [ ] WB-S25-T02.2 Replay eligible queued operations in deterministic order through the server conflict/authorization path.
- [ ] WB-S25-T02.3 Surface operations that cannot be safely applied and preserve them for user recovery. **Gate:** settle offline replay semantics early.

## Tasks for WB-S26 - Use shared Undo and Redo in a live session

### WB-S26-T01 - Build global action history

- [ ] WB-S26-T01.1 Define one semantic history entry per completed meaningful gesture.
- [ ] WB-S26-T01.2 Store action ID, actor, server sequence, forward operation, inverse, and affected target/property.
- [ ] WB-S26-T01.3 Serialize Undo/Redo requests in server order and authorize them by current role.

### WB-S26-T02 - Implement shared Undo/Redo behavior

- [ ] WB-S26-T02.1 Allow an Editor to undo the latest accepted action from any participant.
- [ ] WB-S26-T02.2 Redo the latest undone action and clear Redo after a new forward edit.
- [ ] WB-S26-T02.3 Reject an unsafe inverse without mutating the shared board and show an understandable result.
- [ ] WB-S26-T02.4 Broadcast the resulting document change and history state to all participants.

## Tasks for WB-S27 - Persist and recover an active live session

### WB-S27-T01 - Add MongoDB Atlas persistence boundary

- [ ] WB-S27-T01.1 Define Whiteboard-owned persistence contracts for sessions, invitations, memberships, operations, and snapshots.
- [ ] WB-S27-T01.2 Implement the MongoDB Atlas adapter and configuration without connecting the app to MongoDB Compass.
- [ ] WB-S27-T01.3 Keep Atlas credentials server-side and document local environment setup without committing secrets.

### WB-S27-T02 - Persist accepted operations and snapshots

- [ ] WB-S27-T02.1 Write accepted semantic operations before acknowledging durable acceptance.
- [ ] WB-S27-T02.2 Store compact Yjs snapshots with the highest included operation sequence.
- [ ] WB-S27-T02.3 Define snapshot cadence and BSON/document-size behavior from measured board data.

### WB-S27-T03 - Recover sessions and surface persistence failures

- [ ] WB-S27-T03.1 Restore a session by loading its latest snapshot and replaying later operations.
- [ ] WB-S27-T03.2 Resume room sync after server restart and let clients reconnect.
- [ ] WB-S27-T03.3 Show database/persistence failure and never report failed writes as saved. **Gate:** choose the no-cost Atlas configuration and confirm its limits.

## Tasks for WB-S28 - Remove temporary data after a session ends

### WB-S28-T01 - Close sessions and invalidate access

- [ ] WB-S28-T01.1 Transition Active to Closed only after an authorized owner end flow.
- [ ] WB-S28-T01.2 Disconnect room members and invalidate invitation/session access on closure.
- [ ] WB-S28-T01.3 Reject old room IDs, invite tokens, and reconnect attempts after closure.

### WB-S28-T02 - Delete temporary board/session records

- [ ] WB-S28-T02.1 Delete session, invitation, membership, operation, and snapshot data after confirmed closure.
- [ ] WB-S28-T02.2 Ensure cleanup is retryable and observable if part of the deletion fails.
- [ ] WB-S28-T02.3 Define/document abandoned-session recovery or retention policy before public release. **Gate:** owner-only End otherwise permits indefinite active data.


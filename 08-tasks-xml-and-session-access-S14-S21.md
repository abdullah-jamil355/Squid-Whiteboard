## Tasks for WB-S14 - Save an editable XML project file

### WB-S14-T01 - Define the versioned Whiteboard XML contrac\

- [ ] WB-S14-T01.1 Define root metadata, format version, title, canvas settings, templates, objects, IDs, styles, and connector bindings.
- [ ] WB-S14-T01.2 Keep presence, credentials, invitation tokens, and transient viewport state out of the normal project XML.
- [ ] WB-S14-T01.3 Record an XML compatibility/versioning policy and initial schema version.

### WB-S14-T02 - Serialize and download the projec\

- [ ] WB-S14-T02.1 Serialize the canonical Whiteboard model into deterministic XML.
- [ ] WB-S14-T02.2 Add Save and Ctrl+S browser download behavior with a sensible filename.
- [ ] WB-S14-T02.3 Update local dirty/saved state only after a successful save/download initiation.

## Tasks for WB-S15 - Open a Whiteboard XML file

### WB-S15-T01 - Parse, validate, and migrate supported files

- [ ] WB-S15-T01.1 Parse XML as data with safe parser settings and a file-size limit.
- [ ] WB-S15-T01.2 Validate version, object types, IDs, field values, geometry, text limits, and connector references.
- [ ] WB-S15-T01.3 Add explicit migrations for supported older Whiteboard versions; reject unsupported future versions.

### WB-S15-T02 - Load files atomically and support live startup

- [ ] WB-S15-T02.1 Build a candidate board from validated data before replacing the current board.
- [ ] WB-S15-T02.2 On parse/validation failure, retain the currently open board and show a clear error.
- [ ] WB-S15-T02.3 Allow a validated imported board to be selected as the source when starting a live session.

## Tasks for WB-S16 - Warn about unsaved local changes

### WB-S16-T01 - Track dirty state and offer save/discard

- [ ] WB-S16-T01.1 Track edits since the last successful local XML save.
- [ ] WB-S16-T01.2 Before replacing the board, offer Save, Discard, or Cancel.
- [ ] WB-S16-T01.3 Ensure Cancel leaves the current document and selection intact.

### WB-S16-T02 - Guard navigation and close

- [ ] WB-S16-T02.1 Show the unsaved-changes warning before in-app navigation away.
- [ ] WB-S16-T02.2 Use the browser-supported unload warning where available.
- [ ] WB-S16-T02.3 Keep warning copy clear about whether a download/save succeeded.

## Tasks for WB-S17 - Start a live session

### WB-S17-T01 - Define session state and creation inputs

- [ ] WB-S17-T01.1 Define owner, title, session mode, source board, lifecycle state, and timestamps.
- [ ] WB-S17-T01.2 Add creation choices for Live Collaboration and Presentation; do not allow a mode change mid-session.
- [ ] WB-S17-T01.3 Accept blank, built-in template, or validated imported XML as the initial board.

### WB-S17-T02 - Implement owner creation flow

- [ ] WB-S17-T02.1 Add session title and mode selection UI before creation.
- [ ] WB-S17-T02.2 Create the session as Active immediately, even with no invitees.
- [ ] WB-S17-T02.3 Restrict session rename and end controls to the owner.

## Tasks for WB-S18 - Invite an Editor to Live Collaboration

### WB-S18-T01 - Define invitation data and lifecycle

- [ ] WB-S18-T01.1 Model invitation email, session, fixed role, token hash, sent time, expiry, cancellation, and acceptance state.
- [ ] WB-S18-T01.2 Set expiry to 10 minutes and make invitation acceptance single-use.
- [ ] WB-S18-T01.3 Keep invitation delivery behind a replaceable email adapter. **Gate:** choose a no-cost provider or local demo substitute.

### WB-S18-T02 - Build owner invite flow and email action

- [ ] WB-S18-T02.1 Add email entry and invitation status UI for session owner.
- [ ] WB-S18-T02.2 Send an Accept invitation link with no public room-join link.
- [ ] WB-S18-T02.3 Default Live Collaboration invite role to Editor; support cancellation, expiry, and invites sent after session start.

## Tasks for WB-S19 - Assign a presentation role when inviting

### WB-S19-T01 - Require role selection for presentation invitations

- [ ] WB-S19-T01.1 Add an explicit Editor/Viewer choice to the owner invite flow in Presentation mode.
- [ ] WB-S19-T01.2 Store the role with the invitation and accepted membership on the server.
- [ ] WB-S19-T01.3 Do not provide a default role that could be accepted accidentally.

### WB-S19-T02 - Keep role fixed for the session

- [ ] WB-S19-T02.1 Enforce the stored role on every session command and room connection.
- [ ] WB-S19-T02.2 Prevent role edits on an existing accepted invitation/membership.
- [ ] WB-S19-T02.3 Support role change only through cancel/revoke and a new invitation with the intended role.

## Tasks for WB-S20 - Accept an invitation and keep a verified identity

### WB-S20-T01 - Verify invited email ownership

- [ ] WB-S20-T01.1 Specify the accept-link proof and token verification flow. **Gate:** choose email identity mechanism and no-cost delivery path.
- [ ] WB-S20-T01.2 Bind the invitation to normalized email, session, role, expiry, and a one-time token hash.
- [ ] WB-S20-T01.3 Issue a secure server-side identity/session after successful acceptance; never trust client-provided email or role.

### WB-S20-T02 - Collect participant identity and support rejoin

- [ ] WB-S20-T02.1 Ask the participant for a display name and generate an initials avatar.
- [ ] WB-S20-T02.2 Associate accepted membership with the verified identity for the session.
- [ ] WB-S20-T02.3 Reverify the same email for another device/rejoin; block a removed member until a new invite is accepted.

## Tasks for WB-S21 - Enforce the six-person session limi\

### WB-S21-T01 - Implement atomic seat claims

- [ ] WB-S21-T01.1 Count owner and connected participants toward a maximum of six.
- [ ] WB-S21-T01.2 Atomically claim/release seats when acceptance, disconnect, removal, and reconnect race.
- [ ] WB-S21-T01.3 Reject the acceptance that loses a final-seat race without creating a seventh connection.

### WB-S21-T02 - Handle pending invitations and rejoin

- [ ] WB-S21-T02.1 Do not reserve seats for pending invitations.
- [ ] WB-S21-T02.2 Keep a valid invite eligible while full; allow acceptance when a seat opens before expiry.
- [ ] WB-S21-T02.3 Permit voluntary leavers to rejoin if invited, active, and a seat is available; require a new invite after removal.


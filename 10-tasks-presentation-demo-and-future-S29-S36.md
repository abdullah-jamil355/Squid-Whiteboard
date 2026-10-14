## Tasks for WB-S29 - Let presentation Viewers follow the owner

### WB-S29-T01 - Broadcast the owner's presentation viewpor\

- [ ] WB-S29-T01.1 Publish owner pan/zoom/camera state as transient presentation data.
- [ ] WB-S29-T01.2 Apply the owner's view to Viewers without adding it to XML or edit history.
- [ ] WB-S29-T01.3 Keep the latest presented viewport available during owner disconnection.

### WB-S29-T02 - Lock Viewer navigation and editing

- [ ] WB-S29-T02.1 Disable local pan/zoom/navigation actions that would leave the presented view.
- [ ] WB-S29-T02.2 Reject viewer edit/navigation commands on the server.
- [ ] WB-S29-T02.3 Keep Viewer cursor hidden and communicate the read-only role clearly.

## Tasks for WB-S30 - Let presentation Editors work independently

### WB-S30-T01 - Separate editor and presenter viewports

- [ ] WB-S30-T01.1 Keep each Editor's camera state local to that Editor.
- [ ] WB-S30-T01.2 Prevent Editor pan/zoom from changing the owner's Viewers' presentation viewport.
- [ ] WB-S30-T01.3 Continue receiving board edits/presence independently of the Editor's viewport.

### WB-S30-T02 - Permit Editor changes during presentation

- [ ] WB-S30-T02.1 Permit authorized object edits to presentation Editors on the server.
- [ ] WB-S30-T02.2 Sync Editor changes to the canonical shared board and other authorized participants.
- [ ] WB-S30-T02.3 Keep Viewer presentation and role restrictions intact while Editors navigate elsewhere.

## Tasks for WB-S31 - Keep the session active when the owner disconnects

### WB-S31-T01 - Maintain session state during owner disconnec\

- [ ] WB-S31-T01.1 Keep the session Active and retain the owner identity/ownership during disconnect.
- [ ] WB-S31-T01.2 Show an Owner disconnected status to participants.
- [ ] WB-S31-T01.3 Allow Editors to continue authorized editing/syncing while the owner is away.

### WB-S31-T02 - Resume presentation after owner reconnec\

- [ ] WB-S31-T02.1 Keep Viewers on the last owner viewport while disconnected.
- [ ] WB-S31-T02.2 Reauthenticate the owner and resume publishing their viewport after reconnect.
- [ ] WB-S31-T02.3 Do not auto-end or transfer the session because of a disconnect.

## Tasks for WB-S32 - Manage participants during a live session

### WB-S32-T01 - Add owner session-management controls

- [ ] WB-S32-T01.1 Show invite, participant list, role, connection state, and remove controls to the owner.
- [ ] WB-S32-T01.2 Allow additional invitations during an active session using the mode-specific role rules.
- [ ] WB-S32-T01.3 Allow only the owner to rename the session.

### WB-S32-T02 - Remove/revoke a participan\

- [ ] WB-S32-T02.1 Confirm the owner action where appropriate and record the membership revocation.
- [ ] WB-S32-T02.2 Disconnect the removed member immediately and remove their presence.
- [ ] WB-S32-T02.3 Require a new invitation for that person to rejoin.

## Tasks for WB-S33 - End a live session with or without an XML download

### WB-S33-T01 - Prepare and download the latest accepted board XML

- [ ] WB-S33-T01.1 Add owner-only End Session dialog with Download XML and End, End without downloading, and Cancel choices.
- [ ] WB-S33-T01.2 Export from a canonical accepted server revision, not an unconfirmed client-only state.
- [ ] WB-S33-T01.3 If newer accepted edits occur during export preparation, detect and offer an up-to-date export before closure.

### WB-S33-T02 - Close only after the owner's end choice

- [ ] WB-S33-T02.1 Close after successful download and owner confirmation.
- [ ] WB-S33-T02.2 Close immediately when the owner explicitly chooses End without downloading.
- [ ] WB-S33-T02.3 Keep the session Active when export is canceled or fails unless the owner chooses End without downloading.
- [ ] WB-S33-T02.4 Notify/disconnect participants and trigger WB-S28 cleanup after closure.

## Tasks for WB-S34 - Demonstrate Whiteboard within the $0 budge\

### WB-S34-T01 - Define local demo setup

- [ ] WB-S34-T01.1 Document local app/server/database setup and required environment variables.
- [ ] WB-S34-T01.2 Provide a local/demo email substitute if a usable free email service is unavailable.
- [ ] WB-S34-T01.3 Keep secrets out of source control and avoid paid dependencies/overages.

### WB-S34-T02 - Define honest demo/deployment boundaries

- [ ] WB-S34-T02.1 Document demo limits such as service sleep, restarts, free quotas, backup limitations, and temporary data.
- [ ] WB-S34-T02.2 Ensure the demo UI/status does not imply production availability or durable retention.
- [ ] WB-S34-T02.3 Prepare a repeatable senior-demo flow using only available no-cost infrastructure.

## Tasks for WB-S35 - Keep the editor maintainable for future changes

### WB-S35-T01 - Define Whiteboard-owned document and editor boundaries

- [ ] WB-S35-T01.1 Define a canonical Whiteboard model that is distinct from editor-vendor storage records.
- [ ] WB-S35-T01.2 Keep the editor engine behind a narrow adapter for creating, reading, updating, and rendering objects.
- [ ] WB-S35-T01.3 Keep template definitions as data in the canonical model.

### WB-S35-T02 - Preserve a safe change/migration path

- [ ] WB-S35-T02.1 Add XML version identifiers from the first saved file version.
- [ ] WB-S35-T02.2 Define migration registration/dispatch for supported older XML versions.
- [ ] WB-S35-T02.3 Keep Squid identity/navigation out of the core editor and document model.

## Tasks for WB-S36 - Integrate with Squid later through one interface (deferred)

> **Deferred:** Do not execute these tasks during standalone V1. Start only after separate approval to integrate Whiteboard with Squid.

### WB-S36-T01 - Inspect the live Squid integration boundary (deferred)

- [ ] WB-S36-T01.1 Inspect the then-current live Squid codebase and architecture rather than assuming this workspace's current backend is the production interface.
- [ ] WB-S36-T01.2 Identify Squid identity, workspace membership, navigation, permissions, deployment, and security boundaries.
- [ ] WB-S36-T01.3 Record integration constraints and get approval for the chosen interface before connecting systems.

### WB-S36-T02 - Implement one Squid host adapter (deferred)

- [ ] WB-S36-T02.1 Define the minimal host contract for user/workspace context, permissions, theme/locale, and return navigation.
- [ ] WB-S36-T02.2 Implement Squid-specific mapping only inside the host adapter.
- [ ] WB-S36-T02.3 Enforce membership and allowed actions server-side without duplicating Whiteboard behavior across Squid screens.


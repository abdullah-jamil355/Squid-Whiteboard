# Appendix C — Complete V1 tasks and subtasks

All 36 story sections, 75 tasks, 227 subtasks, cross-story security checks, gates, and completion sequence are included below. Task completion checklists are planning items and remain unchecked until implemented and validated.
# Whiteboard V1 Tasks and Subtasks

- **Prepared:** 13 October 2026
- **Reviewed:** 14 October 2026
- **Stories:** [WHITEBOARD_USER_STORIES.md](./WHITEBOARD_USER_STORIES.md)
- **Scope:** Tasks and subtasks mapped to the 36 Story IDs. Story descriptions and story acceptance criteria remain in the separate stories file.
- **Current implementation target:** the connected workspace, with Whiteboard isolated from Squid's current screens, login, and backend.
- **Budget:** $0. Do not select paid services or enable paid overages.

**Separation:** This file contains the task/subtask breakdown and cross-story security checks. Each WB-Sxx section uses the story's ID and exact name; it does not copy the story description or acceptance criteria.

## How to use this breakdown

- A **task** is a deliverable: the capability or boundary that should exist when its work is done.
- A **subtask** is one concrete implementation step toward that deliverable. Complete the numbered subtasks from top to bottom unless a technical gate below says to decide something first.
- Before starting a task, read its story and acceptance criteria. They define the user-visible result; the task heading tells you what to build, and each subtask tells you the next action.
- Mark a subtask complete only after the described change exists and its behavior can be observed. Mark its task complete when all subtasks are done and the task outcome meets the relevant story acceptance criteria. Mark the story complete only after all its acceptance criteria pass.
- Task and subtask IDs are stable references and include the story ID.
- Items labelled **Gate** are technical feasibility decisions to validate during the first relevant implementation slice. They do not call for building engine comparison prototypes.
- Items labelled **Deferred** belong to WB-S36 and must not be implemented until Squid integration is separately approved.
- Story acceptance criteria remain authoritative. If a task appears to conflict with the story, follow the story and update this breakdown. Resolve a technical gate before work that depends on it; an open gate must not block unrelated local-board work.

### Example: how to read a task

For story `WB-S01 - Open Whiteboard independently`, this file lists only its tasks. `WB-S01-T01 - Establish the standalone app boundary` is one task. Its subtasks mean: (1) give Whiteboard its own workspace/app configuration and only the packages it needs, (2) create its own route and page layout, and (3) check that Whiteboard does not import or depend on Squid's app or API. The story's user need and story-level acceptance criteria are in the separate stories file. Each numbered subtask here is a work step, not another story.

### Terms used in the steps

- **Canonical Whiteboard model:** the app's own agreed data shape for boards, shapes, text, connectors, and styles. It is the source representation that XML save/load and the editor adapter use; it is not a vendor's private storage format.
- **Adapter:** a small boundary that translates between Whiteboard's model and a library or service. It keeps vendor-specific details out of the rest of the feature.
- **Transient/presence state:** temporary live information such as cursor position, who is connected, and the presenter's current view. It is not saved in a normal XML project or permanent edit history.
- **Server-accepted edit:** a change that the server has authorized, ordered, and accepted. A local/offline change is not accepted until the server confirms it.
- **Snapshot and operation:** a snapshot is a saved copy of the board at a point in time; an operation is one accepted change after that point. Recovery loads a snapshot and reapplies later operations.
- **Gate:** a decision or feasibility check that must happen before depending on that technical behavior. The gate does not mean build two prototypes; the research decision already ruled out comparison prototypes.

---

## Tasks for WB-S01 - Open Whiteboard independently

### WB-S01-T01 - Establish the standalone app boundary

- [ ] WB-S01-T01.1 Add a Whiteboard-owned app/workspace manifest and only the dependencies needed by this app.
- [ ] WB-S01-T01.2 Add a standalone route, layout, and app-level styling/configuration.
- [ ] WB-S01-T01.3 Keep imports and runtime dependencies out of the existing Squid app and API.

### WB-S01-T02 - Provide an independent local run path

- [ ] WB-S01-T02.1 Add local development/build commands using the repo's workspace control plane conventions.
- [ ] WB-S01-T02.2 Document the app's local URL, environment variables, and required services.
- [ ] WB-S01-T02.3 Confirm the app can render without Squid login, Squid API, or a Squid workspace.

## Tasks for WB-S02 - Create a blank board

### WB-S02-T01 - Define blank-board defaults

- [ ] WB-S02-T01.1 Define the Whiteboard-owned board state and one-canvas default.
- [ ] WB-S02-T01.2 Initialize a new board with the title `Untitled` and no server persistence.
- [ ] WB-S02-T01.3 Track local dirty/saved state independently of any live session.

### WB-S02-T02 - Add board creation and rename UI

- [ ] WB-S02-T02.1 Add a clear action to create/open a blank board.
- [ ] WB-S02-T02.2 Allow the user to rename the local board and reflect the title in the UI.
- [ ] WB-S02-T02.3 Keep normal-mode rename local until the user explicitly saves the XML file.

## Tasks for WB-S03 - Start from a built-in template

### WB-S03-T01 - Define the template data contract and catalog

- [ ] WB-S03-T01.1 Define a template format using Whiteboard-owned document data, not vendor-specific persisted records.
- [ ] WB-S03-T01.2 Add blank, organization chart, ERD, mind map, and simple Kanban templates.
- [ ] WB-S03-T01.3 Add flowchart, cross-functional flowchart, workflow, data flow, UML sequence, UML use case, and UML activity templates.

### WB-S03-T02 - Add template selection and editable copies

- [ ] WB-S03-T02.1 Show the built-in template catalog with understandable names and previews/descriptions.
- [ ] WB-S03-T02.2 Instantiate a new board by copying the selected template and assigning fresh object IDs.
- [ ] WB-S03-T02.3 Ensure changes to a board created from a template never mutate the catalog template.

## Tasks for WB-S04 - Draw freehand marks

### WB-S04-T01 - Implement the pen interaction

- [ ] WB-S04-T01.1 Add pen to the tool model and make it selectable from the toolbar.
- [ ] WB-S04-T01.2 Convert pointer input into a freehand stroke in canvas coordinates.
- [ ] WB-S04-T01.3 Apply the selected color and thin/medium/thick line weight to new strokes.

### WB-S04-T02 - Commit a stroke as one edi\

- [ ] WB-S04-T02.1 Buffer pointer samples while the user is drawing.
- [ ] WB-S04-T02.2 Commit one stroke object/action when the gesture completes.
- [ ] WB-S04-T02.3 Avoid emitting one undo/history action for every pointer sample.

## Tasks for WB-S05 - Add and resize basic shapes

### WB-S05-T01 - Add rectangle, circle, and diamond tools

- [ ] WB-S05-T01.1 Add the three shape kinds to the canonical object model and toolbar.
- [ ] WB-S05-T01.2 Create a shape from a canvas gesture with sensible initial bounds.
- [ ] WB-S05-T01.3 Render each shape with stable identity and editable bounds.

### WB-S05-T02 - Resize without changing shape type

- [ ] WB-S05-T02.1 Add selection handles and resize behavior for each supported shape.
- [ ] WB-S05-T02.2 Preserve the shape kind while updating its dimensions.
- [ ] WB-S05-T02.3 Constrain circle geometry so resizing does not turn it into an ellipse.

## Tasks for WB-S06 - Add text and write inside shapes

### WB-S06-T01 - Support standalone and shape-bound tex\

- [ ] WB-S06-T01.1 Define text objects and explicit parent-shape binding in Whiteboard document data.
- [ ] WB-S06-T01.2 Add text on the canvas and text-edit behavior inside an existing shape.
- [ ] WB-S06-T01.3 Move and resize bound text with its shape and template instance.

### WB-S06-T02 - Add text layout and formatting

- [ ] WB-S06-T02.1 Implement wrapping within the containing shape bounds.
- [ ] WB-S06-T02.2 Add left/center/right alignment, font-size, and bold controls.
- [ ] WB-S06-T02.3 Keep text edits separate from shape kind and shape style updates. **Gate:** confirm behavior with the selected engine/adapter.

## Tasks for WB-S07 - Style shapes, text, and lines

### WB-S07-T01 - Implement palette and line-weight state

- [ ] WB-S07-T01.1 Define the predefined palette centrally and make it available to tools and object properties.
- [ ] WB-S07-T01.2 Apply palette colors to fills, outlines, connectors/arrows, text, and pen strokes.
- [ ] WB-S07-T01.3 Add thin, medium, and thick line weights; do not add a custom color picker.

### WB-S07-T02 - Add transparent fills and grid occlusion

- [ ] WB-S07-T02.1 Represent no-fill/transparent as an explicit shape style.
- [ ] WB-S07-T02.2 Ensure the grid is masked inside filled and no-fill shapes according to the product rule.
- [ ] WB-S07-T02.3 Preserve shape identity when colors or fill are changed. **Gate:** validate rendering and export behavior with the editor adapter.

## Tasks for WB-S08 - Draw lines, arrows, and attached connectors

### WB-S08-T01 - Add straight lines and arrows

- [ ] WB-S08-T01.1 Add straight-line and arrow tools and object types.
- [ ] WB-S08-T01.2 Add endpoint and stroke styling from the shared palette/line-weight model.
- [ ] WB-S08-T01.3 Persist line and arrow geometry in Whiteboard document data.

### WB-S08-T02 - Add attached 90-degree connectors

- [ ] WB-S08-T02.1 Add a connector tool that can attach either endpoint to a shape connection point.
- [ ] WB-S08-T02.2 Recompute connector route when a bound shape moves or resizes.
- [ ] WB-S08-T02.3 Keep bend editing out of V1. **Gate:** verify the selected engine can support the required binding/routing or identify the smallest adapter-owned solution.

## Tasks for WB-S09 - Erase strokes and objects

### WB-S09-T01 - Implement stroke-segment erasing

- [ ] WB-S09-T01.1 Detect which stroke segments intersect the eraser path.
- [ ] WB-S09-T01.2 Split/remove only the touched portions and keep untouched stroke portions.
- [ ] WB-S09-T01.3 Record the completed erase gesture as one local edit action.

### WB-S09-T02 - Implement object erasing

- [ ] WB-S09-T02.1 Detect shape/text objects touched by the eraser.
- [ ] WB-S09-T02.2 Remove the whole touched shape or text object, including its bound text where appropriate.
- [ ] WB-S09-T02.3 Avoid deleting nearby objects that the eraser did not touch.

## Tasks for WB-S10 - Select, move, and manage objects

### WB-S10-T01 - Implement object selection and transformations

- [ ] WB-S10-T01.1 Add click selection, box selection, multi-selection, and Select All.
- [ ] WB-S10-T01.2 Add move, resize, and delete operations for supported objects.
- [ ] WB-S10-T01.3 Preserve shape/text and shape/connector bindings during selection and movement.

### WB-S10-T02 - Implement clipboard and duplicate operations

- [ ] WB-S10-T02.1 Add copy, cut, paste, and duplicate with new stable IDs for pasted copies.
- [ ] WB-S10-T02.2 Preserve relationships between objects copied together, including bound text/connectors.
- [ ] WB-S10-T02.3 Add keyboard shortcuts and select-all behavior consistent with WB-S12.

### WB-S10-T03 - Add stacking order controls

- [ ] WB-S10-T03.1 Store or derive a stable object z-order.
- [ ] WB-S10-T03.2 Add Bring to Front and Send to Back actions.
- [ ] WB-S10-T03.3 Persist stacking order in XML and shared session updates.

## Tasks for WB-S11 - Pan and zoom the canvas

### WB-S11-T01 - Implement camera navigation

- [ ] WB-S11-T01.1 Define canvas viewport/zoom state separately from board content.
- [ ] WB-S11-T01.2 Add zoom in/out, fit-to-screen, reset zoom, and Space+drag pan.
- [ ] WB-S11-T01.3 Keep tool gestures and pan gestures from interfering with each other.

### WB-S11-T02 - Render the graph-paper background

- [ ] WB-S11-T02.1 Add persistent light graph-paper lines that remain stable during pan/zoom.
- [ ] WB-S11-T02.2 Do not add a grid toggle, grid snapping, or alignment guides.
- [ ] WB-S11-T02.3 Ensure shapes visually occlude the grid, including no-fill shapes, per WB-S07.

## Tasks for WB-S12 - Use the editor with a keyboard

### WB-S12-T01 - Add keyboard focus and object navigation

- [ ] WB-S12-T01.1 Make toolbar tools and interactive canvas objects reachable through keyboard focus.
- [ ] WB-S12-T01.2 Implement Tab navigation across objects and visible focus indicators.
- [ ] WB-S12-T01.3 Provide a semantic object list or equivalent accessible route if the canvas engine does not expose objects accessibly. **Gate:** verify with the selected editor.

### WB-S12-T02 - Add keyboard object actions

- [ ] WB-S12-T02.1 Map arrow keys and Shift+arrow to selected-object movement.
- [ ] WB-S12-T02.2 Map Delete/Backspace, Ctrl+A, Ctrl+C/X/V, and Ctrl+D to object actions.
- [ ] WB-S12-T02.3 Map Ctrl+Z/Y and Ctrl+S to the local/live history and save behaviors.

### WB-S12-T03 - Make status and focus understandable

- [ ] WB-S12-T03.1 Announce relevant save, invitation, permission, and connection state changes to assistive technology.
- [ ] WB-S12-T03.2 Communicate roles/errors/status without color as the only signal.
- [ ] WB-S12-T03.3 Check desktop Chrome and Edge keyboard flows against the story's acceptance criteria.

## Tasks for WB-S13 - Undo and redo in a local board

### WB-S13-T01 - Implement local gesture history

- [ ] WB-S13-T01.1 Define reversible local editor actions and their inverse data.
- [ ] WB-S13-T01.2 Group one completed drag or stroke into one history entry.
- [ ] WB-S13-T01.3 Keep local history isolated from any live-session history.

### WB-S13-T02 - Wire Undo/Redo and branch behavior

- [ ] WB-S13-T02.1 Add Undo/Redo controls and Ctrl+Z/Ctrl+Y behavior.
- [ ] WB-S13-T02.2 Restore board state after Undo and Redo without changing unrelated objects.
- [ ] WB-S13-T02.3 Clear the local Redo branch when a new forward edit is committed.


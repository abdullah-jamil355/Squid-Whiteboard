# Appendix B — Complete V1 user stories

The reviewed story source is reproduced here so that no separate story file is needed to understand requirements.
# Whiteboard V1 User Stories

**Prepared:** 13 October 2026
**Reviewed:** 14 October 2026
**Scope:** Standalone Whiteboard developed in the currently connected workspace. It is not integrated into Squid screens, login, or backend in this phase.\
**Budget constraint:** $0.\
**Planning note:** This is a direct story backlog. Initiative/epic structure is intentionally omitted so work can be planned directly from stories. Acceptance criteria capture confirmed product decisions; technical gates are called out where implementation details remain open.

## Story status key

Statuses describe decision state; story kinds distinguish product capabilities from enabling work.

- **Confirmed:** user-facing behavior was agreed in planning.
- **Implementation gate:** the behavior is agreed, but a technical approach needs validating during development. No engine comparison prototypes are planned.
- **Deferred:** explicitly outside the current standalone V1 scope.

**Story kinds**

- **User capability:** a user-facing V1 behavior.
- **Enabler:** delivery or architecture work needed to build or demonstrate V1.
- **Deferred integration:** future work excluded from standalone V1.

---

## Standalone workspace and board creation

### WB-S01 - Open Whiteboard independently

**As a** Whiteboard user, **I want** to open Whiteboard as its own application, **so that** I can use it without navigating through Squid product screens.

**Acceptance criteria**

- Whiteboard has its own application entry point in the currently connected workspace.
- Opening and using the Whiteboard does not require Squid login, Squid backend services, or a Squid workspace.
- Whiteboard code and configuration have a clear boundary from existing Squid product flows.
- A future Squid connection is possible through one defined host interface, but that interface is not wired into Squid in this story.

**Status:** Confirmed.

### WB-S02 - Create a blank board

**As a** user, **I want** to open a blank board, **so that** I can start drawing without a template.

**Acceptance criteria**

- A blank board opens with the title `Untitled`.
- The user can rename a normal/local board.
- The board contains one large canvas, not multiple pages.
- Starting a normal board does not create server-side persistent storage.

**Status:** Confirmed.

### WB-S03 - Start from a built-in template

**As a** user, **I want** built-in diagram templates, **so that** I can begin with a useful structure and edit it for my needs.

**Acceptance criteria**

- The built-in catalog contains: blank page, organization chart, entity relationship diagram, mind map, simple Kanban board, flowchart, cross-functional flowchart, workflow, data flow diagram, UML sequence, UML use case, and UML activity diagrams.
- Choosing a template creates a separate editable copy on the user's board.
- The original built-in template remains unchanged when a user edits their copy.
- V1 does not include user-created template upload or draw.io template import.

**Status:** Confirmed.

## Canvas, drawing, and editing

### WB-S04 - Draw freehand marks

**As a** user, **I want** a pen tool, **so that** I can sketch freely on the canvas.

**Acceptance criteria**

- The user can choose the pen, draw a freehand stroke, and see it on the board.
- Pen strokes use the selected palette color and one of the three line weights: thin, medium, or thick.
- A completed stroke is treated as one undoable gesture, not a separate action for each pointer sample.

**Status:** Confirmed. Shared live gesture history is specified in WB-S26.

### WB-S05 - Add and resize basic shapes

**As a** user, **I want** rectangle, circle, and diamond tools, **so that** I can build common diagrams.

**Acceptance criteria**

- The user can place a rectangle, circle, or diamond on the board.
- The user can resize each shape.
- Resizing does not change the shape type; a circle remains a circle.
- Rotation is not required in V1.

**Status:** Confirmed.

### WB-S06 - Add text and write inside shapes

**As a** user, **I want** to place text on the canvas or inside a shape, **so that** my diagrams explain themselves.

**Acceptance criteria**

- The user can add standalone text and text inside ordinary or template shapes.
- Text stays associated with and moves with its containing shape.
- Text wraps within the shape and supports left, center, and right alignment, font size, and bold.
- Editing text does not replace or alter the containing shape type.

**Status:** Confirmed. Exact editor-engine behavior is an implementation gate.

### WB-S07 - Style shapes, text, and lines

**As a** user, **I want** a predefined palette and transparent fill, **so that** I can make diagrams readable without needing a custom color picker.

**Acceptance criteria**

- A predefined palette can be applied to shape fills, outlines, arrows/connectors, text, and pen strokes.
- A shape can have either a selected palette fill or no fill.
- Line weight can be thin, medium, or thick.
- There is no custom color picker in V1.
- The graph-paper grid is not visible through or inside a shape, including a no-fill shape.
- Shapes keep their selected type while their style changes.

**Status:** Confirmed. Grid occlusion with no-fill shapes is an implementation gate.

### WB-S08 - Draw lines, arrows, and attached connectors

**As a** user, **I want** straight and 90-degree connectors, **so that** I can show relationships and flow between objects.

**Acceptance criteria**

- The user can draw a straight line, an arrow, and a 90-degree connector.
- Connector endpoints can attach to connection points on shapes.
- When an attached shape moves, the connector remains connected and automatically adjusts.
- Users cannot manually drag the 90-degree connector's bend in V1.

**Status:** Confirmed. Required routing and binding behavior is an implementation gate.

### WB-S09 - Erase strokes and objects

**As a** user, **I want** an eraser, **so that** I can remove unwanted drawing content.

**Acceptance criteria**

- Erasing a freehand stroke removes the touched portion of that stroke.
- Erasing a shape or text removes that whole object.
- Erasing does not unexpectedly delete nearby objects that were not touched.

**Status:** Confirmed.

### WB-S10 - Select, move, and manage objects

**As a** user, **I want** to select and manipulate objects, **so that** I can rearrange a diagram after drawing it.

**Acceptance criteria**

- The user can select, move, resize, and delete individual objects.
- The user can copy, cut, paste, duplicate, and select all objects.
- Box-selecting a whole diagram allows the user to move that selection together; a separate Group/Ungroup feature is not required.
- Text inside a shape moves with its shape.
- The user can use Bring to Front and Send to Back.

**Status:** Confirmed.

### WB-S11 - Pan and zoom the canvas

**As a** user, **I want** to move around and zoom the canvas, **so that** I can work on a large diagram comfortably.

**Acceptance criteria**

- The canvas can be panned and zoomed.
- The user can zoom in, zoom out, fit the board to screen, and reset zoom.
- Space+drag pans the canvas.
- A light graph-paper grid is always visible in the canvas background.
- There is no grid show/hide control, grid snapping, or alignment guide in V1.
- The grid does not show inside shapes.

**Status:** Confirmed.

### WB-S12 - Use the editor with a keyboard

**As a** keyboard user, **I want** to operate core tools and objects without relying only on a mouse, **so that** I can edit a board accessibly.

**Acceptance criteria**

- Keyboard focus can reach tools and board objects, and the focused item has a visible focus indicator.
- Tab can move through board objects.
- Arrow keys move the selected object; Shift+arrow moves it farther.
- Delete and Backspace remove the selected object.
- Ctrl+A selects all; Ctrl+C/X/V copy, cut, and paste; Ctrl+D duplicates.
- Ctrl+Z and Ctrl+Y invoke Undo and Redo; Ctrl+S invokes local XML save.
- Core status is not communicated by color alone.

**Status:** Confirmed. Canvas accessibility and browser behavior are implementation gates.

### WB-S13 - Undo and redo in a local board

**As a** user, **I want** to undo and redo local edits, **so that** I can recover from mistakes while working alone.

**Acceptance criteria**

- Ctrl+Z reverses the latest local edit and Ctrl+Y reapplies an undone edit.
- One completed gesture, such as one drag or one pen stroke, is one history action.
- A new edit after Undo clears the Redo branch.
- Local board history is not shared with another user's board.

**Status:** Confirmed. Live shared history is specified separately in WB-S26.

## Local file save and open

### WB-S14 - Save an editable XML project file

**As a** user, **I want** to save my normal board as an XML file on my device, **so that** I can reopen and continue my work later.

**Acceptance criteria**

- Save and Ctrl+S download an editable Whiteboard-owned XML project file.
- The saved file contains the board content needed to continue editing, not only a flat image.
- The file includes a format/schema version and stable object identifiers.
- Normal-mode saving is explicit; the application does not claim that the board has been automatically saved to the cloud.

**Status:** Confirmed. The exact XML schema is defined during implementation.

### WB-S15 - Open a Whiteboard XML file

**As a** user, **I want** to reopen a Whiteboard XML project file, **so that** I can continue editing a saved board.

**Acceptance criteria**

- V1 opens only Whiteboard's own XML format; it does not import/export draw.io XML.
- Supported older versions can be detected and migrated using explicit migrations.
- Invalid or unsupported future-version files produce a clear error.
- A failed import leaves the currently open board unchanged.
- XML is parsed as data; imported markup, scripts, and external references are never executed.
- A live session can be started from an imported Whiteboard XML board.

**Status:** Confirmed. Schema version and migration implementations are implementation gates.

### WB-S16 - Warn about unsaved local changes

**As a** user, **I want** a warning before I lose unsaved edits, **so that** I do not accidentally discard my board.

**Acceptance criteria**

- Leaving or closing a board with unsaved changes shows a warning with a save/discard choice.
- Opening another file with unsaved changes asks whether to save or discard before replacing the current board.
- Canceling the warning keeps the current board open and unchanged.

**Status:** Confirmed.


# 02 — Architecture Direction

> Covers Section 3 of the research document.

## 1. Basic approach
- Start with a **modular feature boundary**; a separate microservice is not a starting requirement. Keep it simple first and split later if needed.
- Future integration uses **a single Whiteboard entry point and one host contract**.

## 2. What the host contract must pass
| Item | Why it is needed |
|------|------------------|
| **Workspace** | Which workspace the board belongs to |
| **Board** | Which board to open |
| **User** | Who is using it (identity) |
| **Permission** | View / edit / manage |
| **Theme / locale** | UI matches Squid's look and language |
| **Close / navigation context** | How Whiteboard tells the host it closed or navigated back |

## 3. Conceptual boundaries
```
Future Squid host
  └─ Whiteboard entry point / host contract
       └─ Whiteboard interface and editor
            ├─ Board document API and durable persistence
            ├─ Realtime collaboration room and transient presence
            └─ Asset storage for images and attachments
```
- **Future Squid host:** The Squid app that will host Whiteboard.
- **Entry point / host contract:** The single doorway between the two.
- **Whiteboard interface and editor:** The canvas and tools users see.
- **Board document API + durable persistence:** Saving and loading board content permanently.
- **Realtime room + transient presence:** Live sync, cursors, and similar temporary data.
- **Asset storage:** Separate storage for images and attachments.

## 4. Architecture depth (L0–L4)
| Level | Name | Whiteboard content |
|-------|------|--------------------|
| **L0** | One-line definition | A collaborative whiteboard feature for drawing, diagramming, and presenting. |
| **L1** | System context | Users (3–4 per session), Squid host (future), Workspace, Auth boundary. |
| **L2** | Containers | Whiteboard Editor (client), Sync Room Service, Board Document Store, Asset Store. |
| **L3** | Components | Editor engine (tldraw/Excalidraw), CRDT layer (Yjs), WebSocket transport, DB schema, S3 bucket. |
| **L4** | Implementation | Specific functions, schemas, API endpoints, rate limits. |

**Rule:** L3/L4 decisions (such as same-object concurrent edit policy) depend on L0–L2 being explicit. Big picture first, details second.

## 5. Important data distinction
| Type | Examples | How to treat it |
|------|----------|-----------------|
| **Durable board content** | Pages, shapes, text, connectors, groups, presentation frames | Saved permanently |
| **Presence** | Cursors, selections, active tools, online indicators | Temporary; not permanent board history |
| **Local/session state** | Zoom, viewport, panel state | Per user, unless research says it should be shared |
| **Assets** | Large images, attachments | Stored separately; the board document holds only a *reference* |

## 6. Authorization
Must eventually be checked server-side for:
- Board **reads/writes**
- **Room joins**
- **Asset access**

> **Key point:** A browser-supplied room or workspace identifier is *not authorization*. A client can send anything, so the server must verify it itself.

## 7. Realtime sync — sequence design
```
User A          Sync Server          User B
  |-- edit op ------>|                  |
  |                  |-- validate auth ->|
  |                  |-- persist (CRDT) ->|
  |                  |-- broadcast op --->|
  |<-- ack ----------|                  |
  |                  |<-- presence ------|
  |<-- broadcast presence                |
```
Step by step:
1. User A sends an edit operation.
2. The server **validates auth**.
3. The edit is **persisted via the CRDT**.
4. The edit is **broadcast** to other users.
5. User A receives an **ack**.
6. User B's **presence** goes to the server and is broadcast to User A.

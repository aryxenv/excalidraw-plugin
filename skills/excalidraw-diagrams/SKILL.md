---
name: excalidraw-diagrams
description: "Create, update, and export architecture diagrams using Excalidraw MCP. Use when: diagram, architecture diagram, visualize, draw system, illustrate flow, create diagram, update diagram, export diagram, add diagram to README."
---

# Excalidraw Diagrams

Create architecture and flow diagrams via the Excalidraw MCP server, export them to PNG, and embed them in docs.

## When to Use

- User asks for a diagram, architecture visual, or flow illustration
- User asks to visualize a system, data flow, or component relationship
- User asks to add a diagram to a README or doc
- Any request involving "diagram", "draw", "visualize", "illustrate"

## Workspace Layout

Diagrams are exported directly to the user's project using the MCP server's built-in export. No local dependencies are needed.

## MCP Server

The Excalidraw MCP server is configured in `.mcp.json`:

```json
{
  "excalidraw": {
    "type": "stdio",
    "command": "npx",
    "args": ["-y", "excalidraw-mcp-server"],
    "tools": ["*"]
  }
}
```

Always use MCP tools (prefixed `mcp_excalidraw_*`) to create and manipulate diagrams. **Never hand-write Excalidraw JSON.**

## Procedure

### Step 1 — Plan the Diagram

Before touching any tool, decide:

- **Type**: Architecture overview, data flow, sequence, component relationship
- **Elements**: List the boxes, labels, arrows, and groupings needed
- **Layout**: Left-to-right for flows, top-to-bottom for hierarchies, grid for components

### Step 2 — Create the Diagram with MCP

1. Call `mcp_excalidraw_create_view` with all elements at once (fastest approach — renders inline for immediate visual feedback).
2. Alternatively, use `mcp_excalidraw_batch_create_elements` or `mcp_excalidraw_create_element` for incremental builds.
3. Organize with `mcp_excalidraw_group_elements`, `mcp_excalidraw_align_elements`, and `mcp_excalidraw_distribute_elements`.
4. Lock finished sections with `mcp_excalidraw_lock_elements`.

#### Element Cheat Sheet

| Element     | Use For                          | MCP Type  |
| ----------- | -------------------------------- | --------- |
| `rectangle` | Services, containers, components | Shape     |
| `ellipse`   | Start/end nodes, badges          | Shape     |
| `diamond`   | Decision points                  | Shape     |
| `arrow`     | Data flow, relationships         | Connector |
| `text`      | Titles, annotations              | Label     |
| `line`      | Separators, borders, connectors  | Line      |

#### Color Palette

| Color          | Hex       | Use For                                  |
| -------------- | --------- | ---------------------------------------- |
| Blue           | `#1971c2` | Primary services, APIs                   |
| Green          | `#2f9e44` | Databases, storage                       |
| Orange         | `#e67700` | External services, AI                    |
| Purple         | `#6741d9` | Agents, orchestration                    |
| Gray           | `#868e96` | Infrastructure, secondary, arrows        |
| Light Blue     | `#d0ebff` | Container/group backgrounds              |
| Light Green    | `#d3f9d8` | Data-layer group backgrounds             |
| Light Orange   | `#fff3bf` | AI-layer group backgrounds               |
| Dark Gray      | `#1e2530` | Section/tier background panels           |
| Title          | `#e6edf3` | Title text on dark backgrounds           |
| White          | `#ffffff` | Text on coloured fills                   |

#### Sizing

- **Spacing**: 40–60px between elements
- **Text padding**: 20px inside rectangles
- **Arrow gap**: 10px from source/target edges
- **Font sizes**: Title 28px, heading 22px, body 16px, caption 12px
- **Min width**: 80px for readability

### Design Quality Guidelines

The point of using Excalidraw is visual clarity and polish — **do not produce flat grids of same-sized boxes with text labels**. Every diagram should look like it was designed by a human with care.

#### Visual Hierarchy

- **Use layered backgrounds** to group related components. Place a large, semi-transparent rectangle behind each tier/layer (e.g., a dark panel behind the "Client" tier, another behind "API", another behind "Data"). This gives the diagram visual depth and makes the architecture layers immediately obvious.
- **Vary element sizes** to reflect importance. The primary service should be wider than supporting services. Don't make every box 200×80.
- **Use opacity** on background group rectangles (opacity 30–50) so elements on top remain prominent.

#### Layout Techniques

- **Tier labels**: Add a small text label (fontSize 14, gray) at the top-left of each tier background (e.g., "CLIENT", "API LAYER", "DATA & AI SERVICES"). This immediately communicates the architecture's structure.
- **Horizontal separator lines** between tiers using `line` elements (strokeColor `#30363d`, strokeWidth 1) to create visual breaks.
- **Center-align** elements within their tier. Use the MCP `align_elements` and `distribute_elements` tools.
- **Stagger arrows** so they don't overlap. Fan out from different points on the source element.

#### Component Styling

- **Filled rectangles** for services (set `backgroundColor` to the palette colour). Never leave shapes as outlines-only on dark backgrounds — they disappear.
- **Rounded corners** via `rx="8"` (the default for rectangles with roughness 0).
- **Bold labels** inside services (fontSize 18–20, white text). Subtitles below in smaller text (fontSize 12, white or light gray).
- **Icon-like badges** using small `ellipse` elements (20×20) with a single-character text label inside, placed at the top-left corner of a service box, to visually distinguish service types (e.g., "⚡" for API, "🗄" for DB).

#### Arrow & Connection Styling

- **Coloured arrows** to encode meaning — green arrows for data CRUD, orange for AI/search, gray for infrastructure/internal.
- **Label every arrow** with a short description (fontSize 12–13). Place the label near the midpoint of the arrow, offset slightly so it doesn't overlap the line.
- **Use strokeWidth 2** for primary data flows, strokeWidth 1 for secondary/internal flows.
- **Arrowhead on the target end only** (`endArrowhead: "arrow"`, `startArrowhead: null`).

#### What NOT to Do

- ❌ **Flat grid of identically-sized boxes** — this is a spreadsheet, not a diagram
- ❌ **Text-only labels floating in space** without background shapes
- ❌ **All elements the same colour** — use colour to encode categories
- ❌ **Unlabelled arrows** — every connection should explain what flows through it
- ❌ **Cramped spacing** — give elements room to breathe (40px minimum gap)
- ❌ **Thin outlines on dark backgrounds** — use filled shapes with solid colours

### Step 3 — Export PNG from MCP

Use the MCP to export the diagram directly as PNG:

```
mcp_excalidraw_export_scene  format="png"  padding=40
```

Save the PNG output to `diagrams/<name>.png` in the user's project. You can also export as SVG if needed:

```
mcp_excalidraw_export_scene  format="svg"  padding=40
```

### Step 4 — Save the .excalidraw Source

Construct and save the `.excalidraw` source file so the diagram can be edited later:

1. Call `mcp_excalidraw_get_resource resource="elements"` to get all elements.
2. Build the `.excalidraw` JSON:

```json
{
  "type": "excalidraw",
  "version": 2,
  "source": "copilot-cli",
  "elements": [ /* elements from MCP */ ],
  "appState": {
    "viewBackgroundColor": "#0d1117",
    "theme": "dark"
  },
  "files": {}
}
```

3. Save to `diagrams/<name>.excalidraw`.

> **Tip:** Use Python or Node.js to construct and write the JSON. Each element needs `id`, `type`, `x`, `y` at minimum. The MCP elements already have all required fields.

### Step 5 — Embed in Documentation

Reference the exported PNG using a **relative path** from the doc's location:

```markdown
![Architecture](diagrams/architecture.png)
```

## Updating an Existing Diagram

1. Query existing elements with `mcp_excalidraw_query_elements`.
2. Modify with `mcp_excalidraw_update_element` or `mcp_excalidraw_delete_element`.
3. Re-export PNG (Step 3) — overwrites the previous file.
4. No doc changes needed if the filename stays the same.

## Conventions

- **File naming**: Lowercase kebab-case (e.g., `data-flow.excalidraw`, `tag-lifecycle.excalidraw`)
- **Roughness**: Use `0` for clean technical diagrams, `1` for informal sketches
- **Theme**: Design with bright fills and light text for visibility
- **One diagram per file** — don't combine unrelated visuals

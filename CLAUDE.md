# Excalidraw Diagrams

This project provides Excalidraw-based architecture diagramming capabilities via MCP (Model Context Protocol).

## What's Available

### MCP Server — `excalidraw-mcp-server`

The Excalidraw MCP server exposes tools for creating and manipulating diagrams programmatically:

- **`create_view`** — Render a full diagram from an array of elements (rectangles, arrows, text, etc.)
- **`batch_create_elements` / `create_element`** — Add elements to the canvas
- **`update_element` / `delete_element`** — Modify or remove elements
- **`group_elements` / `align_elements` / `distribute_elements`** — Organize layout
- **`export_scene`** — Export the canvas as SVG or PNG
- **`query_elements` / `get_resource`** — Inspect current state
- **`create_from_mermaid`** — Convert Mermaid syntax to Excalidraw

### Diagrammer Agent

A specialized agent (`diagrammer`) that researches the codebase before drawing. It follows a 5-phase workflow: Research → Plan → Create → Confirm → Export. Every element in the diagram is grounded in real code — it never guesses.

### Excalidraw Diagrams Skill

The `excalidraw-diagrams` skill provides step-by-step procedures for creating, exporting, updating, and embedding diagrams. It includes a color palette, sizing guidelines, and design quality rules.

## Design Guidelines (Quick Reference)

- **Use layered backgrounds** — group components by tier with semi-transparent panels
- **Vary element sizes** — primary services wider than supporting ones
- **Label every arrow** — describe what flows through each connection
- **Color-code by category** — Blue (`#1971c2`) for APIs, Green (`#2f9e44`) for data, Orange (`#e67700`) for AI, Purple (`#6741d9`) for orchestration, Gray (`#868e96`) for infrastructure
- **Fill shapes** — never leave outlines-only on dark backgrounds
- **40px minimum spacing** between elements

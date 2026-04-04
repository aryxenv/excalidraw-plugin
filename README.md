# Excalidraw Diagrams Plugin

A plugin that adds Excalidraw architecture diagramming capabilities to your AI coding agent. Works with **GitHub Copilot CLI** and **Claude Code**.

## What's Included

| Component | Description |
|-----------|-------------|
| **Diagrammer Agent** | Researches your codebase, then creates accurate, grounded diagrams |
| **Excalidraw Diagrams Skill** | Step-by-step procedures for creating, exporting, and embedding diagrams |
| **Excalidraw MCP Server** | Tools for programmatic diagram creation (`create_view`, `export_scene`, etc.) |
| **SVG-to-PNG Export** | Dark-mode PNG conversion via `@resvg/resvg-js` |

## Installation

### GitHub Copilot CLI

Install directly from the repository:

```bash
copilot plugin install OWNER/REPO
```

Or from within an interactive session:

```
/plugin install OWNER/REPO
```

After installation:
- The **diagrammer** agent is available via `/agent`
- The **excalidraw-diagrams** skill is auto-loaded
- The **excalidraw** MCP server tools are available
- The **diagram** command is available

> **Note:** Plugin install provides the agent, skill, MCP server, and command. The MCP server can export diagrams as PNG directly via `export_scene format="png"`. The optional `excalidraw/scripts/svg-to-png.js` dark-mode export script is available when cloning the full repo (see Manual Setup).

### Claude Code

Clone or add this repository to your project. Claude Code automatically detects:
- `CLAUDE.md` — project instructions with diagramming guidance
- `.mcp.json` — Excalidraw MCP server configuration
- `.claude/commands/diagram.md` — slash command (`/project:diagram`)

Then use it:

```
/project:diagram system architecture showing all services and data flows
```

### Manual Setup (Any Tool)

Copy the relevant files into your project:

1. **MCP config** — Copy `.mcp.json` to your project root
2. **Agent** — Copy `.github/agents/diagrammer.agent.md`
3. **Skill** — Copy `.github/skills/excalidraw-diagrams/` directory
4. **Export tools** *(optional)* — Copy the `excalidraw/` directory, then run `cd excalidraw && npm install` for custom dark-mode PNG export

## Usage

### With the Diagrammer Agent

In Copilot CLI, select the diagrammer agent via `/agent`, then describe what you want:

> Create an architecture diagram of the API layer showing all endpoints, the database, and external service connections.

The agent will:
1. Research your codebase to find real components
2. Plan the diagram layout with you
3. Create it using Excalidraw MCP tools
4. Export to PNG and embed in your docs

### With the Slash Command (Claude Code)

```
/project:diagram data flow from user request through auth, API, and database
```

### Direct MCP Tool Usage

If you prefer to drive the MCP tools directly:

```
# Create a diagram with all elements at once
excalidraw-create_view  elements=[...]

# Export as SVG
excalidraw-export_scene  format="svg"

# Convert to dark-mode PNG
cd excalidraw && node scripts/svg-to-png.js diagrams/export/my-diagram.svg
```

## File Structure

```
├── plugin.json                    # Copilot CLI plugin manifest
├── .mcp.json                      # MCP server config (Copilot CLI + Claude Code)
├── CLAUDE.md                      # Instructions (Copilot CLI + Claude Code)
├── .claude/commands/diagram.md    # Claude Code slash command
├── .github/
│   ├── agents/
│   │   └── diagrammer.agent.md    # Diagrammer agent definition
│   └── skills/
│       └── excalidraw-diagrams/
│           └── SKILL.md           # Skill procedures & guidelines
├── .vscode/mcp.json               # VS Code MCP config
└── excalidraw/
    ├── package.json               # Export dependencies
    ├── scripts/svg-to-png.js      # SVG → PNG converter
    └── diagrams/                  # Diagram source & exports
```

## Cross-Tool Compatibility

This plugin works across tools because both ecosystems share conventions:

| File | Copilot CLI | Claude Code |
|------|-------------|-------------|
| `plugin.json` | ✅ Plugin manifest | — |
| `.mcp.json` | ✅ Auto-detected MCP config | ✅ Auto-detected MCP config |
| `CLAUDE.md` | ✅ Read as instructions (project-level) | ✅ Read as instructions |
| `.github/agents/` | ✅ Agent definitions | — |
| `.github/skills/` | ✅ Skill definitions | — |
| `.claude/commands/` | ✅ Loaded as commands via plugin | ✅ Slash commands |
| `.vscode/mcp.json` | ✅ VS Code workspace config | — |

> **MCP config note:** `.mcp.json` uses cross-platform `npx` and the standard `mcpServers` format. `.vscode/mcp.json` uses Windows-specific `npx.cmd` and the VS Code `servers` format. Both configure the same `excalidraw-mcp-server`. When using plugin install, `.mcp.json` takes precedence.

## License

MIT

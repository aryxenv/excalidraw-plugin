# Excalidraw Diagrams Plugin

> [!NOTE]
> Personally I use [excalidraw-workflow](https://github.com/aryxenv/excalidraw-workflow), which includes custom dark mode script layer which is an extra step that builds on top of the existing excalidraw-mcp. Custom logic is not possible to add through a plugin (yet), hence why it's a separate project.

A plugin that adds Excalidraw architecture diagramming capabilities to your AI coding agent. Works with **GitHub Copilot CLI** and **Claude Code**.

## What's Included

| Component                     | Description                                                                   |
| ----------------------------- | ----------------------------------------------------------------------------- |
| **Diagrammer Agent**          | Researches your codebase, then creates accurate, grounded diagrams            |
| **Excalidraw Diagrams Skill** | Step-by-step procedures for creating, exporting, and embedding diagrams       |
| **Excalidraw MCP Server**     | Tools for programmatic diagram creation (`create_view`, `export_scene`, etc.) |

## Installation

### GitHub Copilot CLI

Install directly from the repository:

```bash
copilot plugin install aryxenv/excalidraw-plugin
```

Or from within an interactive session:

```
/plugin install aryxenv/excalidraw-plugin
```

After installation:

- The **diagrammer** agent is available via `/agent`
- The **excalidraw-diagrams** skill is auto-loaded
- The **excalidraw** MCP server tools are available (auto-downloaded via `npx`)
- The **diagram** command is available

> **Note:** The MCP server (`excalidraw-mcp-server`) is downloaded automatically via `npx` — no manual npm install needed. Restart the CLI after installing the plugin.

### Claude Code

Add the MCP server and instructions to your project:

1. **MCP config** — Copy `.mcp.json` to your project root
2. **Instructions** — Copy `CLAUDE.md` to your project root
3. **Slash command** — Copy `.claude/commands/diagram.md`

Then use it:

```
/project:diagram system architecture showing all services and data flows
```

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

# Export as PNG
excalidraw-export_scene  format="png"
```

## File Structure

```
├── plugin.json                    # Copilot CLI plugin manifest
├── .mcp.json                      # MCP server config (Copilot CLI + Claude Code)
├── CLAUDE.md                      # Instructions (Copilot CLI + Claude Code)
├── .claude/commands/diagram.md    # Claude Code slash command
├── agents/                        # Agent definitions
│   └── diagrammer.agent.md
├── skills/                        # Skill definitions
│   └── excalidraw-diagrams/
│       └── SKILL.md
├── .vscode/mcp.json               # VS Code MCP config
```

## Cross-Tool Compatibility

This plugin works across tools because both ecosystems share conventions:

| File                | Copilot CLI                             | Claude Code                 |
| ------------------- | --------------------------------------- | --------------------------- |
| `plugin.json`       | ✅ Plugin manifest                      | —                           |
| `.mcp.json`         | ✅ Auto-detected MCP config             | ✅ Auto-detected MCP config |
| `CLAUDE.md`         | ✅ Read as instructions (project-level) | ✅ Read as instructions     |
| `agents/`           | ✅ Agent definitions                    | —                           |
| `skills/`           | ✅ Skill definitions                    | —                           |
| `.claude/commands/` | ✅ Loaded as commands via plugin        | ✅ Slash commands           |
| `.vscode/mcp.json`  | ✅ VS Code workspace config             | —                           |

> **MCP config note:** `.mcp.json` uses cross-platform `npx` and the standard `mcpServers` format. `.vscode/mcp.json` uses Windows-specific `npx.cmd` and the VS Code `servers` format. Both configure the same `excalidraw-mcp-server`. When using plugin install, `.mcp.json` takes precedence.

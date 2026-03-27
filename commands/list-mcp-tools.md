---
name: list-mcp-tools
description: List all available tools with their descriptions for a given MCP server or the default toolset.
version: 1.0.1
---

List all available tools with their descriptions for a given MCP server or the default toolset.

## Usage

The user provides an argument in `$ARGUMENTS` which is the name of the MCP server (e.g. `playwright`, `playwright-test`) or the keyword `default` for built-in tools.

## Instructions

### If `$ARGUMENTS` is `default`

List the following built-in tools with a one-line description for each, formatted as a markdown table:

| Tool       | Description                                                             |
| ---------- | ----------------------------------------------------------------------- |
| Read       | Read files from the filesystem (supports code, images, PDFs, notebooks) |
| Write      | Create new files or completely rewrite existing ones                    |
| Edit       | Make targeted string replacements in existing files                     |
| Bash       | Execute shell commands and terminal operations                          |
| Grep       | Search file contents using regex patterns (powered by ripgrep)          |
| Glob       | Find files by name/path patterns (e.g. `**/*.ts`)                       |
| Agent      | Launch specialized sub-agents for complex multi-step tasks              |
| Skill      | Invoke user-defined slash commands/skills                               |
| ToolSearch | Fetch full schemas for deferred MCP tools so they can be called         |

### If `$ARGUMENTS` is an MCP server name

1. Use the `ToolSearch` tool with query `+mcp__<server-name>` (replacing `<server-name>` with the value from `$ARGUMENTS`, converting hyphens to underscores in the prefix) and set `max_results` to 50 to retrieve as many tools as possible.
   - For example, if the argument is `playwright`, search for `+mcp__playwright`.
   - If the argument is `playwright-test`, search for `+mcp__playwright_test`.
2. Present the results as a markdown table with columns: **Tool** (short name without the `mcp__<server>__` prefix), **Full Name** (the complete tool name needed to call it), and **Description** (from the tool schema).

### If `$ARGUMENTS` is empty or missing

List the available options:

- `default` — built-in Claude Code tools
- Then use `ToolSearch` with a broad query like `mcp__` to discover which MCP servers are available, and list their names as additional options.

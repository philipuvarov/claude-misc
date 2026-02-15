# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

This is a Claude Code plugin (`claude-misc`) containing reusable skills, agents, hooks, and MCP server configs shared across multiple machines.

## Plugin Structure

```
.claude-plugin/plugin.json   # Plugin manifest (name, version, metadata)
skills/<name>/SKILL.md       # Slash-command skills (user-invocable via /claude-misc:<name>)
agents/<name>.md             # Custom subagent definitions
hooks/hooks.json             # Event-driven hooks (PreToolUse, PostToolUse, etc.)
scripts/                     # Shell scripts used by hooks and skills
.mcp.json                    # MCP server configurations bundled with plugin
```

## Conventions

- **Skills** go in `skills/<skill-name>/SKILL.md` with YAML frontmatter (`name`, `description`, `allowed-tools`, etc.). Supporting files (templates, scripts) live alongside the SKILL.md.
- **Agents** go in `agents/<agent-name>.md` with YAML frontmatter (`name`, `description`, `model`, `allowed-tools`).
- **Hooks** are registered in `hooks/hooks.json`. Hook scripts go in `scripts/` and should reference `${CLAUDE_PLUGIN_ROOT}` for portable paths.
- **MCP servers** are declared in `.mcp.json` at the repo root. Use `${VAR}` or `${VAR:-default}` for environment-dependent values.
- Skills can use `!`backtick`` syntax for dynamic context injection (runs shell commands before Claude sees content).
- Skills can reference arguments via `$ARGUMENTS`, `$0`, `$1`, etc.

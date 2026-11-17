# Antigravity Compatibility Notes

> Verified against the current Antigravity installation on 2026-11-17.

## Platform Details

- Plugin format: `plugins/<name>/plugin.json` manifest + `skills/`, `rules/`, optional `mcp_config.json`, `hooks.json`
- Skill format: `skills/<name>/SKILL.md` with YAML frontmatter (`name`, `description`)
- Rules format: `rules/AGENTS.md` — always-on Markdown, no frontmatter, 24KB per-file limit
- Discovery: Global plugins at `~/.gemini/config/plugins/`, workspace at `.agents/plugins/`
- Model selection: Use `inherit` — no hardcoded model identifiers
- Subagents: Available via `define_subagent` / `invoke_subagent` at runtime; not declarable in plugin config files

## Design Implications

- Specialist agents (Product Analyst, Prompt Engineer, Research Agent, Memory Manager, Code Auditor) are implemented as **skills** rather than separate agents, since plugin config does not support declarative agent definitions.
- If runtime subagent delegation proves valuable, the primary agent can spawn subagents dynamically using Antigravity's built-in `invoke_subagent` tool.
- MCP is not used in the MVP. Local Markdown files provide all persistent memory.

## Known Limitations

- Skills are progressively loaded (name + description visible, full content loaded on activation). Long skill instructions should use `references/` subdirectories.
- Rules budget is 20,000 tokens aggregate. The `AGENTS.md` rules file is well within this limit.
- Plugin enable/disable is managed via the settings UI or CLI `plugin enable`/`plugin disable`.

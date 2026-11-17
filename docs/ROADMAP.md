# Development Roadmap

## Phase 0: Repository and compatibility audit

- Inspect repository contents.
- Check current Antigravity agent, skill, rule, and MCP documentation.
- Record supported syntax and paths.
- Identify any conflicts between this specification and current platform behavior.

**Exit criteria:** A compatibility note exists and the implementation approach is based on verified current behavior.

## Phase 1: Idea-stage MVP

- Create the primary Project Architect agent.
- Create project discovery skill.
- Create project-local memory templates.
- Create memory status and provenance rules.
- Add installation instructions.

**Exit criteria:** On an empty project, the agent can guide discovery and create an initial brief, open questions, and a proposed roadmap without source code.

## Phase 2: Task-to-prompt workflow

- Create task preparation skill.
- Define task records and acceptance criteria.
- Add prompt quality requirements.
- Ensure codebase inspection is required before code-specific prompts.

**Exit criteria:** Given a feature request, the agent creates a useful, copy-ready implementation prompt grounded in the project context.

## Phase 3: Memory maintenance and progress

- Create memory update workflow.
- Add progress and development log handling.
- Add stale-memory detection.
- Distinguish reported implementation from verified implementation.

**Exit criteria:** After a simulated feature change, the agent updates only relevant memory and records evidence and status correctly.

## Phase 4: Audit and research

- Add project audit capability.
- Add technical research capability with source tracking.
- Add requirement-to-implementation traceability.

**Exit criteria:** The agent can audit a small project against requirements and report evidence-based findings without silently changing code.

## Phase 5: Optional specialist agents and integrations

- Split specialist roles into subagents if useful.
- Add MCP integrations only for demonstrated needs.
- Consider structured storage or semantic search only if Markdown becomes insufficient.
- Add versioned installation or update tooling.

**Exit criteria:** Integrations improve reliability without making the core system fragile or difficult to install.

## Non-goals for the first release

- Building a full desktop application.
- Creating a hosted multi-tenant memory service.
- Automatically coding every task.
- Automatically committing, pushing, deploying, or publishing.
- Requiring a vector database or paid API.
- Claiming universal IDE compatibility before testing it.

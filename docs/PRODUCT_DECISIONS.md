# Product Decisions and Open Questions

## Confirmed direction

- Product name: Project Architect.
- Primary initial environment: Google Antigravity.
- The user expects to use it from the beginning of a project, before source code exists.
- It should guide project discovery, maintain project context, prepare implementation prompts, track progress, and verify memory against reality.
- It should be reusable across multiple projects.
- The reusable system should have its own GitHub repository.
- Project-specific memory should remain separate inside each target project.
- The core should be IDE-independent where practical, while the first integration is Antigravity-first.
- A primary agent should provide one consistent user-facing interface.
- Specialist agents can be introduced where they add real value.

## Proposed decisions (not yet implemented)

- Use local Markdown files for the first memory implementation.
- Start with one primary agent and focused skills; split into subagents after the workflow is proven.
- Avoid a database, vector store, hosted service, or mandatory MCP dependency in the MVP.
- Use a project-local `.project-architect/` folder for each target project's memory, subject to Antigravity compatibility and user preference.

## Open questions for the developer

Ask progressively, not all at once:
1. Should the repository be public or private?
2. Which GitHub account/organization should own it?
3. Should the initial installation be a manual copy, a template repository, or a small installer?
4. Should Project Architect ever be allowed to edit product source code, or should it remain advisory and prompt-focused?
5. Should memory updates happen only on request, or should the agent propose updates after completed tasks?
6. Which model is actually available in the user's Antigravity model selector? Verify exact model naming in the IDE rather than assuming.
7. Does the user want a license, and if so, which one?

## Important caution

Do not block the MVP on optional choices. Use safe defaults for reversible implementation details and ask the user only when a choice affects ownership, visibility, cost, privacy, or destructive actions.

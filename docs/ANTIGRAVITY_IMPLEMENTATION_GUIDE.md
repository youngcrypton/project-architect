# Antigravity Implementation Guide

## Goal

Implement Project Architect using the current capabilities of Google Antigravity. The repository specification is intentionally platform-aware but does not hard-code unverified configuration syntax.

## First action: inspect the environment

Before creating agent files, verify from current official Antigravity documentation and the installed IDE:
- Supported custom agent configuration format and directory.
- Whether agents can delegate to subagents and the exact syntax.
- Supported skills and rules formats and discovery paths.
- Model selection and valid model identifiers.
- MCP server configuration format and permission model.
- Whether project-level configuration differs from user-level configuration.

Record the documentation links and the date checked in `docs/ANTIGRAVITY_COMPATIBILITY.md`.

Do not assume that a model label mentioned by the user, such as "Gemini 4 Argon", is a valid API/model identifier. Use the model actually offered by their installed IDE, or a supported inherit/default setting.

## Recommended MVP

Start with:
1. One primary Project Architect agent.
2. Three focused skills: project discovery, task-to-prompt, and memory update/verification.
3. Project-local Markdown memory templates.
4. One project rule that protects memory quality and prevents unsupported claims.
5. A small acceptance-test checklist.

Add separate subagents only after the primary workflow works. If Antigravity's native subagent support is available and stable, split Product Analyst, Research Agent, Prompt Engineer, Memory Manager, and Code Auditor into separate agents later.

## Suggested repository layout

Adapt paths to current Antigravity conventions after checking official documentation.

```text
project-architect/
├── agents/
├── skills/
│   ├── project-discovery/
│   ├── task-to-prompt/
│   ├── memory-update/
│   └── project-audit/
├── rules/
├── templates/
├── docs/
├── tests/
├── README.md
├── HANDOVER_FOR_GEMINI.md
└── CHANGELOG.md
```

## Suggested project installation

The reusable system should provide a documented way to copy or install its agent configuration into a target project. For the first version, a simple documented copy/install procedure is acceptable.

When installed into a target project, create:

```text
target-project/
└── .project-architect/
    ├── PROJECT_BRIEF.md
    ├── REQUIREMENTS.md
    ├── FEATURE_CATALOG.md
    ├── PROPOSED_ARCHITECTURE.md
    ├── CURRENT_ARCHITECTURE.md
    ├── TECHNICAL_DECISIONS.md
    ├── ROADMAP.md
    ├── TASKS.md
    ├── DEVELOPMENT_LOG.md
    ├── RISKS_AND_OPEN_QUESTIONS.md
    └── MEMORY_INDEX.md
```

## Core workflows

### Onboard a new idea

1. Detect whether source code exists.
2. If no code exists, enter discovery mode.
3. Ask one focused question at a time.
4. Record confirmed answers, assumptions, and open questions separately.
5. Draft a project brief for review.
6. After user approval, create initial requirements and roadmap.
7. Mark architecture as proposed until implementation exists.

### Prepare a task

1. Clarify the desired outcome if ambiguous.
2. Read relevant memory.
3. If source code exists, inspect relevant files and conventions.
4. Check for stale or conflicting memory.
5. Generate the implementation prompt.
6. Save a task record if the user wants task tracking or if the workflow is configured to do so.
7. Do not implement the feature unless asked.

### Update memory

1. Identify what changed.
2. Inspect evidence where possible.
3. Update only affected memory sections.
4. Mark claims as implemented or verified appropriately.
5. Preserve significant old decisions as superseded history.
6. Append a dated development-log entry.

### Audit a project

1. Read requirements and acceptance criteria.
2. Inspect implementation and tests.
3. Report findings with evidence.
4. Separate confirmed defects from risks and suggestions.
5. Do not modify code during an audit unless explicitly asked.

## MCP

Do not make MCP mandatory for the MVP. Local Markdown files are sufficient for initial persistent memory. Add MCP only when a real requirement appears, such as cross-project memory search, external documentation retrieval, or integration with an approved service.

If MCP is added:
- Document each server's purpose and permissions.
- Prefer read-only permissions unless writes are necessary.
- Do not store secrets in repository files.
- Explain any external data transfer.
- Test behavior when the server is unavailable.

## Verification

Use `docs/ACCEPTANCE_TESTS.md`. Test both an empty project and a project with an existing codebase. Confirm that no feature is described as implemented until it is actually implemented and checked.

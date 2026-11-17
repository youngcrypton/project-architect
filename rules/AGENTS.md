# Project Architect — Memory Quality & Provenance Rules

These rules apply whenever Project Architect skills are active.

## Status Discipline

- Every important decision, requirement, or architectural claim recorded in `.project-architect/` MUST have one of these statuses: `CONFIRMED`, `PROPOSED`, `VERIFIED`, `NEEDS_REVIEW`, `SUPERSEDED`.
- `CONFIRMED` means the developer directly stated it or authoritative project evidence supports it.
- `PROPOSED` means it is a suggestion that has not been accepted or implemented.
- `VERIFIED` means it has been checked against the current codebase, tests, or an authoritative source.
- `NEEDS_REVIEW` means it may be stale, incomplete, or conflicting.
- `SUPERSEDED` means it is retained for history but is no longer current.

## Never Fabricate

- Never label a proposed design as implemented or verified.
- Never invent file paths, function names, API endpoints, dependencies, or architecture that do not exist in the actual project.
- Never fabricate dates, evidence, sources, or test results.
- If evidence is unavailable, say so explicitly rather than guessing.

## Implementation ≠ Verification

- When a developer or coding agent reports that work is finished, record the status as `IMPLEMENTED`, not `VERIFIED`.
- Only mark something `VERIFIED` after inspecting relevant code, tests, configuration, or running checks that confirm the expected behavior.

## Memory Separation

- Project-specific memory belongs in `<project-root>/.project-architect/`. Never mix memory from different projects.
- Do not store API keys, passwords, tokens, private keys, or other secrets in project memory files.

## Codebase Inspection Before Code-Level Advice

- Before generating an implementation prompt that references specific files, functions, or conventions, inspect the actual repository first.
- If no codebase exists, the prompt must clearly state that and must not reference imaginary files or architecture.

## Change Tracking

- When updating memory, preserve significant old decisions as `SUPERSEDED` with a date rather than silently deleting them.
- Append a dated entry to `DEVELOPMENT_LOG.md` for meaningful changes.

## Audit Integrity

- During an audit, do not modify source code unless the developer explicitly requests it.
- Separate confirmed defects from risks and suggestions in audit reports.

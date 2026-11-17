# System Architecture

## 1. Overview

Project Architect has two layers:

- **Reusable system layer:** Agent instructions, skills, rules, workflows, templates, and tests maintained in this repository.
- **Project instance layer:** A memory directory and project-specific configuration created inside each software project that uses the system.

This separation allows the same system to support multiple unrelated projects without mixing their context.

## 2. Conceptual architecture

```text
Developer
   |
   v
Project Architect (primary agent)
   |
   +--> Discovery / Product Analysis
   |
   +--> Project Memory
   |      +--> Project brief
   |      +--> Requirements
   |      +--> Architecture and decisions
   |      +--> Roadmap and task status
   |      +--> Development history
   |
   +--> Task Preparation
   |      +--> Codebase inspection (when code exists)
   |      +--> Research (when needed)
   |      +--> Prompt generation
   |
   +--> Verification
          +--> Memory consistency check
          +--> Acceptance criteria check
          +--> Project audit
```

## 3. Primary agent responsibilities

Project Architect is the sole required user-facing entry point. It should:
- Determine the project's current lifecycle stage.
- Read only the memory needed for the current request, then retrieve more as needed.
- Identify whether the project is at idea, planning, active development, or maintenance stage.
- Delegate only when delegation improves quality or reduces context load.
- Present findings and next steps clearly.
- Ensure memory changes are consistent and traceable.

## 4. Specialist capabilities

Specialists may be separate agents or skills depending on Antigravity's current support.

### Product Analyst
Input: raw idea, constraints, user answers, existing product notes.
Output: structured requirements, user workflows, assumptions, open questions, acceptance criteria.

### Prompt Engineer
Input: requested task, relevant requirements, architecture, actual code evidence, constraints.
Output: copy-ready implementation prompt with scope, context, files, behavior, edge cases, tests, and acceptance criteria.

### Research Agent
Input: technical question or decision.
Output: evidence-backed comparison, trade-offs, recommendation, source references, confidence, and unresolved risks.

### Memory Manager
Input: new confirmed information, implementation evidence, changed decisions, task results.
Output: minimal updates to project memory, status changes, and a dated change log.

### Code Auditor
Input: requirements, acceptance criteria, implementation, test results.
Output: findings prioritized by severity, evidence, reproducible checks, and pass/fail status.

## 5. Lifecycle and memory status

The system must distinguish the following lifecycle states:

- `IDEA`: Product concept is being clarified.
- `PLANNING`: Requirements and proposed design are being developed.
- `READY`: A task or milestone has clear scope and acceptance criteria.
- `IN_PROGRESS`: Work has started.
- `IMPLEMENTED`: The developer or coding agent reports that work is complete, but verification may be pending.
- `VERIFIED`: Relevant checks or inspection confirm the expected result.
- `BLOCKED`: Progress depends on an unresolved issue.
- `DEFERRED`: Work is intentionally postponed.
- `SUPERSEDED`: A task or decision has been replaced.

Do not treat `IMPLEMENTED` as equivalent to `VERIFIED`.

## 6. Project-local memory layout

Recommended starting layout for each project:

```text
<project-root>/
└── .project-architect/
    ├── PROJECT_BRIEF.md
    ├── USERS_AND_WORKFLOWS.md
    ├── REQUIREMENTS.md
    ├── FEATURE_CATALOG.md
    ├── DESIGN_DIRECTION.md
    ├── TECHNICAL_DECISIONS.md
    ├── PROPOSED_ARCHITECTURE.md
    ├── CURRENT_ARCHITECTURE.md
    ├── ROADMAP.md
    ├── TASKS.md
    ├── DEVELOPMENT_LOG.md
    ├── RISKS_AND_OPEN_QUESTIONS.md
    └── MEMORY_INDEX.md
```

For an empty project, `CURRENT_ARCHITECTURE.md` should explicitly state that implementation architecture has not yet been verified. Proposed design belongs in `PROPOSED_ARCHITECTURE.md`.

## 7. Memory entry format

Use a consistent format for important decisions and facts:

```markdown
### [Short title]
- Status: PROPOSED | CONFIRMED | VERIFIED | NEEDS_REVIEW | SUPERSEDED
- Date: YYYY-MM-DD
- Source: developer statement | file inspection | test | external reference
- Evidence: file path, symbol, test name, or conversation-derived note where available
- Detail: concise explanation
- Revisit when: condition that could make this stale
```

Do not fabricate dates, evidence, or sources.

## 8. Task record format

```markdown
## TASK-001: [Task name]
- Status: IDEA | READY | IN_PROGRESS | IMPLEMENTED | VERIFIED | BLOCKED | DEFERRED
- Milestone: [milestone]
- Requirement links: [REQ-...]
- Objective: [outcome]
- Scope: [included work]
- Out of scope: [excluded work]
- Dependencies: [dependencies]
- Acceptance criteria:
  - [ ] Criterion one
  - [ ] Criterion two
- Evidence: [tests, files, review notes]
- Last updated: YYYY-MM-DD
```

## 9. Prompt generation contract

Every implementation prompt should contain:
1. Role and task objective.
2. Relevant product context.
3. User workflow and expected behavior.
4. Verified current architecture and relevant files.
5. Scope and explicit non-goals.
6. Technical constraints and existing conventions.
7. Edge cases and failure behavior.
8. Accessibility, responsive, security, and performance considerations where relevant.
9. Testing and verification instructions.
10. Acceptance criteria.
11. Instructions to report changed files, tests run, failures, and remaining risks.

If the project has no code yet, the prompt should say so and focus on the first setup or planning task. It must not invent existing files or architecture.

## 10. Privacy and safety

- Keep memory local to the project by default.
- Exclude secrets and personal data not needed for development.
- Never place credentials in Markdown memory.
- Do not send project content to third-party services without user awareness and authorization.
- Do not perform destructive or external actions without approval.

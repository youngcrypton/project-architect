---
name: pa-project-audit
description: >-
  Project Architect: read-only project audit and compliance evaluation.
  Triggered by the slash commands `/pa-project-audit`, `/pa-audit`, or `/audit`,
  or mentions like `@audit` or `@pa-project-audit`. Also use when the user asks to
  audit the codebase, verify implementation against requirements, check
  acceptance criteria, review code quality against project memory, or says "audit
  this project", "check if we met our requirements", "audit without changing code",
  or "run an audit". Inspects codebase and tests without modifying code, identifies
  confirmed defects vs risks vs suggestions, detects architectural drift, and
  produces a structured audit report.
---

# Project Audit Skill (`pa-project-audit`)

You are acting as **Project Architect** — a project-aware development companion.
This skill executes comprehensive, **read-only audits** of a software project.
It rigorously compares what was planned in `.project-architect/` against what was
actually implemented in the codebase, identifying defects, architectural drift,
untested paths, compliance gaps, and synthetic AI code smells.

---

## The Audit Invariants (Strict Rules)

1. **Strict Read-Only Enforcement**:
   - You must **NEVER** edit, create, or delete product code or configuration files during an audit.
   - Do NOT attempt to "quick-fix" bugs during the audit. Report them with precision so a dedicated coding task can address them cleanly.
2. **Defect vs Risk Separation**:
   - **Confirmed Defect**: An actual bug, failed test, broken logic, or explicitly violated requirement supported by tangible code evidence.
   - **Risk / Suggestion**: Potential performance issues, code style preferences, missing tests, unverified edge cases, or AI code smells. Never conflate an advisory risk with a confirmed defect unless acceptance criteria were violated.
3. **Evidence-Backed Attribution**:
   - Every claim must reference the exact file path, line numbers, function name, or test output.
4. **Memory Synchronization**:
   - The audit does not modify product code, but it may record the audit event in `.project-architect/DEVELOPMENT_LOG.md` and flag unverified/stale items in `REQUIREMENTS.md` or `TASKS.md` with status `NEEDS_REVIEW`.

---

## Step-by-Step Audit Workflow

### Step 1: Define Audit Scope & Ingest Project Memory

1. Locate `.project-architect/` at the project root.
2. Read the baseline specification documents:
   - `REQUIREMENTS.md`: Functional and non-functional requirements.
   - `TASKS.md`: Task list, implementation status, and acceptance criteria.
   - `TECHNICAL_DECISIONS.md`: Approved technologies, libraries, and patterns.
   - `CURRENT_ARCHITECTURE.md`: Previously verified architecture.
   - `PROPOSED_ARCHITECTURE.md`: Planned architecture.
3. Determine scope:
   - Full audit: inspect all requirements and active milestones.
   - Focused audit: inspect specific feature areas or recent commits/tasks requested by the user.

---

### Step 2: Codebase & Test Suite Inspection (Read-Only)

Inspect the repository using read-only operations:

1. **Locate Source Files**: Identify directory structure, key modules, entry points, and component directories.
2. **Examine Manifests & Configuration**: Check `package.json`, `tsconfig.json`, `Cargo.toml`, etc., to verify actual dependencies and versions.
3. **Inspect Implementation Files**:
   - Read relevant source code files using `read_file`.
   - Check input validation, authentication guards, error handling, and state management.
4. **Evaluate Test Coverage**:
   - Locate test files (e.g. `*.test.ts`, `tests/`, `__tests__/`).
   - Read test suites to check if tests actually assert against specified acceptance criteria.
   - If tests or type-checking can be verified via build output or sandbox test runs, check the pass/fail results.

---

### Step 3: Evaluate Requirements & Acceptance Criteria

For each requirement (`REQ-XXX`) or task (`TASK-XXX`) within scope:

1. **Check for Existence**: Is there actual code implementing this requirement?
   - If missing: Log as **Implementation Gap** / **Not Implemented**.
2. **Verify Acceptance Criteria**:
   - Compare code logic against each criterion in `TASKS.md` or `REQUIREMENTS.md`.
   - If an acceptance criterion is explicitly violated: Log as a **Confirmed Defect**.
3. **Verify Edge Cases & Error Handling**:
   - Check if network errors, empty states, null checks, and invalid inputs are handled.
4. **Inspect for AI Tells & Code Smells**:
   - Check for superficial narrative comments that merely narrate obvious syntax (`// return data`, `// fetch user`).
   - Check for unhandled stubs or placeholders (`// TODO: implement later`, empty catch blocks).
   - Check for AI marketing buzzwords (*seamless, delve, leverage, revolutionize*) in comments, UI copy, or docs.
   - Check for synthetic co-author attribution or AI watermarks in source files.

---

### Step 4: Detect Architectural & Decision Drift

Compare codebase reality against `TECHNICAL_DECISIONS.md` and `PROPOSED_ARCHITECTURE.md`:

1. **Stack Divergence**:
   - Did the codebase introduce unapproved libraries, frameworks, or external APIs not recorded in `TECHNICAL_DECISIONS.md`?
2. **Structural Divergence**:
   - Are components structured in a way that breaks approved patterns (e.g. direct database queries inside UI components when an API service layer was specified)?
3. **Record Findings**:
   - Note any discrepancies under **Architectural Drift**.

---

### Step 5: Synthesize Findings & Assign Severities

Categorize all findings using the classification rubric in [audit-report-template.md](./references/audit-report-template.md):

- **CRITICAL**: Security vulnerability, data loss risk, complete failure of core user workflow, or broken build preventing application execution.
- **HIGH**: Explicit requirement or acceptance criterion failed, major feature non-functional under standard conditions, or missing critical error handling.
- **MEDIUM**: Edge-case failure, non-critical requirement partially met, performance degradation under load, or incomplete test coverage for secondary paths.
- **LOW / CODE HYGIENE**: Code style inconsistency, redundant/narrative comments, AI buzzwords, stub leftovers, or cosmetic defects.

---

### Step 6: Generate and Deliver the Audit Report

1. Format the complete report adhering to [audit-report-template.md](./references/audit-report-template.md).
2. Deliver the report clearly to the user (via artifact or formatted markdown).
3. If the user desires memory tracking:
   - Append an entry to `.project-architect/DEVELOPMENT_LOG.md`:
     - `Type: audit`
     - `Summary: Audit conducted. Verdict: [PASS/FAIL]. [X] defects identified.`
   - Update any failed requirements/tasks in `REQUIREMENTS.md` or `TASKS.md` to `Status: NEEDS_REVIEW`.
4. Present actionable next steps:
   - Offer to generate implementation prompts (`pa-task-prompt`) to remediate specific defects identified in the audit.

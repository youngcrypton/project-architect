# Memory Update, Verification & Lifecycle Guide

This guide establishes the protocols for managing project memory, verifying implementations, recording historical changes, and maintaining status hygiene across `.project-architect/`.

---

## 1. Status Taxonomy & Transition Rules

Every requirement, technical decision, architecture component, and task must carry an explicit status.

| Status | Meaning | Valid Transitions From | Evidence Required |
|--------|---------|------------------------|-------------------|
| `IDEA` | Early concept, not yet specified | Initial state | None |
| `PROPOSED` | Suggested architecture/decision; not yet approved | `IDEA` | Rationale/Trade-off note |
| `CONFIRMED` | Explicitly agreed/stated by the developer | `PROPOSED`, `IDEA` | Developer statement |
| `READY` | Task/Milestone fully specified with acceptance criteria | `CONFIRMED`, `PROPOSED` | Criteria checklist |
| `IN_PROGRESS` | Implementation active | `READY` | Assignee note |
| `IMPLEMENTED` | Work reported complete by developer/agent; **unverified** | `IN_PROGRESS`, `READY` | Agent/dev completion report |
| `VERIFIED` | Inspected code/tests confirm functionality and acceptance criteria | `IMPLEMENTED` | File paths, test results, date |
| `NEEDS_REVIEW` | Potential staleness, conflict, or regression detected | Any status | Reason / conflict note |
| `SUPERSEDED` | Deprecated/replaced by a newer decision or requirement | Any status | Replacement ID, date, rationale |
| `BLOCKED` | Dependent on external resolution | Any status | Blocker ID / description |
| `DEFERRED` | Intentionally postponed | `IDEA`, `PROPOSED`, `READY` | Deferral rationale |

> [!CAUTION]
> **Cardinal Rule: `IMPLEMENTED` $\neq$ `VERIFIED`**
> When a developer or coding agent states "Feature X is complete" or "Task 001 is finished", the status is immediately updated to **`IMPLEMENTED`**, never `VERIFIED`. Transition to `VERIFIED` only occurs after actual code or test inspection confirms the claims.

---

## 2. Verification Protocol

When verifying an implemented feature or task:

1. **Identify Target Files**:
   - Locate the files claimed to implement the feature via git status, file search, or developer report.
2. **Inspect Code Reality**:
   - Read the actual implementation files using `read_file`.
   - Check that exported symbols, function signatures, data models, and routes match expectations.
3. **Verify Tests**:
   - Check if unit/integration tests exist covering the new logic.
   - Run tests if in a sandbox/allowed execution environment, or inspect the test output provided by the user.
4. **Evaluate Acceptance Criteria**:
   - Go through each acceptance criterion in `TASKS.md` or `REQUIREMENTS.md`.
   - Mark `[x]` only criteria backed by concrete code or test evidence.
5. **Promote Architecture**:
   - If a new component or data model was previously in `PROPOSED_ARCHITECTURE.md`, move its verified state into `CURRENT_ARCHITECTURE.md` with:
     - `Status: VERIFIED`
     - `Date verified: YYYY-MM-DD`
     - `Evidence: [file paths, exported symbols, test suite]`
6. **Promote Task Status**:
   - Update `TASKS.md` status to `VERIFIED`.
   - Record explicit evidence under `- Evidence:`.

---

## 3. Requirement Change & Impact Analysis Protocol

When the developer changes a requirement (e.g. changing email authentication to Google OAuth):

1. **Update `REQUIREMENTS.md`**:
   - Mark the old requirement as `SUPERSEDED` with:
     - `Superseded by: REQ-XXX on YYYY-MM-DD`
     - `Rationale: [Developer request / pivot]`
   - Create the new requirement (`REQ-XXX`) with `Status: CONFIRMED` (do NOT mark as `IMPLEMENTED` or `VERIFIED`).
2. **Trace Impact Across Memory**:
   - `USERS_AND_WORKFLOWS.md`: Update associated user workflow steps.
   - `FEATURE_CATALOG.md`: Update feature description, link to new `REQ-XXX`.
   - `TECHNICAL_DECISIONS.md`: Log decision (e.g., adding OAuth library), mark previous auth decision as `SUPERSEDED`.
   - `PROPOSED_ARCHITECTURE.md`: Update auth component design.
   - `TASKS.md`: Adjust tasks or create new task; mark obsolete tasks as `SUPERSEDED` or `DEFERRED`.
3. **Never Fabricate Implementation**:
   - Even if the developer says "we're switching to Google OAuth now", the implementation status remains unbuilt (`READY` or `PROPOSED`) until code is created and inspected.

---

## 4. `DEVELOPMENT_LOG.md` Standard Entry Format

Every memory update that alters status, decisions, or requirements must append an entry to `DEVELOPMENT_LOG.md`:

```markdown
## YYYY-MM-DD: [Action Title]
- Type: implementation | verification | requirement_change | technical_decision | reconciliation | bug
- Summary: [1-2 sentences summarizing the change]
- Details: [Context, reasoning, or verification findings]
- Affected files:
  - Code: `path/to/file.ext`
  - Memory: `.project-architect/FILE.md`
- Status changes:
  - `TASK-XXX`: IN_PROGRESS -> IMPLEMENTED (or IMPLEMENTED -> VERIFIED)
  - `REQ-XXX`: CONFIRMED (new) / SUPERSEDED (old)
```

---

## 5. Secrets & Privacy Defense Protocol

Project Architect memory files are committed to version control and must remain 100% secret-free.

1. **Detection**:
   - Monitor all incoming inputs, patch notes, configuration files, and summaries for patterns resembling:
     - API Keys (e.g., `sk_live_...`, `AIzaSy...`, `ghp_...`, `aws_access_key_id`)
     - Passwords, Private Keys (`-----BEGIN PRIVATE KEY-----`), Database connection strings with credentials (`postgresql://user:pass@host...`)
2. **Sanitization**:
   - NEVER write detected secret strings to any `.project-architect/` file.
   - Replace with safe placeholders: `<YOUR_API_KEY>`, `[REDACTED_SECRET]`.
3. **User Advisory**:
   - Warn the user immediately:
     > *"Warning: An API key or secret was detected in this update. Project Architect does not store credentials in project documentation. Please store this key in an environment variable (e.g., `.env.local`) or secrets manager, and ensure it is excluded in `.gitignore`."*

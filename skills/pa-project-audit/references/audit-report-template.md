# Project Audit Report Template & Evaluation Rubric

This reference defines the structured format and severity classification for project audits conducted by `pa-project-audit`. Every audit report must adhere to this structure and remain strictly grounded in reproducible evidence.

---

## 1. Audit Principles

1. **Read-Only Guarantee**:
   - An audit strictly inspects code, tests, and memory. It must NEVER modify, create, or delete product code or configuration files during an audit.
2. **Evidence-Backed Findings**:
   - Every defect or gap reported must cite concrete evidence: exact file paths, line numbers, symbols, error logs, or missing exports.
   - Theoretical or unconfirmed concerns must be categorized under **Risks & Suggestions**, never as **Confirmed Defects**.
3. **Traceability to Requirements**:
   - Evaluate code against the explicit criteria defined in `.project-architect/REQUIREMENTS.md` and `.project-architect/TASKS.md`.
4. **Senior Craftsmanship & Anti-AI Smell Enforcement**:
   - Explicitly inspect for robotic/narrative comments, placeholder stubs, AI watermarks, and synthetic marketing buzzwords.

---

## 2. Standard Audit Report Format

When delivering an audit, format the report as follows:

````markdown
# Project Audit Report: [Project Name]

- **Date**: YYYY-MM-DD
- **Auditor**: Project Architect (Audit Engine)
- **Codebase Scope**: [Directory / Commits / Files inspected]
- **Target Requirements**: [REQ-001, REQ-002, etc.]
- **Overall Verdict**: **PASS** | **CONDITIONAL PASS** | **FAIL**

---

## 1. Executive Summary
[2-3 paragraphs summarizing the health of the project, alignment between requirements and implementation, test adequacy, and primary concerns.]

---

## 2. Requirement Verification Matrix

| Requirement ID | Title | Status in Memory | Audit Finding | Evidence / Notes |
|----------------|-------|------------------|---------------|------------------|
| REQ-001 | [Title] | IMPLEMENTED | VERIFIED PASS | `src/auth/login.ts` (lines 45-80), tests pass |
| REQ-002 | [Title] | READY | NOT IMPLEMENTED | No matching implementation files found |
| REQ-003 | [Title] | IMPLEMENTED | DEFECT DETECTED | Password validation fails on special characters |

---

## 3. Confirmed Defects
*(Actual errors, logic flaws, broken acceptance criteria, or failing tests)*

### [DEF-001]: [Defect Title]
- **Severity**: CRITICAL | HIGH | MEDIUM | LOW
- **Related Requirement / Task**: REQ-XXX / TASK-XXX
- **Affected File(s)**: `path/to/file.ext#L12-L34`
- **Expected Behavior**: [What the specification requires]
- **Actual Behavior**: [What the code actually does or why it fails]
- **Reproducible Evidence**: [Code snippet, test failure trace, or step-by-step reproduction]

---

## 4. Architectural Drift & Discrepancies
*(Mismatches between `.project-architect/` specifications and actual codebase reality)*

- **Decision Drift**:
  - `TD-XXX`: [Description of divergence between decision and code, e.g., memory records REST API, codebase implemented GraphQL].
- **Component Drift**:
  - [Component Name]: [Unregistered dependencies or unintended coupling].

---

## 5. Test Suite & Coverage Assessment
- **Automated Tests**: [Passing / Failing / None found]
- **Type Safety & Build**: [Clean build / Type errors detected / Lint issues]
- **Untested Critical Paths**:
  - `path/to/critical_service.ts`: Missing test coverage for error/timeout handlers.

---

## 6. Code Craftsmanship & AI Smell Inspection
*(Detection of robotic AI fingerprints, narrative restatements, stubs, and buzzwords)*

- **Superficial / Redundant Comments**: [e.g. `// return user` in `src/users.ts:52` — remove narrative noise]
- **Unfinished Stubs / Mocks**: [e.g. `// TODO: implement error handler` in `src/api.ts:88`]
- **AI Buzzwords in Copy/Docs**: [e.g. Usage of "seamlessly", "cutting-edge" in `docs/` or UI strings]
- **Synthetic Watermarks**: [e.g. "Co-authored-by: AI" or assistant disclaimers]

---

## 7. Risks & Advisory Suggestions
*(Non-blocking technical debt, security hygiene, accessibility, or performance risks)*

1. **Security**: [e.g. Missing rate limiting on sensitive endpoint]
2. **Maintainability**: [e.g. Duplicated business logic across components]
3. **Performance**: [e.g. Unmemoized complex computation or unindexed query]

---

## 8. Actionable Remediation Plan
Ordered list of recommended tasks to address audit findings:
- [ ] 1. Fix [DEF-001]: [Action description]
- [ ] 2. Update `.project-architect/REQUIREMENTS.md` or `TECHNICAL_DECISIONS.md` to reconcile architectural drift.
- [ ] 3. Clean up AI code smells and remove redundant comments.
- [ ] 4. Generate implementation prompt for missing [REQ-XXX].
````

---

## 3. Severity Classification Rubric

- **CRITICAL**: Security vulnerability, data loss risk, complete failure of core user workflow, or broken build preventing application execution.
- **HIGH**: Explicit requirement or acceptance criterion failed, major feature non-functional under standard conditions, or missing critical error handling.
- **MEDIUM**: Edge-case failure, non-critical requirement partially met, performance degradation under load, or incomplete test coverage for secondary paths.
- **LOW / CODE HYGIENE**: Code style inconsistency, minor UI alignment issue, redundant narrative comments, AI buzzwords, or placeholder stubs.

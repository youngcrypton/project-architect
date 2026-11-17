# Project Architect — Usage Manual

This guide describes how to use **Project Architect** across all four project lifecycle stages, detailing common user prompts, workflows, and best practices.

---

## 1. Overview of Skills

| Skill | Trigger Phrases | Primary Function | Output |
|---|---|---|---|
| `pa-discovery` | *"I want to build...", "Plan this project from scratch", "Onboard this repo"* | Guided question-and-answer discovery for new ideas or existing codebases | Populated `.project-architect/` directory |
| `pa-task-prompt` | *"Prepare a prompt for...", "Create task for...", "What should I give my coding agent?"* | Inspects codebase + memory to generate an implementation prompt | Copy-ready prompt + entry in `TASKS.md` |
| `pa-memory-update` | *"The coding agent finished...", "We switched to Google OAuth", "Update memory"* | Records task completions (`IMPLEMENTED` vs `VERIFIED`), pivots, and updates logs | Updated `TASKS.md`, `DEVELOPMENT_LOG.md`, `CURRENT_ARCHITECTURE.md` |
| `pa-project-audit` | *"Audit this project", "Check if we met our requirements", "Audit without edits"* | Read-only inspection of code vs requirements, flagging defects and drift | Structured audit report |

---

## 2. Mode A: Idea Stage (Greenfield Project)

### Scenario
You have an idea for a software project and an empty directory. You do not have source code yet.

### Step-by-Step Flow
1. **Start Discovery**:
   - Tell the agent:
     > *"I want to build a web application that helps independent music teachers schedule lessons and accept credit card payments."*
2. **Progressive Elicitation**:
   - The agent activates `pa-discovery`.
   - It asks one question at a time (e.g., target users, must-have features, constraints, integrations).
   - If a question has discrete options, the agent presents multiple-choice buttons.
   - For anything you haven't decided, reply *"Not sure yet"* or *"Skip"* — the agent records it under `RISKS_AND_OPEN_QUESTIONS.md`.
3. **Review Project Memory**:
   - The agent initializes `.project-architect/` with all 13 documents.
   - It marks confirmed facts as `CONFIRMED` and its own suggestions as `PROPOSED`.
   - Architecture proposals are saved in `PROPOSED_ARCHITECTURE.md`.
   - `CURRENT_ARCHITECTURE.md` remains empty ("no implementation verified").
   - You review the generated project brief summary and approve or adjust.

---

## 3. Mode B: Planning & Task Preparation

### Scenario
You are ready to begin implementation of the first milestone or feature.

### Step-by-Step Flow
1. **Request an Implementation Prompt**:
   - Tell the agent:
     > *"Prepare a prompt for the coding agent to implement the teacher calendar view."*
2. **Context-Aware Prompt Generation**:
   - The agent activates `pa-task-prompt`.
   - It inspects the workspace (detecting greenfield or existing code).
   - It checks `PROJECT_BRIEF.md`, `REQUIREMENTS.md`, and `TECHNICAL_DECISIONS.md`.
   - It creates `TASK-001` in `.project-architect/TASKS.md` with status `READY`.
   - It outputs a comprehensive, copy-ready prompt containing in-scope/out-of-scope boundaries, acceptance criteria, error handling, and coding agent reporting instructions.
3. **Execute with Your Coding Agent**:
   - Copy the generated prompt block directly into your primary coding agent (Claude Code, Cursor, Codex, etc.).

---

## 4. Mode C: Active Development & Verification

### Scenario
Your coding agent has completed the task and modified files in the repository.

### Step-by-Step Flow
1. **Report Completion**:
   - Tell Project Architect:
     > *"The coding agent finished TASK-001 and created the calendar components."*
2. **Status Transition to `IMPLEMENTED`**:
   - The agent activates `pa-memory-update`.
   - It immediately updates `TASK-001` to `IMPLEMENTED` (unverified).
3. **Verification Inspection**:
   - Tell the agent:
     > *"Please verify the implementation and tests."*
   - The agent inspects the created files (`read_file`), checks types, and evaluates test coverage against the acceptance criteria in `TASK-001`.
4. **Status Transition to `VERIFIED`**:
   - If criteria pass:
     - Promotes `TASK-001` to `VERIFIED` with file/test evidence.
     - Checks off criteria `[x]` in `TASKS.md`.
     - Promotes the verified component into `CURRENT_ARCHITECTURE.md`.
     - Appends an entry to `DEVELOPMENT_LOG.md`.

---

## 5. Mode D: Maintenance, Pivots & Auditing

### Scenario 1: Changing a Requirement
- Tell the agent:
  > *"We decided to switch from Stripe to LemonSqueezy for payment processing."*
- The agent activates `pa-memory-update`:
  - Marks previous Stripe requirement and decision as `SUPERSEDED` with date and reason.
  - Adds LemonSqueezy requirement as `CONFIRMED`.
  - Links affected workflows and updates `DEVELOPMENT_LOG.md`.
  - Does NOT mark LemonSqueezy as implemented.

### Scenario 2: Auditing Code Against Requirements
- Tell the agent:
  > *"Audit this project against our requirements. Do not change any code."*
- The agent activates `pa-project-audit`:
  - Inspects code files and test suites in read-only mode.
  - Categorizes findings into **Confirmed Defects**, **Architectural Drift**, and **Risks & Suggestions**.
  - Outputs a structured audit report with reproducible file and line evidence.
  - Recommends an ordered remediation plan without editing code.

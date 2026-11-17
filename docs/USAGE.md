# Project Architect — Usage Manual

This guide describes how to use **Project Architect** across all four project lifecycle stages, detailing common user prompts, workflows, and best practices.

---

## 1. Overview of Skills & Activation Commands

| Skill | Slash Commands | `@` Mentions | Natural Triggers | Primary Function |
|---|---|---|---|---|
| `pa-discovery` | `/pa-discovery`<br>`/discover`<br>`/onboard` | `@discovery`<br>`@pa-discovery` | *"I want to build...", "Plan this project", "Onboard repo"* | Guided question-and-answer discovery for new ideas or existing codebases |
| `pa-task-prompt` | `/pa-task-prompt`<br>`/pa-prompt`<br>`/prompt` | `@prompt`<br>`@pa-task-prompt` | *"Prepare prompt for...", "Create task for...", "Next prompt"* | Inspects codebase + memory to generate an implementation prompt |
| `pa-memory-update` | `/pa-memory-update`<br>`/pa-sync`<br>`/sync` | `@sync`<br>`@pa-memory-update` | *"Coding agent finished...", "We switched to...", "Update memory"* | Records task completions (`IMPLEMENTED` vs `VERIFIED`), pivots, and logs |
| `pa-project-audit` | `/pa-project-audit`<br>`/pa-audit`<br>`/audit` | `@audit`<br>`@pa-project-audit` | *"Audit this project", "Check requirements", "Audit without edits"* | Read-only inspection of code vs requirements, flagging defects & drift |
| `pa-research` | `/pa-research`<br>`/research`<br>`/investigate` | `@research`<br>`@pa-research` | *"Research X", "I don't understand X", "Compare X vs Y"* | Deep technical investigation via web search & live Chrome browser |

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
2. **Context-Aware Codebase & Memory Inspection**:
   - The agent activates `pa-task-prompt`.
   - It inspects the workspace (detecting greenfield or existing code).
   - It checks `PROJECT_BRIEF.md`, `REQUIREMENTS.md`, and `TECHNICAL_DECISIONS.md`.
3. **Pre-Execution Confirmation & Alignment Gate**:
   - Before outputting the final prompt, the agent pauses and asks 1–2 focused confirmation questions via multiple-choice buttons (e.g., confirming files in scope, preferred testing library).
   - You tap your preferred option (or say *"proceed"*).
4. **Prompt Assembly & Task Tracking**:
   - It creates `TASK-001` in `.project-architect/TASKS.md` with status `READY`.
   - It outputs a comprehensive, copy-ready prompt containing in-scope/out-of-scope boundaries, acceptance criteria, error handling, and coding agent reporting instructions.
5. **Execute with Your Coding Agent**:
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

---

## 6. Mode E: Deep Technical Research & Browser Investigation

### Scenario
You are evaluating two libraries, hit an unfamiliar architectural pattern, or need real-time browser inspection of visual components or documentation.

### Step-by-Step Flow
1. **Trigger Research**:
   - Tell the agent:
     > *"/research Compare TanStack Query vs SWR for Next.js App Router with optimistic updates."*
2. **Autonomous Tool Routing**:
   - The agent activates `pa-research`.
   - Uses `search_web` for changelogs and RFCs.
   - Launches Chrome DevTools `browser` subagent for interactive visual inspection, client-rendered docs, or bundle size analyzers.
3. **Transparent Origin Reporting**:
   - If any documentation site requires authentication or throws a 403 / bot challenge, the agent explicitly reports the block and pivots to alternative public mirrors.
4. **Structured Decision & Prototype**:
   - Receives an executive summary, trade-off matrix (footprint, SSR support, maintenance), and a complete, typed code prototype.
5. **Memory Synchronization**:
   - The agent offers to append the decision to `.project-architect/TECHNICAL_DECISIONS.md` (`TD-XXX`) and update `DEVELOPMENT_LOG.md`.

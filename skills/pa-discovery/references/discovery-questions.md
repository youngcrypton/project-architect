# Discovery Question Framework

This reference provides the full question bank for project discovery. The main
SKILL.md gives the priority order and rules. This file provides detailed
follow-up prompts, recording guidance, and edge-case handling.

---

## Category 1: Core Identity

### Q1.1: Elevator Pitch
- Ask: "What does this project do, in one or two sentences?"
- Follow-up if vague: "Who uses it, and what can they do with it that they
  can't do easily today?"
- Record in: `PROJECT_BRIEF.md` → Elevator Pitch
- Status: CONFIRMED (user's own words)

### Q1.2: Problem Statement
- Ask: "What problem does this solve? Who experiences this problem?"
- Follow-up: "What happens if this problem isn't solved? What do people do
  today instead?"
- Record in: `PROJECT_BRIEF.md` → Problem Statement
- Status: CONFIRMED

### Q1.3: Desired Outcome
- Ask: "What does success look like? If this project works perfectly, what
  changes for the user?"
- Record in: `PROJECT_BRIEF.md` → Desired Outcome
- Status: CONFIRMED

---

## Category 2: Users

### Q2.1: Primary Users
- Ask: "Who is the main user of this product? Describe them briefly."
- Follow-up: "How technical are they? What's their context when they use this?"
- Record in: `PROJECT_BRIEF.md` → Target Users AND
  `USERS_AND_WORKFLOWS.md` → first persona
- Status: CONFIRMED

### Q2.2: Secondary Users
- Ask only if relevant: "Are there other types of users? Admins, managers,
  API consumers?"
- Record in: `USERS_AND_WORKFLOWS.md` → additional personas
- Status: CONFIRMED if stated, PROPOSED if inferred

### Q2.3: Key Workflow
- Ask: "Walk me through the main thing a user does, step by step. What do
  they see, click, or interact with?"
- Follow-up: "What's the first thing they do when they open the app? What's
  the most important action?"
- Record in: `USERS_AND_WORKFLOWS.md` → WF-001
- Status: CONFIRMED

---

## Category 3: Scope and Type

### Q3.1: Project Type
- Use `ask_question` with options:
  - Web application
  - Mobile app (iOS/Android/cross-platform)
  - CLI tool
  - API / backend service
  - Library / package
  - Desktop application
  - Browser extension
  - Other
- Record in: `PROJECT_BRIEF.md` → Key Constraints (project type)
- Status: CONFIRMED

### Q3.2: Platform
- Ask only if not already clear from project type.
- Use `ask_question` with options:
  - Web (browser-based)
  - iOS
  - Android
  - Cross-platform mobile
  - Desktop (Windows/macOS/Linux)
  - Multi-platform
- Record in: `PROJECT_BRIEF.md` → Key Constraints (platform)
- Status: CONFIRMED

### Q3.3: Scope
- Use `ask_question` with options:
  - MVP / proof of concept (smallest useful version)
  - Core product (main features, production-ready)
  - Full product (comprehensive feature set)
- Record in: `PROJECT_BRIEF.md` → Key Constraints (scope) AND
  `ROADMAP.md` → affects milestone granularity
- Status: CONFIRMED

---

## Category 4: Features

### Q4.1: Must-Have Features
- Ask: "What are the 3–5 features that absolutely must work for this to be
  useful?"
- Follow-up: "If you could only ship one feature, which one?"
- Record in: `FEATURE_CATALOG.md` → one entry per feature, status IDEA
- Also derive initial requirements in `REQUIREMENTS.md`, status PROPOSED

### Q4.2: Nice-to-Have Features
- Ask only after must-haves: "Any features you'd love to have but could
  live without for the first version?"
- Record in: `FEATURE_CATALOG.md` → status IDEA, priority LOW/MEDIUM
- Do NOT let nice-to-haves bloat the roadmap

### Q4.3: Explicit Non-Goals
- Ask: "Anything this project should explicitly NOT do?"
- Record in: `PROJECT_BRIEF.md` → Key Constraints
- Status: CONFIRMED

---

## Category 5: Integrations and Data

### Q5.1: External Services
- Ask: "Does this need to connect to any external services or APIs? Payment
  processing, authentication providers, email, maps, AI services, etc."
- Record in: `PROJECT_BRIEF.md` → Integrations
- Status: CONFIRMED if named, PROPOSED if inferred

### Q5.2: Data
- Ask: "What kind of data does this handle? Where does it come from? Does it
  need to be stored persistently?"
- Follow-up if relevant: "Any sensitive data? Personal information, financial
  data, health data?"
- Record in: `PROPOSED_ARCHITECTURE.md` → Data Model (high level)
- Status: PROPOSED
- If sensitive data → record a risk in `RISKS_AND_OPEN_QUESTIONS.md`

---

## Category 6: Constraints

### Q6.1: Timeline
- Ask: "Do you have a deadline or target timeline?"
- Record in: `PROJECT_BRIEF.md` → Key Constraints
- Status: CONFIRMED

### Q6.2: Team
- Ask: "Is this just you, or is there a team? Who's building it?"
- Record in: `PROJECT_BRIEF.md` → Key Constraints
- Status: CONFIRMED

### Q6.3: Budget
- Ask only if relevant (hosting, paid APIs, etc.): "Any budget constraints
  for infrastructure or services?"
- Record in: `PROJECT_BRIEF.md` → Key Constraints
- Status: CONFIRMED

### Q6.4: Performance / Scale
- Ask only if relevant: "How many users do you expect? Any performance
  requirements?"
- Record in: `REQUIREMENTS.md` → non-functional requirement
- Status: CONFIRMED if stated, skip if not relevant

### Q6.5: Accessibility
- Ask if user-facing: "Any accessibility requirements? WCAG compliance level?"
- Record in: `REQUIREMENTS.md` → non-functional requirement
- Status: CONFIRMED if stated, PROPOSED if you recommend it

---

## Category 7: Technical Direction

### Q7.1: Tech Preferences
- Ask: "Do you have strong preferences for languages, frameworks, or tools?
  Or would you like to decide that later?"
- If they have preferences → record in `TECHNICAL_DECISIONS.md`, status
  CONFIRMED
- If they want help choosing → record as open question in
  `RISKS_AND_OPEN_QUESTIONS.md`. Do NOT pick for them during discovery.
- If they say "you decide" → propose options with trade-offs, record as
  PROPOSED

### Q7.2: Hosting / Deployment
- Ask only if relevant and the user seems ready: "Do you know where you want
  to host this? Or should we decide that later?"
- Record in: `TECHNICAL_DECISIONS.md` if decided, otherwise
  `RISKS_AND_OPEN_QUESTIONS.md`

### Q7.3: Existing Code or Dependencies
- Ask if onboarding: "Is there existing code, a starter template, or
  dependencies I should know about?"
- If code exists → inspect it before asking more questions

---

## Category 8: Design

### Q8.1: Visual Direction
- Ask if user-facing: "Any visual style preferences? Modern/minimal,
  playful/colorful, enterprise/serious? Any apps or sites whose look you
  admire?"
- Record in: `DESIGN_DIRECTION.md`
- Status: CONFIRMED if stated, skip if not user-facing

### Q8.2: Branding
- Ask only if relevant: "Do you have a name, logo, or brand colors already?"
- Record in: `DESIGN_DIRECTION.md`
- Status: CONFIRMED if stated

---

## Category 9: Wrap-Up

### Q9.1: Anything Else
- Always ask: "Is there anything important about this project that I haven't
  asked about?"
- Record any new information in the appropriate file

### Q9.2: Review
- Present the brief summary and ask for confirmation
- Incorporate corrections immediately

---

## Edge Cases

### User gives a detailed spec upfront
- Extract answers to as many questions as possible from the spec.
- Ask only about genuine gaps.
- Do not re-ask questions the spec already answers.

### User says "just get started" or "skip the questions"
- Ask only Q1.1 (elevator pitch), Q3.1 (project type), and Q3.3 (scope).
- Record everything else as open questions.
- Create memory with what you have — it can be filled in later.

### User changes an answer mid-conversation
- Update the relevant memory file.
- If the change contradicts something already recorded, mark the old entry
  as `SUPERSEDED` with a date.
- Note the change in `DEVELOPMENT_LOG.md`.

### User mentions a secret (API key, password)
- Warn immediately: "I won't store that in project memory — API keys and
  secrets should go in environment variables or a secrets manager, not in
  documentation."
- Do NOT record the secret anywhere in `.project-architect/`.
- Record in `RISKS_AND_OPEN_QUESTIONS.md` that secrets management needs to
  be set up.

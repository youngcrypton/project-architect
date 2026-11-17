# Project Architect (Antigravity Plugin)

A living, project-aware AI development companion for Google Antigravity that supports software projects from the idea stage through planning, implementation, verification, and maintenance.

---

## What It Is

Project Architect is **not an autonomous coding agent**. Instead, it is an architectural co-pilot that:
1. **Maintains living project memory** in a structured, portable `.project-architect/` directory.
2. **Guides idea-stage discovery** on greenfield projects before source code exists.
3. **Inspects code reality** before generating zero-hallucination implementation prompts for external coding agents (Claude Code, Cursor, Codex).
4. **Tracks progress with status integrity**, strictly enforcing `IMPLEMENTED` $\neq$ `VERIFIED`.
5. **Audits implementations** against specifications in read-only mode without touching code.

---

## Directory Structure

```
~/.gemini/config/plugins/project-architect/
├── plugin.json                          # Plugin manifest
├── README.md                            # This overview
├── rules/
│   └── AGENTS.md                        # Memory quality, status discipline & privacy rules
├── skills/
│   ├── pa-discovery/                    # Mode A: Idea discovery & onboarding
│   │   ├── SKILL.md
│   │   └── references/discovery-questions.md
│   ├── pa-task-prompt/                  # Mode B: Task preparation & prompt generation
│   │   ├── SKILL.md
│   │   └── references/prompt-contract.md
│   ├── pa-memory-update/                # Mode C: Verification & progress tracking
│   │   ├── SKILL.md
│   │   └── references/verification-guide.md
│   └── pa-project-audit/                # Mode D: Read-only project auditing
│       ├── SKILL.md
│       └── references/audit-report-template.md
├── templates/                           # 13 blank memory templates for target projects
│   ├── PROJECT_BRIEF.md
│   ├── USERS_AND_WORKFLOWS.md
│   ├── REQUIREMENTS.md
│   ├── FEATURE_CATALOG.md
│   ├── DESIGN_DIRECTION.md
│   ├── TECHNICAL_DECISIONS.md
│   ├── PROPOSED_ARCHITECTURE.md
│   ├── CURRENT_ARCHITECTURE.md
│   ├── ROADMAP.md
│   ├── TASKS.md
│   ├── DEVELOPMENT_LOG.md
│   ├── RISKS_AND_OPEN_QUESTIONS.md
│   └── MEMORY_INDEX.md
└── docs/
    ├── INSTALLATION.md                  # Installation & project setup guide
    ├── USAGE.md                         # Detailed usage manual for all modes
    ├── LIMITATIONS.md                   # Operational boundaries & constraints
    └── ANTIGRAVITY_COMPATIBILITY.md     # Platform notes & verification log
```

---

## Activation Commands & Shortcuts

You can trigger any skill immediately using clean slash commands or `@` mentions:

| Action | Slash Command | Shortcut | `@` Mention |
|---|---|---|---|
| **Start Discovery / Onboard** | `/pa-discovery` | `/discover`, `/onboard` | `@discovery` |
| **Prepare Implementation Prompt** | `/pa-task-prompt` | `/prompt`, `/task` | `@prompt` |
| **Update Memory & Verify** | `/pa-memory-update` | `/sync`, `/verify` | `@sync` |
| **Audit Code vs Requirements** | `/pa-project-audit` | `/audit` | `@audit` |

---

## Documentation Quick Links

- [Installation Guide](./docs/INSTALLATION.md)
- [Usage Manual](./docs/USAGE.md)
- [Platform Compatibility](./docs/ANTIGRAVITY_COMPATIBILITY.md)
- [Limitations & Scope Boundaries](./docs/LIMITATIONS.md)

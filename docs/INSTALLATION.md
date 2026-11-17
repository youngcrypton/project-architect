# Installing Project Architect into a Project

## Prerequisites

- Google Antigravity with the Project Architect plugin enabled globally.

## Installation

Project Architect initializes automatically when you use the **pa-discovery** skill on a project. You can also set up the memory directory manually.

### Automatic (Recommended)

1. Open your project in Antigravity.
2. Tell the agent: *"I want to set up Project Architect for this project"* or *"Help me plan this project from scratch."*
3. The discovery skill will detect whether a `.project-architect/` directory exists and create one with fresh templates if it doesn't.

### Manual

Copy the template files from the plugin's `templates/` directory into your project:

```
<your-project>/
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

On Windows (PowerShell):
```powershell
$src = "$env:USERPROFILE\.gemini\config\plugins\project-architect\templates"
$dst = ".\.project-architect"
New-Item -ItemType Directory -Force -Path $dst
Copy-Item "$src\*" -Destination $dst -Force
```

On macOS/Linux:
```bash
cp -r ~/.gemini/config/plugins/project-architect/templates/ ./.project-architect/
```

## Version Control

Add `.project-architect/` to your repository so project memory travels with the code. It contains only Markdown — no secrets, no binaries.

If you prefer to keep it private, add `.project-architect/` to `.gitignore` instead.

## Uninstalling from a Project

Delete the `.project-architect/` directory. The global plugin remains available for other projects.

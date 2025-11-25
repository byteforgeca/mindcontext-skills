# FlowForge Skills

> Claude Code skills plugin for structured project management

## Overview

FlowForge brings structured project management to Claude Code through natural language. Simply describe what you want to do and Claude activates the appropriate skill.

## Installation

### Option 1: Claude Marketplace (Recommended)

**From GitHub (remote):**
```
/plugin install github:tmsjngx0/flowforge-skills
```

**From local path:**
```
/plugin install /path/to/flowforge-skills
```

### Option 2: Manual Installation

**Project-level (single project):**
```bash
mkdir -p .claude/skills
cp -r flowforge-skills/skills/* .claude/skills/
```

**Personal (all projects):**
```bash
cp -r flowforge-skills/skills/* ~/.claude/skills/
```

### Verifying Installation

After installation, check available skills:
```
/skills
```

You should see FlowForge skills listed (prd-create, epic-planning, task-workflow, etc.)

## Available Skills

### Project Setup
| Skill | Triggers | Description |
|-------|----------|-------------|
| `project-scaffold` | "initialize project", "set up flowforge" | Create `.project/` structure |
| `project-migrate` | "migrate project", "convert to flowforge" | Migrate existing PM files to FlowForge |

### Session Management
| Skill | Triggers | Description |
|-------|----------|-------------|
| `start-of-day` | "sod", "start of day", "morning sync" | Sync repos, prime context, show status |
| `prime-context` | "prime context", "load context" | Load project context files |
| `session-init` | "start session", "init session" | Initialize work session |

### Planning & Requirements
| Skill | Triggers | Description |
|-------|----------|-------------|
| `prd-create` | "create prd for X", "new prd" | Create product requirements document |
| `epic-planning` | "plan epic X", "decompose epic" | Break epic into tasks with dependencies |
| `epic-start` | "start epic X", "begin epic" | Create branch and launch epic work |

### Task Execution
| Skill | Triggers | Description |
|-------|----------|-------------|
| `task-start` | "start task X", "begin task" | Start specific task with analysis |
| `task-workflow` | "work on task X", "implement task" | Complete task lifecycle with quality gates |
| `tdd-workflow` | "tdd", "test first" | Test-driven development workflow |

### Git & Merging
| Skill | Triggers | Description |
|-------|----------|-------------|
| `merge-workflow` | "merge X into Y", "combine branches" | Branch merge with conflict resolution |

## Project Structure

FlowForge uses this standard structure:

```
your-project/
├── .project/
│   ├── prds/              # Product requirements documents
│   │   └── feature.md
│   ├── epics/             # Epic definitions and tasks
│   │   └── feature/
│   │       ├── epic.md    # Epic overview & architecture
│   │       ├── 001.md     # Task 1
│   │       ├── 002.md     # Task 2
│   │       └── updates/   # Progress tracking
│   └── context/           # Project context
│       ├── progress.md    # Current progress
│       ├── project-overview.md
│       └── tech-context.md
├── CLAUDE.md              # AI instructions
└── README.md
```

## Natural Language Examples

**Starting your day:**
```
"Start of day"
"Morning sync"
"What's my status?"
```

**Creating requirements:**
```
"Create a PRD for user authentication"
"I need to document the payment system requirements"
```

**Planning work:**
```
"Plan the authentication epic"
"Break down the payment feature into tasks"
"Start epic user-auth"
```

**Executing tasks:**
```
"Work on task 3"
"Start task 001"
"Use TDD for this feature"
```

**Git operations:**
```
"Merge feature branch into main"
"Combine test/0.69 with test/0.610"
```

**Migration:**
```
"Migrate this project to FlowForge"
"Convert existing PM structure"
```

## Migrating Existing Projects

If you have an existing project with:
- Custom `.claude/commands/` or `.claude/skills/`
- Project management files (PRDs, epics, tasks)
- Context documents scattered around

Use the migration skill:
```
"Migrate this project to FlowForge"
```

This will:
1. Scan for existing PM structures
2. Identify duplicates with FlowForge skills
3. Move context files to `.project/context/`
4. Reorganize PRDs and epics
5. Create migration report

## Configuration

FlowForge works out of the box. Optional customization in `CLAUDE.md`:

```markdown
## FlowForge Settings

- PRD location: .project/prds/
- Epic location: .project/epics/
- Context location: .project/context/
```

## Uninstalling

**Marketplace plugin:**
```
/plugin uninstall flowforge-skills
```

**Manual installation:**
```bash
rm -rf .claude/skills/  # project-level
# or
rm -rf ~/.claude/skills/flowforge-*  # personal
```

## Contributing

Issues and PRs welcome at: https://github.com/tmsjngx0/flowforge-skills

## License

MIT

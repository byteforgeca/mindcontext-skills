# FlowForge Skills

> Claude Code skills and agents plugin for structured project management with BMAD methodology

## Overview

FlowForge brings structured project management to Claude Code through natural language. Simply describe what you want to do and Claude activates the appropriate skill or spawns specialized agents for complex operations.

**What's Included:**
- **20 Skills** - Natural language triggers for PM workflows
- **4 Agents** - Specialized AI agents using BMAD methodology
- **Entity Management** - Unified CRUD for PRDs, Epics, and Issues
- **Serena Integration** - Intelligent code analysis for agents

## Installation

### Option 1: From Marketplace (Recommended)

**Step 1: Add the marketplace**
```
/plugin marketplace add tmsjngx0/flowforge-skills
```

This points Claude Code to the GitHub repository at `https://github.com/tmsjngx0/flowforge-skills` and registers it as a plugin marketplace.

**Step 2: Install the plugin**
```
/plugin install flowforge-skills@tmsjngx0
```

Or use the interactive browser:
```
/plugin
```
Then select "Browse Plugins" and find flowforge-skills.

**Quick one-liner:**
```
/plugin marketplace add tmsjngx0/flowforge-skills && /plugin install flowforge-skills@tmsjngx0
```

### Option 2: From Local Path

For local development or testing:
```
/plugin marketplace add ./flowforge-skills
```

Then install:
```
/plugin install flowforge-skills@local
```

### Option 3: Manual Installation

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

After installation, skills are automatically available. Try triggering one:
```
"Initialize FlowForge in this project"
```

Or ask Claude what skills are available:
```
"What FlowForge skills do I have?"
```

### Managing Plugins

```
/plugin                                    # Interactive plugin manager
/plugin enable flowforge-skills@tmsjngx0  # Enable plugin
/plugin disable flowforge-skills@tmsjngx0 # Disable plugin
/plugin uninstall flowforge-skills@tmsjngx0 # Remove plugin
```

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

### Entity Management
| Skill | Triggers | Description |
|-------|----------|-------------|
| `entity-manage` | "create/edit/show/list/close prd/epic/issue" | Unified CRUD with intelligent agent routing |

### Code Analysis
| Skill | Triggers | Description |
|-------|----------|-------------|
| `serena-setup` | "setup serena", "install serena", "configure serena" | Install and configure Serena MCP for intelligent code analysis |

## Available Agents

FlowForge includes specialized agents using BMAD (Business Model Amplification through Design) methodology:

| Agent | Purpose | Model | Used By |
|-------|---------|-------|---------|
| `pm-agent` | Strategic discovery and PRD creation | sonnet | `create prd` |
| `architect-agent` | Technical design and epic creation | sonnet | `create epic` |
| `developer-agent` | Implementation planning and coding | sonnet | `create issue`, `sync` |
| `qa-agent` | Quality validation and testing | sonnet | Code review, testing |

### How Agents Work

Skills automatically route to appropriate agents based on the operation:

```
"create prd user-auth"
  → Spawns pm-agent (sonnet)
  → Strategic questioning
  → Creates .project/prds/user-auth.md

"create epic user-auth"
  → Spawns architect-agent (sonnet)
  → Reads PRD, designs architecture
  → Creates .project/epics/user-auth/epic.md

"show prd user-auth"
  → Handled inline (haiku)
  → Fast read and display
```

### Agent Routing Matrix

| Operation | Agent | Model | Reason |
|-----------|-------|-------|--------|
| `create` (prd) | pm-agent | sonnet | Strategic discovery |
| `create` (epic) | architect-agent | sonnet | Technical decomposition |
| `create` (issue) | developer-agent | sonnet | Implementation planning |
| `edit` | inline | haiku | Simple modification |
| `show` | inline | haiku | Read and display |
| `list` | inline | haiku | Directory listing |
| `close` | inline | haiku | Status update |
| `sync` | developer-agent | sonnet | GitHub integration |
| `status` | inline | haiku | Quick status check |

### Serena Integration

Agents use [Serena MCP](https://github.com/anthropics/serena) for intelligent code analysis when available:

| Agent | Serena Tools |
|-------|-------------|
| `architect-agent` | `get_symbols_overview`, `find_symbol`, `find_referencing_symbols`, `search_for_pattern` |
| `developer-agent` | All tools including `replace_symbol_body`, `insert_*`, `rename_symbol` |
| `qa-agent` | `get_symbols_overview`, `find_symbol`, `find_referencing_symbols`, `search_for_pattern` |

**Setup Serena:**
```
"setup serena"
```

This configures Serena MCP for efficient code analysis. Agents automatically use Serena tools when available.

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

**Entity Management (NEW):**
```
"create prd user-auth"
"show epic payments"
"list issues in auth-epic"
"close issue 3"
"sync epic to github"
"prd status"
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
/plugin uninstall flowforge-skills@tmsjngx0
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

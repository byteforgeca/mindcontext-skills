# MindContext Skills

**The thinking layer for AI-assisted engineering.**

> From vibe code to production code.

## What is MindContext?

You built something with AI. That's awesome! But now you need to add features, fix bugs, or work with a team—and things get messy fast.

MindContext helps you move from "it works on my machine" to "it works in production." No CS degree required. Just practical structure that makes your AI-built projects maintainable.

**You'll get:**
- **Structure that scales** - PRDs, epics, and tasks that actually help
- **AI that understands context** - 4 specialized agents (PM, Architect, Developer, QA)
- **Clean git history** - Shadow Engineering separates AI mess from production code
- **Natural language** - Just describe what you want: "create a PRD for user auth"
- **Progressive learning** - Learn engineering concepts as you build

**Built for:**
- Developers who used Cursor/Lovable/Bolt and want to level up
- Teams adopting AI-assisted development
- Anyone who wants structure without the overwhelming theory

## Installation

### Option 1: From Marketplace (Recommended)

**Step 1: Add the marketplace**
```
/plugin marketplace add byteforgeca/mindcontext-skills
```

This points Claude Code to the GitHub repository at `https://github.com/byteforgeca/mindcontext-skills` and registers it as a plugin marketplace.

**Step 2: Install the plugin**
```
/plugin install mindcontext-skills@byteforgeca
```

Or use the interactive browser:
```
/plugin
```
Then select "Browse Plugins" and find mindcontext-skills.

**Quick one-liner:**
```
/plugin marketplace add byteforgeca/mindcontext-skills && /plugin install mindcontext-skills@byteforgeca
```

### Option 2: From Local Path

For local development or testing:
```
/plugin marketplace add ./mindcontext-skills
```

Then install:
```
/plugin install mindcontext-skills@local
```

### Option 3: Manual Installation

**Project-level (single project):**
```bash
mkdir -p .claude/skills
cp -r mindcontext-skills/skills/* .claude/skills/
```

**Personal (all projects):**
```bash
cp -r mindcontext-skills/skills/* ~/.claude/skills/
```

### Verifying Installation

After installation, skills are automatically available. Try triggering one:
```
"Initialize MindContext in this project"
```

Or ask Claude what skills are available:
```
"What MindContext skills do I have?"
```

### Managing Plugins

```
/plugin                                    # Interactive plugin manager
/plugin enable mindcontext-skills@byteforgeca  # Enable plugin
/plugin disable mindcontext-skills@byteforgeca # Disable plugin
/plugin uninstall mindcontext-skills@byteforgeca # Remove plugin
```

## Quick Start

**New to structured development?** Start here:

```
"Initialize MindContext in this project"
```

This creates the `.project/` folder structure for you. Then try:

- **"Create a PRD for [your feature]"** - Document what you're building
- **"Setup Shadow Engineering"** - Separate AI context from production code
- **"What MindContext skills do I have?"** - See everything available

**Already shipping code?** Jump straight to:

- **Planning:** "Create a PRD", "Plan this epic", "Break this down"
- **Building:** "Work on task 3", "Start epic user-auth", "Use TDD"
- **Maintaining:** "Commit my changes", "Merge this branch", "Sync to GitHub"

All commands use natural language—just describe what you need.

## Available Skills

### Project Setup
| Skill | Triggers | Description |
|-------|----------|-------------|
| `project-scaffold` | "initialize project", "set up mindcontext" | Create `.project/` structure |
| `project-migrate` | "migrate project", "convert to mindcontext" | Migrate existing PM files to MindContext |

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

### Git & Version Control
| Skill | Triggers | Description |
|-------|----------|-------------|
| `smart-commit` | "commit", "smart commit", "save work" | Intelligent commit across submodules, worktrees, and parent repo |
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

MindContext includes specialized agents using BMAD (Business Model Amplification through Design) methodology:

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

MindContext uses this standard structure:

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

## Shadow Engineering

Ever notice how your git history is full of "fix typo", "actually fix it", "ok NOW it works"? And random AI-generated comments everywhere?

**Shadow Engineering** keeps that mess separate from your production code:

```
Your Workspace (Shadow)              What You Ship (Light)
project-mgmt/                        actual-project/
├── .project/         ← PRDs, plans  ├── src/         ← Clean code
├── .claude/          ← AI context   ├── tests/       ← Real tests
├── prompts/          ← Experiments  ├── package.json ← No AI artifacts
└── actual-project/   ← submodule    └── .git         ← Pro commits
```

**Why this matters:**

✅ **Your git history looks professional** - No "fixing AI's mistakes" commits
✅ **Teams can review your code** - No random AI comments to explain
✅ **You can contribute anywhere** - Your code looks like any other repo
✅ **Context stays organized** - All the messy iteration lives in one place
✅ **Ship with confidence** - Production code is clean and maintainable

**Real scenario:**
- You: "Hey Claude, add user authentication"
- Claude iterates in the shadow repo (messy, trying things)
- You review, commit clean code to the submodule
- GitHub shows pro commits, not AI chaos

**Try it:**
```
"Setup Shadow Engineering"
```

See [shadow-setup skill](skills/shadow-setup/SKILL.md) for the full walkthrough.

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
"Commit"
"Smart commit"
"Save my work"
"Merge feature branch into main"
```

**Migration:**
```
"Migrate this project to MindContext"
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
"Migrate this project to MindContext"
```

This will:
1. Scan for existing PM structures
2. Identify duplicates with MindContext skills
3. Move context files to `.project/context/`
4. Reorganize PRDs and epics
5. Create migration report

## Configuration

MindContext works out of the box. Optional customization in `CLAUDE.md`:

```markdown
## MindContext Settings

- PRD location: .project/prds/
- Epic location: .project/epics/
- Context location: .project/context/
```

## Uninstalling

**Marketplace plugin:**
```
/plugin uninstall mindcontext-skills@byteforgeca
```

**Manual installation:**
```bash
rm -rf .claude/skills/  # project-level
# or
rm -rf ~/.claude/skills/mindcontext-*  # personal
```

## Contributing

Issues and PRs welcome at: https://github.com/byteforgeca/mindcontext-skills

## License

MIT

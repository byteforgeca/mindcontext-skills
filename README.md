# MindContext Skills

**Claude Code plugin for structured AI-assisted engineering.**

> From vibe code to production code.

## What is MindContext?

A **Claude Code plugin** that brings structured software engineering to AI-assisted development.

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

- **Planning:** "Create a PRD", "Create epic from PRD", "Plan this epic"
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
| `focus-state` | "what am I working on", "focus on epic X" | Manage current work focus (epic/issue/branch) |

### Planning & Requirements
| Skill | Triggers | Description |
|-------|----------|-------------|
| `prd-create` | "create prd for X", "new prd" | Create product requirements document |
| `epic-create` | "create epic X", "convert X PRD to epic" | Convert PRD to technical epic with architecture |
| `epic-planning` | "plan epic X", "decompose epic" | Break epic into tasks with dependencies |
| `epic-start` | "start epic X", "begin epic" | Create branch and launch epic work |

### Task Execution
| Skill | Triggers | Description |
|-------|----------|-------------|
| `next-task` | "next task", "what's next", "give me a task" | Find next available task by priority and dependencies |
| `next-job` | "next job", "what's next job", "give me next job" | Alias for next-task (same functionality) |
| `task-start` | "start task X", "begin task" | Start specific task with analysis |
| `task-workflow` | "work on task X", "implement task" | Complete task lifecycle with quality gates |
| `tdd-workflow` | "tdd", "test first" | Test-driven development workflow |

### Git & Version Control
| Skill | Triggers | Description |
|-------|----------|-------------|
| `smart-commit` | "commit", "smart commit", "save work" | Intelligent commit across submodules, worktrees, and parent repo |
| `merge-workflow` | "merge X into Y", "combine branches" | Branch merge with conflict resolution |


### Code Analysis
| Skill | Triggers | Description |
|-------|----------|-------------|
| `serena-setup` | "setup serena", "install serena", "configure serena" | Install and configure Serena MCP for intelligent code analysis |

## Available Agents

MindContext includes specialized agents using BMAD (Business Model Amplification through Design) methodology:

| Agent | Purpose | Model | Used By |
|-------|---------|-------|---------|
| `pm-agent` | Strategic discovery and PRD creation | opus | `create prd` |
| `architect-agent` | Technical design and epic creation | opus | `create epic` |
| `developer-agent` | Implementation planning and coding | opus | `create issue`, `sync` |
| `qa-agent` | Quality validation and testing | opus | Code review, testing |

### How Agents Work

Skills automatically route to appropriate agents based on the operation:

```
"create prd user-auth"
  → Spawns pm-agent (opus)
  → Strategic questioning
  → Creates .project/prds/user-auth.md

"create epic user-auth"
  → Spawns architect-agent (opus)
  → Reads PRD, designs architecture
  → Creates .project/epics/user-auth/epic.md

"show prd user-auth"
  → Handled inline (haiku)
  → Fast read and display
```

### How Agent Routing Works

**Main Claude agent automatically routes based on user intent:**

| User Request | Agent Spawned | Why |
|--------------|---------------|-----|
| "create prd auth" | pm-agent | Strategic discovery, BMAD methodology |
| "create epic auth" | architect-agent | Technical design, architecture decisions |
| "plan epic auth" | architect-agent | Task decomposition, dependency mapping |
| "work on task 3" | developer-agent | Implementation, coding, testing |
| "analyze this code" | qa-agent | Bug hunting, quality analysis |
| "review my changes" | qa-agent | Code review, pattern validation |
| "debug this error" | qa-agent | Root cause analysis, troubleshooting |

**No routing skill needed** - Main agent reads skill descriptions and user intent, then spawns the appropriate subagent with context.

### Serena Integration

Agents use [Serena MCP](https://github.com/oraios/serena) for intelligent code analysis when available:

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

- ✅ **Your git history looks professional** - No "fixing AI's mistakes" commits
- ✅ **Teams can review your code** - No random AI comments to explain
- ✅ **You can contribute anywhere** - Your code looks like any other repo
- ✅ **Context stays organized** - All the messy iteration lives in one place
- ✅ **Ship with confidence** - Production code is clean and maintainable

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

## Available Commands

MindContext includes **10 workflow-focused commands** for common operations:

### Session Commands
- `/sod` - Start of day (sync repos, load context, show status)
- `/eod` - End of day (check uncommitted, update context)

### Planning Commands
- `/prd` - Create PRD
- `/epic` - Create epic from PRD
- `/plan` - Decompose epic into tasks

### Working Commands
- `/next` - Find next available task
- `/focus` - Manage current work focus
- `/commit` - Smart commit across repos

### Syncing & Setup
- `/sync` - Sync to GitHub
- `/shadow` - Setup Shadow Engineering

**Note:** Analysis commands (analyze, debug, find, review, trace, security) were removed in v2.1.0. Use natural language instead - Claude routes to the correct agent automatically.

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
"Create epic user-auth"              # Convert PRD → Epic
"Plan the authentication epic"       # Decompose epic → tasks
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

**Analysis (natural language, no commands):**
```
"analyze this code for bugs"
"debug why login fails"
"find where API calls are made"
"review my changes before I commit"
"trace how user data flows from signup to database"
"security audit the payment flow"
```

**Focus management:**
```
"what am I working on?"
"focus on epic user-auth"
"focus on issue 003"
"what's next?"
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

## FAQ

### Does this work with Cursor, Codex, or other AI coding tools?

No. MindContext Skills is **specifically for Claude Code**. It's built on Claude Code's plugin system and won't run in other tools.

However, you can manually apply the **methodology** (PRDs, TDD, Shadow Engineering) in any tool—you just won't get the automated skills and agents.

### Do I need to know how to code?

You need basic familiarity with git, terminals, and your project structure. MindContext teaches you engineering concepts as you build—no CS degree required.

### Is this free?

Yes. MIT licensed and fully open source.

## Contributing

Issues and PRs welcome at: https://github.com/byteforgeca/mindcontext-skills

## License

MIT

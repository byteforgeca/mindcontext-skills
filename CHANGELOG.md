# Changelog

All notable changes to MindContext Skills will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [2.4.0] - 2025-12-03

### Added
- **Automatic focus state management** - Focus now updates automatically throughout workflow
  - `task-start` - Sets focus when starting a task
  - `task-workflow` - Updates focus at start and completion
  - `epic-start` - Sets focus when starting an epic
  - `end-of-day` - Refreshes focus timestamp before session ends

### Changed
- **Complete session continuity** - Focus state persists through entire workflow
  - Task start: Records epic, issue, branch, timestamp
  - Task completion: Updates timestamp to reflect actual completion time
  - Epic start: Records epic and branch (no specific issue yet)
  - End of day: Captures final session state

### Fixed
- **Focus state gaps** - No longer loses context if Claude crashes
  - Focus updates happen at key workflow points
  - Timestamps accurately reflect last activity
  - Session state survives accidental exits

## [2.3.1] - 2025-12-03

### Changed
- **start-of-day skill** - Now displays focus state from `.project/state/focus.json`
  - Shows last working epic, issue, and branch
  - Displays issue details (title, status) if available
  - Suggests how to continue work based on focus state
  - Provides guidance if no focus is set
  - Integrates seamlessly with `/next` command workflow

### Fixed
- **Focus state integration** - `/sod` now properly restores context of what you were working on

## [2.3.0] - 2025-12-03

### Removed
- **tdd-workflow skill** - Removed to eliminate conflict with superpowers plugin
  - superpowers provides superior TDD enforcement with micro-task granularity
  - MindContext now focuses on strategic planning layer
  - Users should install superpowers for TDD workflows

### Added
- **Plugin ecosystem documentation** - README now explains how MindContext complements other plugins
  - MindContext + superpowers integration guide
  - MindContext + feature-dev integration guide
  - Combined workflow examples showing all three plugins together
  - Clear division of responsibilities (Strategy/Analysis/Execution layers)

### Changed
- **plugin.json** - Updated description to reflect 22 skills and plugin complementarity
  - Version bumped from 2.2.0 → 2.3.0
  - Description now mentions superpowers and feature-dev compatibility
- **Total skills: 22** (23 - 1 removed tdd-workflow)

## [2.2.0] - 2025-12-03

### Added
- **External LLM review workflow** - New command for incorporating feedback from ChatGPT, Claude web, or other LLMs
  - `/update-plan` - Incorporate external review feedback into current issue/task
  - Works with existing MindContext Epic/Task structure
  - Updates issue files directly with improvements from review

### Changed
- **`/next` command` - Clarified documentation for continuing current work or finding next task

## [2.1.1] - 2025-12-03

### Fixed
- **Skill wording clarity** - Corrected language in 4 skills to clarify that main Claude agent spawns subagents, not the skills themselves
  - `task-workflow` - Changed "Invoke developer-agent" to "When main Claude agent spawns developer-agent"
  - `tdd-workflow` - Changed "Invoke developer-agent" to "When main Claude agent spawns developer-agent"
  - `task-start` - Changed "Invoke developer-agent" to "When main Claude agent spawns developer-agent"
  - `epic-start` - Removed "launching parallel agents" language, clarified main agent spawns developer-agents

## [2.1.0] - 2025-12-02

### Added
- **epic-create skill** - Convert PRD to technical Epic (completes PRD → Epic → Tasks workflow)
- **focus-state skill** - Manage current work focus (epic/issue/branch tracking)
- **State management** - `config.json` and `.project/state/focus.json` for project configuration
- **Workflow commands (5 new)** - `/prd`, `/epic`, `/plan`, `/focus`, `/next`

### Removed
- **entity-manage skill** - Violated architecture (skills cannot spawn agents)
- **Analysis commands (6 removed)** - `/analyze`, `/debug`, `/find`, `/review`, `/trace`, `/security`
  - Use natural language instead: "analyze this code", "debug this error", etc.
  - Main Claude agent routes to correct subagent automatically

### Changed
- **Commands simplified** - From 11 to 10 workflow-focused commands
- **project-scaffold skill** - Now creates `.project/state/` directory and state files
- **README** - Updated skill table, added commands section, clarified natural language usage
- **Total skills: 23** (21 existing + 2 new - 1 removed)

### Fixed
- **Architecture compliance** - No skills spawn agents (per official Claude Code docs)
- **Workflow gap** - PRD → Epic conversion now works via epic-create skill

## [1.1.0] - 2025-11-25

### Added
- **Session & Reporting Skills (4 new)**
  - `end-of-day` - Wrap up session, check uncommitted changes
  - `project-status` - Show overall project status and progress
  - `standup-report` - Generate daily standup summary
  - `next-task` - Find next available task to work on

### Changed
- Total skills now: 16

## [1.0.0] - 2025-11-25

### Added
- **Core Skills (12 total)**
  - `project-scaffold` - Initialize MindContext project structure
  - `project-migrate` - Migrate existing PM files to MindContext
  - `start-of-day` - Session initialization with repo sync
  - `prime-context` - Load project context files
  - `session-init` - Initialize work session
  - `prd-create` - Create product requirements documents
  - `epic-planning` - Decompose epics into tasks
  - `epic-start` - Begin epic work with branch setup
  - `task-start` - Start specific task with analysis
  - `task-workflow` - Complete task lifecycle with quality gates
  - `tdd-workflow` - Test-driven development workflow
  - `merge-workflow` - Branch merge with conflict resolution

- **Plugin Infrastructure**
  - `plugin.json` manifest with skills path
  - `marketplace.json` for marketplace distribution
  - README with installation instructions

### Fixed
- Plugin manifest `author` field format (object vs string)
- Marketplace `source` path format (`./` prefix required)
- Removed bash code blocks that caused parse errors

## [0.1.0] - 2025-11-23

### Added
- Initial plugin structure
- Basic skills: prd-create, epic-planning, task-workflow, project-scaffold, session-init
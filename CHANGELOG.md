# Changelog

All notable changes to FlowForge Skills will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

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
  - `project-scaffold` - Initialize FlowForge project structure
  - `project-migrate` - Migrate existing PM files to FlowForge
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
---
name: project-scaffold
description: Initialize MindContext project structure with .project/ directory, CLAUDE.md, and GitHub integration. Use when user says "initialize project", "set up mindcontext", "scaffold project", or "create project structure".
---

# Project Scaffold

Initialize or migrate any project to use MindContext methodology.

**Note:** This creates the `.project/` structure in a single repository. If you need Shadow Engineering (parent + submodule pattern), use the `shadow-setup` skill instead, which calls this skill and then adds the submodule structure.

## When to Use

- Setting up a new project (single repo)
- Adding MindContext to existing project
- User says "initialize project", "set up mindcontext", "scaffold"
- **For Shadow Engineering:** Use `shadow-setup` skill instead

## Workflow

### 1. Check Existing Structure

First, check if `.project/` directory already exists.
If it does, ask user whether to overwrite or skip.

Also check if this is a git repository. If not, offer to initialize git.

### 2. Detect Project Type

Look for these files to determine project type:
- `package.json` with "react" → frontend-react
- `package.json` with "express" → backend-node
- `pom.xml` → backend-java
- `Cargo.toml` → backend-rust
- `requirements.txt` or `pyproject.toml` → backend-python
- `*.csproj` or `*.sln` → backend-dotnet
- None of above → minimal

Report detected type to user.

### 3. Create Directory Structure

Create these directories:
- `.project/prds/`
- `.project/epics/`
- `.project/context/`
- `.project/state/`

### 4. Create Progress File

Create `.project/context/progress.md` with:

```markdown
# Project Progress

## Current Status
**Health:** Getting Started
**Updated:** [current date]

## In Progress
- Initial setup

## Next Steps
1. Create first PRD
2. Plan first epic
3. Start implementation
```

### 5. Create CLAUDE.md

Create `CLAUDE.md` in project root with:

```markdown
# CLAUDE.md

## Project Overview
[Project description - ask user or detect from package.json/README]

## MindContext Structure
- PRDs: `.project/prds/`
- Epics: `.project/epics/`
- Progress: `.project/context/progress.md`

## Workflow
- Create PRD: "Create a PRD for [feature]"
- Plan Epic: "Plan the [name] epic"
- Work: "Work on task [number]"

## Testing
[Detect test command or ask user]

## Code Style
Follow existing patterns in the codebase.

## Git Commits
- Do NOT include AI attribution in commit messages
- No "Generated with Claude Code" or "Co-Authored-By: Claude" footers
- Keep commit messages clean and professional
```

### 6. Create State Files

Create `.project/config.json` with:

```json
{
  "tdd_mode": true,
  "auto_sync_github": false,
  "default_agent_model": "opus",
  "commit_style": "conventional"
}
```

Create `.project/state/focus.json` with:

```json
{
  "current_epic": null,
  "current_issue": null,
  "current_branch": null,
  "last_updated": null
}
```

### 7. GitHub Labels (Optional)

If `gh` CLI is available and authenticated, create labels:
- `epic` (green)
- `task` (blue)

### 8. Summary

Show user:

```
MINDCONTEXT INITIALIZED

Created:
  .project/prds/
  .project/epics/
  .project/context/progress.md
  .project/state/focus.json
  .project/config.json
  CLAUDE.md

Detected: [project type]

Next: "Create a PRD for [your-first-feature]"
```

## Notes

- Non-destructive (preserves existing src/, tests/, etc.)
- Creates `.project/` as hidden directory
- Auto-detects project type
- GitHub integration is optional

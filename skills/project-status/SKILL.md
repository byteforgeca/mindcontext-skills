---
name: project-status
description: Show overall project status including epics, tasks, and progress. Use when user says "project status", "show status", "what's the status", or "overview".
---

# Project Status

Display comprehensive project status including epics, tasks, progress, and blockers.

## When to Use

- Need overall project view
- User says "project status", "what's the status", "show overview"
- Want to see all epics and tasks at a glance
- Checking project health

## Workflow

### Step 1: Gather Project Info

```bash
# Count PRDs
prd_count=$(find .project/prds -name "*.md" 2>/dev/null | wc -l)

# Count epics
epic_count=$(find .project/epics -maxdepth 1 -type d 2>/dev/null | wc -l)

# Count tasks by status
total_tasks=$(find .project/epics -name "[0-9]*.md" 2>/dev/null | wc -l)
open_tasks=$(find .project/epics -name "[0-9]*.md" -exec grep -l "^status: *open" {} \; 2>/dev/null | wc -l)
in_progress=$(find .project/epics -name "[0-9]*.md" -exec grep -l "^status: *in_progress" {} \; 2>/dev/null | wc -l)
done_tasks=$(find .project/epics -name "[0-9]*.md" -exec grep -l "^status: *done" {} \; 2>/dev/null | wc -l)
```

### Step 2: Generate Status Report

```
📊 PROJECT STATUS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Project: [project name from CLAUDE.md or directory]
Branch: [current branch]
Last Updated: [date]

OVERVIEW
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PRDs: [count]
Epics: [count]
Tasks: [open] open | [in_progress] in progress | [done] done | [total] total

Progress: [percentage]% complete
[████████░░░░░░░░░░░░]

EPICS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[For each epic:]
📁 [epic-name]
   Status: [active/complete/blocked]
   Tasks: [done]/[total] complete
   Progress: [percentage]%

IN PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[List tasks with status: in_progress]
  • #[task] - [title] ([epic])

BLOCKED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[List tasks with unmet dependencies]
  • #[task] - [title] (waiting on #[dep])

READY TO START
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[List tasks with no blockers]
  • #[task] - [title] ([epic])
```

### Step 3: Show Recent Activity

```bash
# Files modified in last 24 hours
find .project -name "*.md" -mtime -1 2>/dev/null
```

```
RECENT ACTIVITY (24h)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  • Modified [count] PRD(s)
  • Updated [count] epic(s)
  • Worked on [count] task(s)
```

### Step 4: Recommendations

Based on status, suggest:
```
RECOMMENDED ACTIONS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  → Work on task #[X] (highest priority ready task)
  → Unblock task #[Y] (has blocked dependents)
  → Review epic [Z] (near completion)
```

## Output Format

```
📊 PROJECT STATUS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[Full status report as shown above]

Quick Commands:
  "work on task [X]" - Start a task
  "next task" - Get next available task
  "epic status [name]" - Detailed epic view
```

## Notes

- Reads from `.project/` structure
- Shows tasks grouped by status
- Highlights blockers and dependencies
- Provides actionable recommendations

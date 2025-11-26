---
name: standup-report
description: Generate daily standup report with yesterday's work, today's plan, and blockers. Use when user says "standup", "daily standup", "standup report", or "what did I do".
---

# Standup Report

Generate a daily standup summary showing recent activity, current work, and next tasks.

## When to Use

- Morning standup meetings
- User says "standup", "daily standup", "what did I do yesterday"
- Need quick summary of recent work
- Preparing status update

## Workflow

### Step 1: Gather Yesterday's Activity

```bash
# Files modified in last 24 hours
recent_files=$(find .project -name "*.md" -mtime -1 2>/dev/null)

# Git commits in last 24 hours
git log --oneline --since="24 hours ago" --all
```

### Step 2: Get Current Work

```bash
# Find in-progress tasks
find .project/epics -name "*.md" -exec grep -l "^status: *in_progress" {} \;
```

### Step 3: Find Next Tasks

```bash
# Find open tasks with no blockers
for task in .project/epics/*/*.md; do
    status=$(grep "^status:" "$task" | head -1)
    deps=$(grep "^depends_on:" "$task" | head -1)
    # Check if open and no deps
done
```

### Step 4: Generate Standup Report

```
📅 DAILY STANDUP - [date]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

YESTERDAY (What I did)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[From git commits and modified files:]
  • [commit message / work item]
  • [commit message / work item]
  • [commit message / work item]

[From modified task files:]
  • Worked on #[task] - [title]
  • Updated #[task] - [title]

TODAY (What I'll do)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[In-progress tasks:]
  • Continue #[task] - [title] ([X]% complete)

[Next available tasks:]
  • Start #[task] - [title]

BLOCKERS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[If any blocked tasks:]
  ⚠ #[task] - Waiting on [dependency/external]

[If no blockers:]
  ✓ No blockers

QUICK STATS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Tasks: [open] open | [in_progress] active | [done] done
Commits (24h): [count]
```

## Output Format

Standard standup format:
```
📅 DAILY STANDUP - 2025-11-25

YESTERDAY:
  • Completed task #3 - API endpoints
  • Reviewed PR for authentication

TODAY:
  • Continue task #4 - Frontend integration
  • Start task #5 - Tests

BLOCKERS:
  ✓ None
```

## Slack/Teams Format (Optional)

If user asks for copyable format:
```
**Daily Standup - [date]**

*Yesterday:*
- [item]
- [item]

*Today:*
- [item]

*Blockers:*
- None
```

## Notes

- Pulls from git history and task files
- Shows both completed and in-progress work
- Highlights blockers clearly
- Can output in different formats for chat apps
- Pairs well with start-of-day skill

---
name: next-task
description: Find and suggest the next available task to work on based on dependencies and priority. Use when user says "next task", "what's next", "what should I work on", or "give me a task".
---

# Next Task

Find the next available task based on dependencies, priority, and current progress.

## When to Use

- Just finished a task
- Starting fresh work
- User says "next task", "what's next", "what should I work on"
- Need task recommendation

## Workflow

### Step 1: Find Open Tasks

```bash
# Find all open tasks
for epic_dir in .project/epics/*/; do
    for task in "$epic_dir"/*.md; do
        status=$(grep "^status:" "$task" | head -1)
        # Check if open
    done
done
```

### Step 2: Check Dependencies

For each open task:
```bash
# Get dependencies
deps=$(grep "^depends_on:" "$task" | head -1)

# Check if all deps are done
for dep in $deps; do
    dep_status=$(grep "^status:" ".project/epics/*/$dep.md")
    # If any dep not done, task is blocked
done
```

### Step 3: Prioritize Ready Tasks

Order by:
1. Priority field (if exists)
2. Task number (lower = earlier)
3. Epic priority
4. Can run in parallel (bonus)

### Step 4: Present Recommendations

```
📋 NEXT AVAILABLE TASKS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

RECOMMENDED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

✅ #[task] - [title]
   Epic: [epic-name]
   Priority: [high/medium/low]
   [If parallel: 🔄 Can run in parallel]

   Why this task:
   - [reason: first in queue / high priority / unblocks others]

OTHER READY TASKS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  • #[task] - [title] ([epic])
  • #[task] - [title] ([epic])
  • #[task] - [title] ([epic])

BLOCKED TASKS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  • #[task] - Waiting on #[dep]
  • #[task] - Waiting on #[dep], #[dep]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 Summary: [X] tasks ready, [Y] blocked

Start recommended task? Say "work on task [X]"
```

## Task Selection Logic

```
1. Filter: Only open tasks (status: open)
2. Filter: No unmet dependencies
3. Sort by:
   - Priority (high > medium > low)
   - Task number (001 before 002)
   - Unblocks others (tasks that are dependencies)
4. Recommend top task
```

## Special Cases

### No Tasks Available
```
📋 NO TASKS AVAILABLE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

All tasks are either:
  • Already in progress
  • Blocked by dependencies
  • Completed

Suggestions:
  → Check blocked tasks for unblock opportunities
  → Create new epic: "create prd for [feature]"
  → Review completed work
```

### All Tasks Blocked
```
⚠️ ALL TASKS BLOCKED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Blocked tasks:
  • #[task] → needs #[dep]
  • #[task] → needs #[dep]

To unblock:
  → Complete external dependency
  → Remove dependency if no longer needed
  → Work on blocking task in different epic
```

### In-Progress Tasks Exist
```
📋 NEXT TASK
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

⚠️ You have tasks in progress:
  • #[task] - [title] (started [date])

Options:
1. Continue current task
2. Start new task anyway (parallel work)

Ready tasks if you want something new:
  • #[task] - [title]
```

## Output Format

```
📋 NEXT TASK: #[X] - [Title]

Epic: [epic-name]
Priority: [priority]

Ready to start? Say "work on task [X]"
```

## Notes

- Respects task dependencies
- Prioritizes tasks that unblock others
- Warns about in-progress tasks
- Suggests alternatives if no tasks ready
- Works with `.project/epics/` structure

---
name: task-start
description: Begin work on a specific task with analysis and parallel stream support. Use when user says "start task", "begin task", "work on issue", or provides a task number.
---

# Task Start

Begin work on a specific task with developer-agent analysis and setup.

When main Claude agent spawns developer-agent for this task, the developer-agent should prepare the task for implementation:

1. **Find task** - Locate task file in `.project/epics/*/{number}.md`
2. **Load context** - Read task, epic, and dependencies
3. **Set focus** - Update `.project/state/focus.json` with current task
4. **Analyze** - Research codebase for patterns
5. **Plan** - Create implementation checklist
6. **Setup** - Update status to `in_progress`
7. **Launch** - Begin implementation or offer parallel streams

The developer-agent will:
- Search for task file by number
- Parse task frontmatter and acceptance criteria
- Check dependencies are met (or warn if not)
- **Update focus.json** - Set current_epic, current_issue, current_branch, last_updated
- Read parent epic for architecture context
- Analyze codebase for existing patterns
- Create implementation plan from criteria
- Update task status to `in_progress`
- Track progress in updates directory

**Focus State Update:**
```bash
# Extract epic and issue number from task path
EPIC=$(dirname "$TASK_FILE" | xargs basename)
ISSUE=$(basename "$TASK_FILE" .md)
BRANCH=$(git branch --show-current)

# Update focus.json
jq --arg epic "$EPIC" \
   --arg issue "$ISSUE" \
   --arg branch "$BRANCH" \
   --arg ts "$(date -u +"%Y-%m-%dT%H:%M:%SZ")" \
   '.current_epic = $epic | .current_issue = $issue | .current_branch = $branch | .last_updated = $ts' \
   .project/state/focus.json > /tmp/focus.json && \
   mv /tmp/focus.json .project/state/focus.json
```

Then either:
- Begin single-focus implementation
- Offer parallel work streams if applicable
- Defer to task-workflow for full lifecycle

Error handling:
- Task not found → suggest available tasks
- Dependencies not met → require completion first
- Already in progress → offer to continue
- Already complete → offer to reopen or suggest next task

---
name: task-start
description: Begin work on a specific task with analysis and parallel stream support. Use when user says "start task", "begin task", "work on issue", or provides a task number.
---

# Task Start

Begin work on a specific task with developer-agent analysis and setup.

Invoke developer-agent in task-start mode to prepare the task for implementation:

1. **Find task** - Locate task file in `.project/epics/*/{number}.md`
2. **Load context** - Read task, epic, and dependencies
3. **Analyze** - Research codebase for patterns
4. **Plan** - Create implementation checklist
5. **Setup** - Update status to `in_progress`
6. **Launch** - Begin implementation or offer parallel streams

The developer-agent will:
- Search for task file by number
- Parse task frontmatter and acceptance criteria
- Check dependencies are met (or warn if not)
- Read parent epic for architecture context
- Analyze codebase for existing patterns
- Create implementation plan from criteria
- Update task status to `in_progress`
- Track progress in updates directory

Then either:
- Begin single-focus implementation
- Offer parallel work streams if applicable
- Defer to task-workflow for full lifecycle

Error handling:
- Task not found → suggest available tasks
- Dependencies not met → require completion first
- Already in progress → offer to continue
- Already complete → offer to reopen or suggest next task

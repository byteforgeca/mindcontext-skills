---
name: task-workflow
description: Execute complete task lifecycle from analysis through implementation, testing, review, and completion with quality gates. Use when user says "work on task", "implement task", "do task", or "start task".
---

# Task Workflow

Execute complete task lifecycle with developer-agent orchestration.

When main Claude agent spawns developer-agent for this task, the developer-agent should execute the full workflow:

1. **Load task** - Read task file from `.project/epics/{epic}/{number}.md`
2. **Set focus** - Update `.project/state/focus.json` with current task
3. **Analyze** - Understand requirements and existing patterns
4. **Implement** - Write code following architecture
5. **Validate** - Build, test, verify
6. **Review** - Check acceptance criteria
7. **Complete** - Update status, track progress, update focus timestamp

The developer-agent will:
- Read task file and epic context
- **Set focus state** - Update focus.json with current task at start
- Check dependencies before starting
- Create implementation plan from acceptance criteria
- Write code matching existing patterns
- Run build and tests to validate
- Verify all acceptance criteria are met
- Update task status to `done` when complete
- **Update focus timestamp** - Refresh last_updated when task completes
- Track progress in `.project/context/progress.md`
- Suggest commit message

**Focus State Management:**
```bash
# At task start - Set focus
EPIC=$(dirname "$TASK_FILE" | xargs basename)
ISSUE=$(basename "$TASK_FILE" .md)
BRANCH=$(git branch --show-current)

jq --arg epic "$EPIC" \
   --arg issue "$ISSUE" \
   --arg branch "$BRANCH" \
   --arg ts "$(date -u +"%Y-%m-%dT%H:%M:%SZ")" \
   '.current_epic = $epic | .current_issue = $issue | .current_branch = $branch | .last_updated = $ts' \
   .project/state/focus.json > /tmp/focus.json && \
   mv /tmp/focus.json .project/state/focus.json

# At task completion - Update timestamp
jq --arg ts "$(date -u +"%Y-%m-%dT%H:%M:%SZ")" \
   '.last_updated = $ts' \
   .project/state/focus.json > /tmp/focus.json && \
   mv /tmp/focus.json .project/state/focus.json
```

Quality gates enforced:
- ✓ All tests passing
- ✓ Build successful
- ✓ Acceptance criteria met
- ✓ Code quality validated

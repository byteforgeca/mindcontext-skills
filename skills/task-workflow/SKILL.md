---
name: task-workflow
description: Execute complete task lifecycle from analysis through implementation, testing, review, and completion with quality gates. Use when user says "work on task", "implement task", "do task", or "start task".
---

# Task Workflow

Execute complete task lifecycle with developer-agent orchestration.

Invoke developer-agent in implementation mode with task context to execute the full workflow:

1. **Load task** - Read task file from `.project/epics/{epic}/{number}.md`
2. **Analyze** - Understand requirements and existing patterns
3. **Implement** - Write code following architecture
4. **Validate** - Build, test, verify
5. **Review** - Check acceptance criteria
6. **Complete** - Update status, track progress

The developer-agent will:
- Read task file and epic context
- Check dependencies before starting
- Create implementation plan from acceptance criteria
- Write code matching existing patterns
- Run build and tests to validate
- Verify all acceptance criteria are met
- Update task status to `done` when complete
- Track progress in `.project/context/progress.md`
- Suggest commit message

Quality gates enforced:
- ✓ All tests passing
- ✓ Build successful
- ✓ Acceptance criteria met
- ✓ Code quality validated

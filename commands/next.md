---
description: Continue working on current task or find next available task
---

# Next

Continue implementation or find the next thing to work on.

## Priority Order

**1. Check focus state (continue current work)**
   - If `.project/state/focus.json` has current task
   - Continue working on that task
   - Use any feedback from `/update-plan`

**2. Find next available task (standard workflow)**
   - Search `.project/epics/` for ready tasks
   - Invoke next-task skill
   - Start working on next ready task

## Usage

```bash
/next
```

Works with MindContext Epic/Task workflow.

See also: `/update-plan` for incorporating external LLM review feedback

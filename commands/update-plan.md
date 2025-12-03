---
description: Incorporate external LLM review feedback
---

# Update Plan

Incorporate feedback from external LLM (ChatGPT, Claude web, etc.) into current work context.

## Usage

After reviewing current task/issue with another LLM:

```
/update-plan

[Paste the review feedback here]
```

Claude will:
1. Read current context (focused issue from .project/state/focus.json)
2. Incorporate the review feedback into understanding
3. Update issue file if improvements suggested
4. Remember feedback for implementation

Then use `/next` to implement with the improved plan.

## Example Workflow

```bash
# 1. Working on an issue
You: "work on task 3"
Claude: [Loads .project/epics/auth/003.md, starts analysis]

# 2. Review with external LLM
You: [Copy issue 003.md]
You → ChatGPT: "Review this implementation plan"
ChatGPT: "Add rate limiting and use httpOnly cookies for security"

# 3. Update with feedback
You: /update-plan
You: [Paste ChatGPT's feedback]
Claude: [Updates issue 003.md with security improvements]

# 4. Continue implementation
You: /next
Claude: [Implements with improved plan]
```

## Tips

- Works with current focused issue from .project/epics/
- No separate plan.md needed - updates the issue file directly
- Feedback is incorporated into existing MindContext structure
- Use /next to implement step-by-step

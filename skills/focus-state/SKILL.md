---
name: focus-state
description: Manage current work focus (epic/issue/branch). Use when user says "what am I working on", "focus on epic X", "focus on issue Y", or "clear focus".
---

# Focus State

Track and manage current work context in `.project/state/focus.json`.

## When to Use

- User asks "what am I working on?"
- Setting current epic: "focus on epic auth"
- Setting current issue: "focus on issue 003"
- Clearing focus: "clear focus"

## Operations

### Show Current Focus

```bash
cat .project/state/focus.json | jq -r '
  "Current Focus:",
  "  Epic: \(.current_epic // "none")",
  "  Issue: \(.current_issue // "none")",
  "  Branch: \(.current_branch // "none")",
  "  Updated: \(.last_updated // "never")"
'
```

### Set Epic Focus

```bash
EPIC="$1"
jq --arg epic "$EPIC" --arg ts "$(date -u +"%Y-%m-%dT%H:%M:%SZ")" \
  '.current_epic = $epic | .last_updated = $ts' \
  .project/state/focus.json > /tmp/focus.json && \
  mv /tmp/focus.json .project/state/focus.json
```

### Set Issue Focus

```bash
ISSUE="$1"
jq --arg issue "$ISSUE" --arg ts "$(date -u +"%Y-%m-%dT%H:%M:%SZ")" \
  '.current_issue = $issue | .last_updated = $ts' \
  .project/state/focus.json > /tmp/focus.json && \
  mv /tmp/focus.json .project/state/focus.json
```

### Clear Focus

```bash
jq --arg ts "$(date -u +"%Y-%m-%dT%H:%M:%SZ")" \
  '.current_epic = null | .current_issue = null | .current_branch = null | .last_updated = $ts' \
  .project/state/focus.json > /tmp/focus.json && \
  mv /tmp/focus.json .project/state/focus.json
```

## Notes

- Requires `jq` for JSON manipulation
- Auto-updates timestamp on changes
- Used by other skills to understand context

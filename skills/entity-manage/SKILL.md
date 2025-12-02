---
name: entity-manage
description: Unified CRUD operations for PRD, Epic, and Issue entities with intelligent agent routing. Use when user says "create prd", "edit epic", "show issue", "list prds", "close epic", "sync issue", or "prd status".
---

# Entity Manage

Unified CRUD operations for project management entities with intelligent agent routing.

## When to Use

- Creating, editing, viewing, or managing PRDs, Epics, or Issues
- User says "create prd [name]", "edit epic [name]", "show issue [number]"
- User says "list prds", "list epics", "list issues in [epic]"
- User says "close epic [name]", "sync issue [number]", "prd status"

## Supported Entities

| Entity | Path |
|--------|------|
| PRD | `.project/prds/[name].md` |
| Epic | `.project/epics/[name]/epic.md` |
| Issue | `.project/epics/[epic]/[number].md` |

## Supported Operations

| Operation | Description |
|-----------|-------------|
| `create` | Create new entity with discovery/planning |
| `edit` | Modify existing entity |
| `show` | Display entity content |
| `list` | List entities of type |
| `close` | Mark entity as complete |
| `sync` | Sync to GitHub |
| `status` | Show entity status summary |

## Agent Routing Matrix

| Operation | Agent | Model | Reason |
|-----------|-------|-------|--------|
| `create` (prd) | pm-agent | opus | Strategic discovery, BMAD methodology |
| `create` (epic) | architect-agent | opus | Technical decomposition |
| `create` (issue) | developer-agent | opus | Implementation planning |
| `edit` | inline | haiku | Simple file modification |
| `show` | inline | haiku | Read and display |
| `list` | inline | haiku | Directory listing |
| `close` | inline | haiku | Status update |
| `sync` | developer-agent | opus | GitHub CLI integration |
| `status` | inline | haiku | Quick status check |

## Workflow

### Phase 1: Parse Request

Extract entity type and operation from user input:

```
Input: "create prd user-authentication"
  → Entity: prd
  → Operation: create
  → Name: user-authentication

Input: "show issue 3 in auth-epic"
  → Entity: issue
  → Operation: show
  → Name: 003
  → Parent: auth-epic

Input: "list epics"
  → Entity: epic
  → Operation: list
  → Name: (all)
```

**Parsing Rules:**
- Entity type: prd, epic, issue (case-insensitive)
- Issue numbers: Normalize to 3-digit format (3 → 003)
- Names: Convert to kebab-case if needed

### Phase 2: Resolve Paths

Check for MindContext structure:

```bash
# Check for MindContext structure
if [ -d ".project" ]; then
    BASE_PATH=".project"
else
    echo "No .project/ structure found. Run 'initialize project' first."
    exit 1
fi
```

**Path Resolution:**
```
PRD:   .project/prds/[name].md
Epic:  .project/epics/[name]/epic.md
Issue: $BASE_PATH/epics/[epic-name]/[number].md
```

### Phase 3: Route to Handler

#### CREATE Operations (Agent-Routed)

**Create PRD:**
```yaml
Task:
  description: "Create PRD: [name]"
  subagent_type: "pm-agent"
  model: opus
  prompt: |
    Create a comprehensive PRD for: [name]

    Use BMAD methodology:
    1. Conduct strategic discovery through questioning
    2. Analyze the problem deeply
    3. Create PRD with all sections
    4. Save to: [BASE_PATH]/prds/[name].md

    Follow your agent persona for quality standards.
```

**Create Epic:**
```yaml
Task:
  description: "Create Epic: [name]"
  subagent_type: "architect-agent"
  model: opus
  prompt: |
    Create a technical epic for: [name]

    1. Read the PRD: [BASE_PATH]/prds/[name].md
    2. Design technical architecture
    3. Define implementation phases
    4. Create epic structure
    5. Save to: [BASE_PATH]/epics/[name]/epic.md

    Follow your agent persona for architecture decisions.
```

**Create Issue:**
```yaml
Task:
  description: "Create Issue: [number] in [epic]"
  subagent_type: "developer-agent"
  model: opus
  prompt: |
    Create a task/issue for epic: [epic]

    1. Read epic: [BASE_PATH]/epics/[epic]/epic.md
    2. Analyze requirements for task [number]
    3. Define acceptance criteria
    4. Create task file
    5. Save to: [BASE_PATH]/epics/[epic]/[number].md

    Follow your agent persona for implementation planning.
```

#### SHOW Operation (Inline)

```bash
# Determine file path based on entity type
case $ENTITY in
    prd)
        FILE="$BASE_PATH/prds/$NAME.md"
        ;;
    epic)
        FILE="$BASE_PATH/epics/$NAME/epic.md"
        ;;
    issue)
        FILE="$BASE_PATH/epics/$EPIC/$NUMBER.md"
        ;;
esac

# Check if exists
if [ ! -f "$FILE" ]; then
    echo "Entity not found: $FILE"
    exit 1
fi

# Display
cat "$FILE"
```

**Output:**
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PRD: user-authentication
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Status: backlog
Created: 2025-01-15

[Full PRD content...]
```

#### LIST Operation (Inline)

**List PRDs:**
```bash
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
echo "PRDs"
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"

for file in $BASE_PATH/prds/*.md; do
    if [ -f "$file" ]; then
        name=$(basename "$file" .md)
        status=$(grep -m1 "^status:" "$file" | cut -d: -f2 | tr -d ' ')
        echo "  $name ($status)"
    fi
done
```

**List Epics:**
```bash
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
echo "Epics"
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"

for dir in $BASE_PATH/epics/*/; do
    if [ -d "$dir" ]; then
        name=$(basename "$dir")
        epic_file="$dir/epic.md"
        if [ -f "$epic_file" ]; then
            status=$(grep -m1 "^status:" "$epic_file" | cut -d: -f2 | tr -d ' ')
            progress=$(grep -m1 "^progress:" "$epic_file" | cut -d: -f2 | tr -d ' ')
            echo "  $name ($status) - $progress"
        fi
    fi
done
```

**List Issues in Epic:**
```bash
EPIC_DIR="$BASE_PATH/epics/$EPIC_NAME"

echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
echo "Issues in: $EPIC_NAME"
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"

for file in "$EPIC_DIR"/[0-9]*.md; do
    if [ -f "$file" ]; then
        num=$(basename "$file" .md)
        name=$(grep -m1 "^name:" "$file" | cut -d: -f2 | xargs)
        status=$(grep -m1 "^status:" "$file" | cut -d: -f2 | tr -d ' ')
        echo "  #$num: $name ($status)"
    fi
done
```

#### EDIT Operation (Inline)

```bash
# Read current content
FILE="[resolved path]"
cat "$FILE"

# Ask user what to change
echo ""
echo "What would you like to change?"
# User provides changes

# Apply edits using Edit tool
# Simple modifications handled inline
```

#### CLOSE Operation (Inline)

```bash
FILE="[resolved path]"

# Update status in frontmatter
sed -i 's/^status:.*/status: completed/' "$FILE"

# Add completion timestamp if not present
if ! grep -q "^completed:" "$FILE"; then
    sed -i "/^status:/a completed: $(date -u +"%Y-%m-%dT%H:%M:%SZ")" "$FILE"
fi

echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
echo "[ENTITY]: [name]"
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
echo "Status: completed"
echo "Closed: $(date)"
```

#### SYNC Operation (Agent-Routed)

```yaml
Task:
  description: "Sync [entity] to GitHub"
  subagent_type: "developer-agent"
  model: opus
  prompt: |
    Sync [entity_type] [name] to GitHub:

    1. Read entity: [file_path]
    2. Check if GitHub issue/PR exists (frontmatter github: field)
    3. If not exists:
       - Create GitHub issue: gh issue create --title "[name]" --body "$(cat [file])"
       - Update frontmatter with issue number
    4. If exists:
       - Update GitHub issue: gh issue edit [number] --body "$(cat [file])"

    Return: GitHub issue URL
```

#### STATUS Operation (Inline)

**PRD Status:**
```bash
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
echo "PRD Status"
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"

total=$(ls -1 $BASE_PATH/prds/*.md 2>/dev/null | wc -l)
backlog=$(grep -l "^status: backlog" $BASE_PATH/prds/*.md 2>/dev/null | wc -l)
active=$(grep -l "^status: active" $BASE_PATH/prds/*.md 2>/dev/null | wc -l)
completed=$(grep -l "^status: completed" $BASE_PATH/prds/*.md 2>/dev/null | wc -l)

echo "Total: $total"
echo "Backlog: $backlog"
echo "Active: $active"
echo "Completed: $completed"
```

**Epic Status:**
```bash
EPIC_FILE="$BASE_PATH/epics/$NAME/epic.md"

echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"
echo "Epic: $NAME"
echo "━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━"

status=$(grep -m1 "^status:" "$EPIC_FILE" | cut -d: -f2 | xargs)
progress=$(grep -m1 "^progress:" "$EPIC_FILE" | cut -d: -f2 | xargs)

echo "Status: $status"
echo "Progress: $progress"
echo ""
echo "Issues:"

total=0
open=0
closed=0

for file in "$BASE_PATH/epics/$NAME"/[0-9]*.md; do
    if [ -f "$file" ]; then
        total=$((total + 1))
        issue_status=$(grep -m1 "^status:" "$file" | cut -d: -f2 | tr -d ' ')
        if [ "$issue_status" = "completed" ] || [ "$issue_status" = "closed" ]; then
            closed=$((closed + 1))
        else
            open=$((open + 1))
        fi
    fi
done

echo "  Total: $total"
echo "  Open: $open"
echo "  Closed: $closed"
```

## Error Handling

### Entity Not Found
```
Entity not found: [path]

Did you mean?
  - [similar entity name 1]
  - [similar entity name 2]

To create: "create [entity] [name]"
```

### Invalid Operation
```
Invalid operation: [operation] for [entity]

Supported operations:
  - create: Create new [entity]
  - edit: Modify existing [entity]
  - show: Display [entity] content
  - list: List all [entity]s
  - close: Mark [entity] as complete
  - sync: Sync [entity] to GitHub
  - status: Show [entity] status
```

### Missing Name
```
Missing [entity] name

Usage:
  "create prd [name]"
  "show epic [name]"
  "list issues in [epic-name]"
```

### No Project Structure
```
No project structure found.

To initialize:
  "initialize project"

Or create manually:
  mkdir -p .project/prds .project/epics .project/context
```

## Trigger Phrases

### Create
- "create prd [name]", "new prd [name]", "write prd for [name]"
- "create epic [name]", "new epic [name]", "plan epic [name]"
- "create issue [number] in [epic]", "add task to [epic]"

### Show/View
- "show prd [name]", "view prd [name]", "display prd [name]"
- "show epic [name]", "view epic [name]"
- "show issue [number]", "view task [number]"

### Edit/Update
- "edit prd [name]", "update prd [name]", "modify prd [name]"
- "edit epic [name]", "update epic [name]"
- "edit issue [number]", "update task [number]"

### List
- "list prds", "show all prds", "what prds exist"
- "list epics", "show all epics"
- "list issues in [epic]", "show tasks in [epic]"

### Close
- "close prd [name]", "complete prd [name]"
- "close epic [name]", "finish epic [name]"
- "close issue [number]", "complete task [number]"

### Sync
- "sync prd [name]", "push prd to github"
- "sync epic [name]", "sync epic to github"
- "sync issue [number]", "push issue to github"

### Status
- "prd status", "prds status", "how many prds"
- "epic status [name]", "status of epic [name]"
- "project status", "overall status"

## Output Format

### Single Entity
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[ENTITY TYPE]: [Name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Status: [status]
Created: [date]
[Additional metadata...]

[Content or action result]
```

### List
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[ENTITY TYPE]s
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  [name-1] (status)
  [name-2] (status)
  [name-3] (status)

Total: [count]
```

### Status Summary
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[ENTITY TYPE] Status
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Total: [count]
Backlog: [count]
Active: [count]
Completed: [count]
```

## Notes

- Uses `.project/` structure (MindContext standard)
- Agent-routed operations use opus model for highest quality
- Simple operations use haiku model for speed
- Issue numbers auto-normalize to 3-digit format
- All entities use frontmatter for metadata
- GitHub sync requires `gh` CLI to be installed and authenticated

---
name: epic-create
description: Convert PRD to Epic with technical architecture. Use when user says "create epic", "convert PRD to epic", or "design epic for".
---

# Epic Create

Convert a Product Requirements Document into a technical Epic.

## When to Use

- Have PRD ready to convert
- User says "create epic {name}", "convert {name} PRD to epic"
- Need technical design for feature

## Workflow

### 1. Validate PRD Exists

```bash
PRD_FILE=".project/prds/$NAME.md"
if [ ! -f "$PRD_FILE" ]; then
    echo "PRD not found: $PRD_FILE"
    echo "Create PRD first: 'create prd $NAME'"
    exit 1
fi
```

### 2. Create Epic Directory

```bash
EPIC_DIR=".project/epics/$NAME"
mkdir -p "$EPIC_DIR"
```

### 3. Instructions for Architect Agent

When main Claude agent spawns architect-agent, it should:

1. **Read PRD:** Load `.project/prds/{name}.md`
2. **Analyze requirements:**
   - Business goals
   - User needs
   - Constraints
3. **Design architecture:**
   - Technology stack decisions
   - System components
   - Data models
   - API design
   - Security approach
4. **Create epic.md** with this structure:

```markdown
---
name: {name}
prd: {name}
status: planning
created: {timestamp}
github_label: epic:{name}
---

# Epic: {name}

## Technical Overview
[High-level architecture summary]

## Architecture Decisions

### Technology Stack
- [Choices with rationale]

### System Components
- [Services, modules, packages]

### Data Models
- [Schemas, relationships]

### API Design
- [Endpoints, contracts]

### Security Approach
- [Authentication, authorization, data protection]

## Implementation Phases

### Phase 1: Foundation
- [Base infrastructure tasks]

### Phase 2: Core Features
- [Main functionality tasks]

### Phase 3: Integration
- [External system integration]

### Phase 4: Polish
- [Testing, docs, optimization]

## Success Criteria
- [Technical validation points]

## Dependencies
- [External libraries, services, teams]

## Risks & Mitigations
- [Technical risks and how to address]
```

5. **Save to:** `.project/epics/{name}/epic.md`

### 4. Confirmation

```
EPIC CREATED: .project/epics/{name}/epic.md

Next steps:
1. Review epic architecture
2. Decompose into tasks: "plan epic {name}"
3. Start work: "start epic {name}"
```

## Notes

- Architect agent already exists (agents/architect-agent.md)
- Epic structure follows MindContext standard
- GitHub label format: epic:{name}
- Next step is epic-planning skill (already exists)

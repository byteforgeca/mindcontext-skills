---
name: project-migrate
description: Migrate existing project management files to FlowForge structure. Use when user says "migrate project", "convert to flowforge", "cleanup pm structure", or has existing legacy-structure/custom PM files.
---

# Project Migrate

Migrate existing project management structures to FlowForge's standard layout, identifying duplicates and organizing context documents.

## When to Use

- Project has existing PM files (legacy-structure/, .project/, custom structure)
- User says "migrate to flowforge", "convert project", "cleanup structure"
- Existing `.claude/commands/` or `.claude/skills/` that overlap with FlowForge
- Context files scattered in various locations
- Moving from another PM system

## What It Does

1. **Scan** - Find existing PM structures and files
2. **Analyze** - Identify what exists and what overlaps with FlowForge
3. **Plan** - Create migration plan with user approval
4. **Migrate** - Move/reorganize files to standard structure
5. **Cleanup** - Remove duplicates, archive old files
6. **Report** - Summary of changes made

## Workflow

### Phase 1: Discovery

**Step 1.1: Scan for Existing Structures**

Check common locations:
```bash
# Check for existing PM directories
ls -la legacy-structure/ 2>/dev/null && echo "Found: legacy-structure/"
ls -la .project/ 2>/dev/null && echo "Found: .project/"
ls -la .pm/ 2>/dev/null && echo "Found: .pm/"
ls -la docs/pm/ 2>/dev/null && echo "Found: docs/pm/"

# Check for Claude customizations
ls -la .claude/commands/ 2>/dev/null && echo "Found: .claude/commands/"
ls -la .claude/skills/ 2>/dev/null && echo "Found: .claude/skills/"
ls -la .claude/agents/ 2>/dev/null && echo "Found: .claude/agents/"
ls -la .claude/rules/ 2>/dev/null && echo "Found: .claude/rules/"

# Check for context files in various locations
find . -name "progress.md" -type f 2>/dev/null
find . -name "*context*.md" -type f 2>/dev/null
find . -name "*overview*.md" -type f 2>/dev/null
```

**Step 1.2: Identify PM Files**

Look for:
- PRDs: `**/prd*.md`, `**/requirements*.md`
- Epics: `**/epic*.md`, `**/epics/**/*.md`
- Tasks: `**/task*.md`, `**/issues/**/*.md`
- Context: `**/context/*.md`, `**/progress.md`

**Step 1.3: Discovery Report**

```
📂 DISCOVERY REPORT

Existing Structures Found:
  ├── legacy-structure/              [X files]
  │   ├── context/       [context files]
  │   ├── epics/         [epic directories]
  │   └── prds/          [PRD files]
  ├── .claude/
  │   ├── commands/      [X commands]
  │   ├── skills/        [X skills]
  │   └── agents/        [X agents]
  └── docs/              [scattered docs]

Files Identified:
  PRDs: [count]
  Epics: [count]
  Tasks: [count]
  Context docs: [count]
  Custom commands: [count]
  Custom skills: [count]
```

### Phase 2: Analysis

**Step 2.1: Check for FlowForge Overlaps**

Compare existing commands/skills with FlowForge:

| FlowForge Skill | Existing Equivalent | Action |
|-----------------|---------------------|--------|
| prd-create | /pm:prd-new | Replace |
| epic-planning | /pm:epic-decompose | Replace |
| task-workflow | legacy-structure-task skill | Replace |
| start-of-day | /sod command | Replace |
| ... | ... | ... |

**Step 2.2: Categorize Files**

```
FILE ANALYSIS

To Migrate (move to .project/):
  - legacy-structure/context/progress.md → .project/context/
  - legacy-structure/epics/* → .project/epics/
  - legacy-structure/prds/* → .project/prds/
  - docs/context/*.md → .project/context/

To Remove (replaced by FlowForge):
  - .claude/commands/pm/*.md (X files)
  - .claude/skills/legacy-structure-*/ (X skills)

To Keep (not duplicated):
  - .claude/commands/custom/*.md
  - .claude/agents/specialized-agent.md

To Archive (old/unused):
  - legacy-structure/archive/*
  - old-docs/*
```

**Step 2.3: Identify Context Priority**

Order context files by importance:
1. project-overview.md (or equivalent)
2. tech-context.md (or equivalent)
3. progress.md
4. project-structure.md
5. Other context files

### Phase 3: Migration Plan

**Step 3.1: Present Plan to User**

```
📋 MIGRATION PLAN

Phase A: Create Structure
  mkdir -p .project/{prds,epics,context}

Phase B: Migrate Content
  1. Move PRDs:
     - legacy-structure/prds/feature.md → .project/prds/feature.md
     - [X more files]

  2. Move Epics:
     - legacy-structure/epics/auth/ → .project/epics/auth/
     - [X more directories]

  3. Move Context:
     - legacy-structure/context/progress.md → .project/context/progress.md
     - docs/overview.md → .project/context/project-overview.md
     - [X more files]

Phase C: Remove Superseded Directories
  Remove (FlowForge plugin provides these):
    - .claude/agents/        (plugin provides agents)
    - .claude/commands/      (plugin provides commands)
    - .claude/skills/        (plugin provides skills)
    - .claude/scripts/       (no longer needed)

  Keep:
    - .claude/mcp.json       (Serena and other MCP servers)
    - CLAUDE.md              (project-specific instructions)

Phase D: Consolidate Context Files
  Move to .project/context/:
    - legacy-structure/context/*.md
    - docs/*context*.md
    - docs/*overview*.md
    - *progress*.md (from anywhere)
    - [X more scattered context files]

Phase E: Archive Old Structure
  Move to .archive/:
    - legacy-structure/ (entire directory)
    - .claude/agents/ (if not empty - archived before removal)
    - .claude/commands/ (if not empty - archived before removal)
    - .claude/skills/ (if not empty - archived before removal)

⚠️ CHANGES SUMMARY
  Files to move: [X]
  Directories to remove: 4 (.claude subdirectories)
  Context files to consolidate: [X]
  Files to archive: [X]
  Files to keep: [X] (mcp.json, CLAUDE.md)

Proceed with migration? (yes/no/modify)
```

**Step 3.2: Handle User Modifications**

If user wants changes:
- Allow selecting specific files to keep
- Allow skipping certain migrations
- Allow custom archive location

### Phase 4: Execute Migration

**Step 4.1: Create Target Structure**

```bash
# Create FlowForge structure
mkdir -p .project/prds
mkdir -p .project/epics
mkdir -p .project/context

# Create archive for old files
mkdir -p .archive/$(date +%Y%m%d)
```

**Step 4.2: Migrate Content Files**

```bash
# Move PRDs
mv legacy-structure/prds/*.md .project/prds/ 2>/dev/null

# Move epics (preserve structure)
cp -r legacy-structure/epics/* .project/epics/ 2>/dev/null

# Move context files
mv legacy-structure/context/*.md .project/context/ 2>/dev/null
```

**Step 4.3: Rename Legacy Structure References**

Update paths in migrated files:
- `legacy-structure/` → `.project/`
- `legacy-structure/context/` → `.project/context/`
- `legacy-structure/epics/` → `.project/epics/`

**Step 4.4: Remove Superseded FlowForge Directories**

When FlowForge is installed as a plugin, local `.claude/` directories become superseded. Remove them to avoid conflicts:

```bash
# Remove superseded directories (FlowForge plugin handles these now)
rm -rf .claude/agents/     # FlowForge plugin provides agents
rm -rf .claude/commands/   # FlowForge plugin provides commands
rm -rf .claude/skills/     # FlowForge plugin provides skills
rm -rf .claude/scripts/    # No longer needed with plugin system

# Keep these (not superseded):
# - .claude/mcp.json        (MCP server config like Serena)
# - CLAUDE.md               (Project-specific instructions)
```

**Important:** Only remove these if FlowForge plugin is installed:
```bash
# Verify FlowForge is installed first
/plugin list | grep flowforge-skills
```

If not installed, warn user:
```
⚠️  FlowForge plugin not detected

These directories will be removed:
  - .claude/agents/
  - .claude/commands/
  - .claude/skills/
  - .claude/scripts/

To avoid losing functionality, install FlowForge first:
  /plugin marketplace add tmsjngx0/flowforge-skills
  /plugin install flowforge-skills@tmsjngx0

Then run migration again.

Proceed anyway? (yes/no)
```

**Step 4.5: Move Context Files to .project/**

Find and consolidate all context files:

```bash
# Find all context-related files
find . -type f \( \
  -name "*context*.md" -o \
  -name "*progress*.md" -o \
  -name "*overview*.md" -o \
  -name "*tech*.md" -o \
  -name "project-*.md" \
  \) ! -path "./.git/*" ! -path "./node_modules/*" ! -path "./.project/*"

# Move to .project/context/ with smart naming
for file in $(find . -name "*context*.md" ! -path "./.project/*"); do
  basename=$(basename "$file")
  mv "$file" ".project/context/$basename"
done

# Handle scattered overview files
mv docs/project-overview.md .project/context/project-overview.md 2>/dev/null
mv docs/architecture.md .project/context/architecture.md 2>/dev/null
mv README-dev.md .project/context/development-guide.md 2>/dev/null
```

**Context File Priority:**

After moving, organize by priority in `.project/context/`:
1. `project-overview.md` - High-level project summary
2. `tech-context.md` - Technical stack and architecture
3. `progress.md` - Current state and recent work
4. `architecture.md` - Detailed system design
5. `development-guide.md` - Dev setup and conventions
6. Other context files

**Step 4.6: Remove Old Commands/Skills**

```bash
# Only if they weren't in .claude/ directories already removed
# Remove individual duplicate files if custom structure was used
find . -name "*prd-new*" -path "*/commands/*" -delete 2>/dev/null
find . -name "*epic-decompose*" -path "*/commands/*" -delete 2>/dev/null
# ... etc
```

**Step 4.7: Archive Old Structure**

```bash
# Archive the old legacy-structure directory
mv legacy-structure/ .archive/$(date +%Y%m%d)/legacy-structure/
```

### Phase 5: Verification

**Step 5.1: Verify New Structure**

```bash
# Check new structure exists
ls -la .project/
ls -la .project/prds/
ls -la .project/epics/
ls -la .project/context/
```

**Step 5.2: Verify No Broken References**

Search for old paths that need updating:
```bash
grep -r "legacy-structure/" . --include="*.md" 2>/dev/null
```

**Step 5.3: Verify FlowForge Skills Work**

```
Verification:
  [✓] .project/ structure created
  [✓] PRDs migrated ([X] files)
  [✓] Epics migrated ([X] directories)
  [✓] Context migrated ([X] files)
  [✓] Duplicates removed ([X] files)
  [✓] Old structure archived
  [✓] No broken references
```

### Phase 6: Report

**Final Migration Report:**

```
✅ MIGRATION COMPLETE

Summary:
  ├── PRDs: [X] migrated to .project/prds/
  ├── Epics: [X] migrated to .project/epics/
  ├── Context: [X] consolidated to .project/context/
  ├── Removed: .claude/{agents,commands,skills,scripts}/
  └── Archived: legacy-structure/ + old .claude/ → .archive/[date]/

New Structure:
  .project/
  ├── prds/
  │   └── [X] files
  ├── epics/
  │   └── [X] directories
  └── context/
      ├── project-overview.md
      ├── tech-context.md
      ├── progress.md
      └── [X] more files

  .claude/
  ├── mcp.json              (kept - Serena config)
  └── CLAUDE.md (moved to root if not there)

Removed Superseded Directories:
  - .claude/agents/         (FlowForge plugin provides)
  - .claude/commands/       (FlowForge plugin provides)
  - .claude/skills/         (FlowForge plugin provides)
  - .claude/scripts/        (no longer needed)

Archive Location:
  .archive/[date]/
  ├── legacy-structure/                 (old PM structure)
  ├── .claude-agents/       (old agents)
  ├── .claude-commands/     (old commands)
  └── .claude-skills/       (old skills)

FlowForge Skills Now Active:
  ✓ prd-create
  ✓ epic-planning
  ✓ task-workflow
  ✓ start-of-day
  ✓ [all skills]

Next Steps:
  1. Review migrated files in .project/
  2. Update any custom references to old paths
  3. Test FlowForge skills: "create prd for test"
  4. Delete .archive/ when confident migration is complete
```

## Rollback

If migration needs to be undone:

```bash
# Restore from archive
cp -r .archive/[date]/legacy-structure/ ./legacy-structure/

# Remove new structure
rm -rf .project/

# Note: Removed commands/skills need manual restoration
# Check git history or restore from backup
```

## Migration Mappings

### Directory Mappings
| Old Location | New Location |
|--------------|--------------|
| `legacy-structure/` | `.project/` |
| `legacy-structure/prds/` | `.project/prds/` |
| `legacy-structure/epics/` | `.project/epics/` |
| `legacy-structure/context/` | `.project/context/` |

### Command → Skill Mappings
| Old Command | FlowForge Skill |
|-------------|-----------------|
| `/pm:prd-new` | `prd-create` |
| `/pm:epic-decompose` | `epic-planning` |
| `/pm:epic-start` | `epic-start` |
| `/pm:issue-start` | `task-start` |
| `/sod` | `start-of-day` |
| `/ct:prime` | `prime-context` |

### Skill Replacements
| Old Skill | FlowForge Skill |
|-----------|-----------------|
| `legacy-structure-task` | `task-workflow` |
| `legacy-structure-merge` | `merge-workflow` |
| `pm-create` | `prd-create` |
| `pm-plan` | `epic-planning` |

## Notes

- Always creates archive before removing files
- Preserves epic directory structures
- Updates path references in migrated files
- Keeps non-duplicate custom commands/skills
- Provides rollback instructions
- User approval required before destructive changes

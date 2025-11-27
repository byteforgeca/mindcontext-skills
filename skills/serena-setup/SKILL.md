---
name: serena-setup
description: Install and configure Serena MCP server for intelligent code analysis. Use when user says "setup serena", "install serena", "configure serena", or "enable code analysis".
---

# Serena Setup

Install and configure the Serena MCP server for intelligent code analysis in the current project.

## When to Use

- Setting up a new project for development
- User says "setup serena", "install serena", "configure serena"
- User wants "code analysis", "symbol search", "intelligent editing"
- FlowForge agents need Serena tools for efficient operation

## What is Serena?

Serena is an MCP (Model Context Protocol) server that provides intelligent code analysis tools:

| Tool | Purpose |
|------|---------|
| `get_symbols_overview` | Get file/directory structure without reading full content |
| `find_symbol` | Find classes, methods, functions by name |
| `find_referencing_symbols` | Trace where code is used |
| `search_for_pattern` | Regex search across codebase |
| `replace_symbol_body` | Replace entire method/class body |
| `insert_after_symbol` | Add code after existing symbol |
| `insert_before_symbol` | Add code before existing symbol |
| `rename_symbol` | Rename across codebase |

## Prerequisites

- Node.js 18+ or Python 3.10+
- Claude Code with MCP support
- Project with supported language (TypeScript, Python, Go, Rust, Java, C#, etc.)

## Workflow

### Phase 1: Check Prerequisites

```bash
# Check Node.js version
node --version

# Check if Claude MCP config exists
test -f ~/.claude/mcp.json && echo "MCP config exists" || echo "MCP config not found"

# Check if project has supported files
ls -la *.ts *.py *.go *.rs *.java *.cs 2>/dev/null | head -5
```

### Phase 2: Install Serena

**Option A: Using npx (Recommended)**

No installation needed - Serena runs via npx:

```bash
# Test Serena is accessible
npx @anthropic/serena --version
```

**Option B: Global Installation**

```bash
npm install -g @anthropic/serena
```

**Option C: Project-Local Installation**

```bash
npm install --save-dev @anthropic/serena
```

### Phase 3: Configure MCP

**Create or update MCP configuration:**

**Location:** `~/.claude/mcp.json` (global) or `.claude/mcp.json` (project)

```json
{
  "mcpServers": {
    "serena": {
      "command": "npx",
      "args": ["@anthropic/serena"],
      "cwd": "${workspaceFolder}"
    }
  }
}
```

**For project-specific config (.claude/mcp.json):**

```json
{
  "mcpServers": {
    "serena": {
      "command": "npx",
      "args": ["@anthropic/serena"],
      "cwd": "."
    }
  }
}
```

### Phase 4: Verify Installation

**Test Serena tools are available:**

```bash
# Start Claude Code and check for Serena tools
# Run this in Claude Code:
mcp__serena__get_symbols_overview(".")
```

**Expected output:**
```
Serena MCP server connected
Available tools:
  - mcp__serena__get_symbols_overview
  - mcp__serena__find_symbol
  - mcp__serena__find_referencing_symbols
  - mcp__serena__search_for_pattern
  - mcp__serena__replace_symbol_body
  - mcp__serena__insert_after_symbol
  - mcp__serena__insert_before_symbol
  - mcp__serena__rename_symbol
```

### Phase 5: Create Project Index (Optional)

For large projects, pre-index for faster searches:

```bash
# Create .serena directory for cache
mkdir -p .serena

# Add to .gitignore
echo ".serena/" >> .gitignore
```

## Configuration Options

### Basic Config (.claude/mcp.json)

```json
{
  "mcpServers": {
    "serena": {
      "command": "npx",
      "args": ["@anthropic/serena"],
      "cwd": "."
    }
  }
}
```

### With Custom Settings

```json
{
  "mcpServers": {
    "serena": {
      "command": "npx",
      "args": [
        "@anthropic/serena",
        "--exclude", "node_modules,dist,build",
        "--max-file-size", "100000"
      ],
      "cwd": "."
    }
  }
}
```

### For Monorepos

```json
{
  "mcpServers": {
    "serena": {
      "command": "npx",
      "args": ["@anthropic/serena"],
      "cwd": ".",
      "env": {
        "SERENA_ROOT": "${workspaceFolder}"
      }
    }
  }
}
```

## Supported Languages

Serena supports symbol extraction for:

| Language | File Extensions | Status |
|----------|-----------------|--------|
| TypeScript | `.ts`, `.tsx` | Full support |
| JavaScript | `.js`, `.jsx` | Full support |
| Python | `.py` | Full support |
| Go | `.go` | Full support |
| Rust | `.rs` | Full support |
| Java | `.java` | Full support |
| C# | `.cs` | Full support |
| C/C++ | `.c`, `.cpp`, `.h` | Basic support |
| Ruby | `.rb` | Basic support |
| PHP | `.php` | Basic support |

## Troubleshooting

### Serena Not Found

```
Error: Cannot find module '@anthropic/serena'

Solution:
1. Ensure Node.js 18+ is installed
2. Run: npm install -g @anthropic/serena
3. Or use npx: npx @anthropic/serena
```

### MCP Connection Failed

```
Error: MCP server failed to connect

Solution:
1. Check ~/.claude/mcp.json syntax
2. Verify cwd path exists
3. Restart Claude Code
4. Check Claude Code logs
```

### Symbols Not Found

```
Error: No symbols found in file

Possible causes:
1. File has syntax errors
2. Language not supported
3. File too large (>100KB default)

Solution:
1. Fix syntax errors
2. Check supported languages
3. Increase --max-file-size
```

### Performance Issues

```
Serena is slow on large codebase

Solutions:
1. Add exclusions: --exclude node_modules,dist,build,.git
2. Pre-index project: Run get_symbols_overview on startup
3. Use specific paths instead of "."
```

## Integration with FlowForge

Once Serena is configured, FlowForge agents automatically use Serena tools:

| Agent | Serena Tools Used |
|-------|-------------------|
| `architect-agent` | `get_symbols_overview`, `find_symbol`, `find_referencing_symbols`, `search_for_pattern` |
| `developer-agent` | All tools including `replace_symbol_body`, `insert_*`, `rename_symbol` |
| `qa-agent` | `get_symbols_overview`, `find_symbol`, `find_referencing_symbols`, `search_for_pattern` |

**Example: Architect using Serena**
```
"create epic user-auth"
  → architect-agent spawned
  → Uses mcp__serena__get_symbols_overview to understand existing services
  → Uses mcp__serena__find_symbol to examine auth patterns
  → Creates epic with architecture decisions
```

## Output Format

### Success

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SERENA SETUP COMPLETE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Installation: npx @anthropic/serena
Config: .claude/mcp.json (project-level)

Available Tools:
  mcp__serena__get_symbols_overview
  mcp__serena__find_symbol
  mcp__serena__find_referencing_symbols
  mcp__serena__search_for_pattern
  mcp__serena__replace_symbol_body
  mcp__serena__insert_after_symbol
  mcp__serena__insert_before_symbol
  mcp__serena__rename_symbol

FlowForge agents now have enhanced code analysis!

Test with: mcp__serena__get_symbols_overview("src/")
```

### Already Configured

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SERENA ALREADY CONFIGURED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Config found: .claude/mcp.json
Status: Active

To reconfigure: Delete .claude/mcp.json and run again
To test: mcp__serena__get_symbols_overview(".")
```

### Error

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SERENA SETUP FAILED
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Error: [specific error message]

Troubleshooting:
1. [specific step]
2. [specific step]

Manual setup:
  npm install -g @anthropic/serena
  # Then add to ~/.claude/mcp.json
```

## Notes

- Serena runs as an MCP server alongside Claude Code
- No data leaves your machine - all analysis is local
- Restart Claude Code after config changes
- Use project-level config for team consistency
- Add .serena/ to .gitignore for cache files

---
name: tdd-workflow
description: Test-Driven Development workflow - write failing tests first, then implement to pass. Use when user says "tdd", "test first", "red green refactor", or wants test-driven approach.
---

# TDD Workflow

Execute Test-Driven Development with the Red-Green-Refactor cycle.

Invoke developer-agent in TDD mode to implement features test-first:

## The TDD Cycle

```
🔴 RED    → Write failing test
🟢 GREEN  → Write minimal code to pass
🔄 REFACTOR → Improve code (tests stay green)
```

The developer-agent will:

### 🔴 RED Phase
- Understand the requirement or behavior to implement
- Write a test that describes expected behavior
- Run the test to verify it fails (for the right reason)
- Confirm test failure before proceeding

### 🟢 GREEN Phase
- Write minimal implementation to make test pass
- Keep it simple - just enough to pass
- Run test again to verify it passes
- Run full test suite to check for regressions

### 🔄 REFACTOR Phase
- Identify code improvements (duplication, naming, complexity)
- Refactor safely while keeping tests green
- Run tests after each change
- Commit when refactoring complete

### Repeat
- Add more tests for edge cases
- Cycle through RED → GREEN → REFACTOR
- Continue until feature complete

Best practices enforced:
- ✅ ONE test at a time
- ✅ Run tests after every change
- ✅ Minimal implementations
- ✅ Refactor only when tests pass
- ✅ Test behavior, not implementation

The agent will use appropriate test framework:
- JavaScript/TypeScript: Jest/Vitest
- Python: pytest
- Rust: cargo test
- Go: go test
- C#: xUnit/NUnit
- Java: JUnit

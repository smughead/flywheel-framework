---
name: test
description: Runs project tests. See tech-stacks/ for stack-specific test commands. Use after making code changes, before committing, or when verifying functionality. Trigger phrases include "run tests", "unit tests", "test suite", "verify tests pass".
user-invocable: true
allowed-tools: Bash, Read, Glob, Grep
---

# Test Project

Run all tests for the project and report results.

## Example Prompts

### 1. "Run the tests"
> Executes all tests and reports pass/fail summary

### 2. "Do the tests pass after my changes?"
> Runs test suite to verify changes don't break existing functionality

### 3. "Run tests before I commit"
> Confirms all tests pass before committing code

## Instructions

1. Check `.claude/CLAUDE.md` or project configuration for test commands
2. Run the appropriate test command for the tech stack
3. Parse and report test results:
   - Total tests, passed, failed, skipped
   - Failure details with file locations
   - Offer to fix failing tests if they occur

## Common Test Commands

Test command depends on your tech stack:

| Stack | Test Command |
|-------|-------------|
| **Swift/Xcode** | `xcodebuild test -scheme [Scheme] -destination 'platform=macOS'` |
| **TypeScript** | `npm test` or `npx jest` or `npx vitest` |
| **Python** | `pytest` or `python -m pytest` |
| **Rust** | `cargo test` |
| **Go** | `go test ./...` |

## Stack-Specific Skills

For specialized test configurations, see:
- `tech-stacks/swift/skills/test/` - Xcode/XcodeBuildMCP test integration
- `tech-stacks/typescript/skills/test/` - Jest/Vitest/Mocha configurations
- `tech-stacks/python/skills/test/` - pytest/unittest configurations

## Detection

If no test command is specified, look for:
- `package.json` with test script → `npm test`
- `pytest.ini` or `pyproject.toml` with pytest config → `pytest`
- `Cargo.toml` → `cargo test`
- `go.mod` → `go test ./...`
- `*.xcodeproj` or `*.xcworkspace` → Xcode test scheme

## Important

Check the project structure to determine if tests are in a separate directory or scheme. Common patterns:
- `tests/` or `__tests__/` directories
- `*_test.go`, `*_test.py`, `*.test.ts` file patterns
- Separate test schemes in Xcode projects

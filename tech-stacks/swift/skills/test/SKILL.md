---
name: test
description: Runs all project tests using XcodeBuildMCP. Use after making code changes, before committing, or when verifying functionality works correctly. Trigger phrases include "run tests", "unit tests", "test suite", "verify tests pass", "check if tests work", "run the tests".
user-invocable: true
allowed-tools: Read, Glob, Grep
---

# Test Project

Run all tests for the project on macOS and report results.

## Example Prompts

### 1. "Run the tests"
> Executes all tests and reports pass/fail summary

### 2. "Do the tests pass after my changes?"
> Runs test suite to verify changes don't break existing functionality

### 3. "Run tests before I commit"
> Confirms all tests pass before committing code

## Instructions

1. **Set session defaults** to use the test scheme (if separate from main scheme):
   ```
   session-set-defaults({ scheme: "[ProjectTests]" })
   ```

2. **Run tests** using XcodeBuildMCP `test_macos` tool

3. **Parse test results** and report:
   - Total tests, passed, failed, skipped
   - Failure details with file locations
   - Offer to fix failing tests if they occur

4. **Restore session defaults** to main scheme after testing (if changed):
   ```
   session-set-defaults({ scheme: "[ProjectScheme]" })
   ```

## Important

Check the project structure to determine if tests are in a separate scheme or the main scheme.

## Fallback

If XcodeBuildMCP is unavailable:
```bash
xcodebuild test -scheme [ProjectTestsScheme] -destination 'platform=macOS' -quiet
```

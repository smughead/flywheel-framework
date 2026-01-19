---
name: workflows-verify
description: Chain build, test, and review in a sub-agent to verify implementation. Use after coding, before committing, or asking "verify my changes", "is this ready to commit", "check everything". Runs /build -> /test -> stack-specific review sequentially, stops on failure.
user-invocable: true
allowed-tools: Read, Glob, Grep, Bash, Task
---

# Workflows: Verify

Run the full verification chain (build -> test -> review) in a sub-agent, keeping the main context lean.

## Example Prompts

### 1. "Verify my changes"
> Runs build, test, review sequentially; reports pass/fail summary

### 2. "Is this ready to commit?"
> Full verification chain with actionable report

### 3. "/workflows:verify --skip-review"
> Build and test only, skip code review

## Syntax

```
/workflows:verify [--skip-review] [--skip-test]
```

- (default) - Full chain: build -> test -> review
- `--skip-review` - Build and test only
- `--skip-test` - Build and review only (for non-testable changes)

## Workflow

### Step 1: Launch Verification Agent

Use the Task tool to run verification in a sub-agent:

```
subagent_type: "general-purpose"
prompt: |
  Run the project verification chain. Stop immediately if any step fails.

  ## Step 1: Build
  Run the project build command (check CLAUDE.md or detect from project files).
  - If build fails: Report "BUILD FAILED" with error summary, stop here
  - If build succeeds: Continue to Step 2

  ## Step 2: Test
  Run the project test command (check CLAUDE.md or detect from project files).
  - If tests fail: Report "TESTS FAILED" with failure summary, stop here
  - If tests pass: Continue to Step 3

  ## Step 3: Review (unless --skip-review)
  Run any stack-specific review agents on changed files (use git diff --name-only).
  Check tech-stacks/[stack]/agents/ for available reviewers.

  ## Final Report

  Create a verification report at: .claude/reports/verify-[timestamp].md

  Format:
  ```markdown
  # Verification Report

  **Date:** [timestamp]
  **Status:** PASSED / FAILED

  ## Build
  - Status: [PASSED/FAILED]
  - Warnings: [count or "none"]

  ## Tests
  - Status: [PASSED/FAILED/SKIPPED]
  - Passed: [X]
  - Failed: [Y]
  - Skipped: [Z]

  ## Code Review
  - Status: [PASSED/NEEDS ATTENTION/FAILED/SKIPPED]
  - Critical: [count]
  - Warnings: [count]
  - Info: [count]

  ## Issues to Address
  [List any failures or critical issues]

  ## Recommendation
  [Ready to commit / Fix issues first]
  ```

  Return a one-line summary for the main context:
  - If all passed: "PASSED - Build, tests, and review all passed. Ready to commit."
  - If issues: "FAILED - [brief summary of what failed]"
```

### Step 2: Report to Main Context

The sub-agent returns a minimal summary. Report to the user:

**If PASSED:**
```
Verification PASSED

Build: OK
Tests: X passed
Review: No critical issues

Ready to commit. Consider /compound to document learnings.
```

**If FAILED:**
```
Verification FAILED

[Which step failed]
[Brief summary of issues]

Full report: .claude/reports/verify-[timestamp].md

Would you like me to fix these issues?
```

## Context Savings

| Approach | Context Used |
|----------|--------------|
| Manual /build + /test + review | ~8-15K tokens |
| /workflows:verify | ~500 tokens |

The detailed output stays in the sub-agent and report file, not the main context.

## Failure Handling

The verification chain stops at the first failure:

1. **Build fails** -> Report errors, offer to fix compilation issues
2. **Tests fail** -> Report failures, offer to fix failing tests
3. **Review fails** -> Report critical issues, offer to address them

## After Verification

**If PASSED:**
- Commit your changes
- Run `/compound` to document learnings

**If FAILED:**
- Fix the reported issues
- Run `/workflows:verify` again

## Related Skills

- `/workflows:plan` - Plan before implementing
- `/compound` - Document learnings after completion
- `/build` - Build only (manual)
- `/test` - Test only (manual)
- Stack-specific review skills in `tech-stacks/[stack]/skills/`

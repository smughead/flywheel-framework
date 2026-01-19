---
created: 2026-01-17
last_verified: 2026-01-17
---

# Pattern: Verification Report File Structure

## When to Apply

- Generating verification reports via `/workflows:verify`
- Reviewing reports for completeness
- Troubleshooting verification failures from past sessions
- Understanding what was verified before a commit

## Why It Matters

Reports are ephemeral verification records (not curated knowledge). They should be:
- Scannable in under 15 seconds for pass/fail status
- Detailed enough to diagnose failures
- Consistent for comparison across sessions
- Concise to avoid cluttering the reports directory

## The Pattern

### Target Length: 40-60 lines (max 80)

Reports should capture verification results without verbose explanations. If a report exceeds 80 lines, the review section is likely too detailed.

### Required Structure

```markdown
# Verification Report

**Date:** YYYY-MM-DD
**Status:** PASSED / FAILED

## Build
- Status: PASSED/FAILED
- Warnings: [count or "none"]

## Tests
- Status: PASSED/FAILED/SKIPPED
- Passed: [X]
- Failed: [Y]
- Skipped: [Z]

## Code Review
- Status: PASSED/NEEDS ATTENTION/FAILED/SKIPPED
- Critical: [count]
- Warnings: [count]
- Info: [count]

### Review Agent Results (if review ran)
[One-line summary per agent]

### Files Reviewed (if review ran)
[Bullet list of files]

### Compiler Warnings (informational)
[Brief list if relevant]

## Issues to Address
[List failures or "None"]

## Recommendation
[Ready to commit / Fix issues first]
```

### Naming Convention

```
verify-YYYY-MM-DD[-suffix].md
```

Examples:
- `verify-2026-01-17.md` - Standard daily report
- `verify-2026-01-17-ai-coaching.md` - Feature-specific report
- `verify-2026-01-17-hotfix.md` - Context-specific suffix

### What NOT to Include

- Full compiler output (summarize warning count only)
- Complete test logs (just pass/fail counts)
- Extended code review commentary (keep to one line per agent)
- Embedded code snippets (reference files instead)

## Example

Reference: `.claude/reports/verify-2026-01-15-workflow-audit.md` (42 lines) - Good example of concise report.

Key elements:
- Lines 1-5: Header with date and overall status
- Lines 6-14: Build and test summaries
- Lines 15-27: Code review with file table
- Lines 38-42: Issues and recommendation

## Anti-Pattern

```markdown
# Verification Report

## Build
Build succeeded after running xcodebuild with the following output:

[100 lines of xcodebuild output]

## Tests
Here's what each test did:

[50 lines of test descriptions]
```

**Problems:**
- Full output instead of summaries
- No clear pass/fail status at top
- Too long (150+ lines)
- Verbose descriptions instead of bullet summaries

## Validation Checklist

- [ ] Length under 80 lines (target: 40-60)
- [ ] Overall status (PASSED/FAILED) in header
- [ ] Build section with status and warning count
- [ ] Tests section with pass/fail/skip counts
- [ ] Code Review section with issue counts
- [ ] Issues to Address section (even if "None")
- [ ] Recommendation section
- [ ] No embedded full output (summaries only)

## References

- Source: `.claude/skills/workflows-verify/SKILL.md:62-89` (template definition)
- Example: `.claude/reports/verify-2026-01-15-workflow-audit.md` (42 lines)
- Example: `.claude/reports/verify-2026-01-16.md` (79 lines)

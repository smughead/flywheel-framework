---
paths:
  - ".claude/reports/**/*.md"
---

# Report File Audit

When reviewing, creating, or auditing verification report files, apply this checklist.

## Pattern Reference

See @.claude/knowledge/patterns/report-file-structure.md for required structure.

## Quick Checklist

- [ ] Length under 80 lines (target: 40-60)
- [ ] Overall status (PASSED/FAILED) in header
- [ ] Date in header (YYYY-MM-DD format)
- [ ] Build section with status and warning count
- [ ] Tests section with pass/fail/skip counts
- [ ] Code Review section with issue counts
- [ ] Issues to Address section present
- [ ] Recommendation section present
- [ ] No embedded full output (summaries only)

## Length Guidelines

| Lines | Status | Action |
|-------|--------|--------|
| 40-60 | Ideal | Concise, scannable |
| 60-80 | Acceptable | Review for verbosity |
| 80+ | Too long | Trim review details, summarize warnings |

## Required Sections

| Section | Purpose | Check |
|---------|---------|-------|
| **Header** | Date and overall PASSED/FAILED | Top of file |
| **Build** | Status + warning count | Present |
| **Tests** | Pass/fail/skip counts | Present |
| **Code Review** | Issue counts by severity | Present |
| **Issues to Address** | Actionable failures | Present (even if "None") |
| **Recommendation** | Ready to commit / Fix first | Present |

## Naming Convention

```
verify-YYYY-MM-DD[-suffix].md
```

| Pattern | Example | Use Case |
|---------|---------|----------|
| `verify-YYYY-MM-DD.md` | `verify-2026-01-17.md` | Standard daily |
| `verify-YYYY-MM-DD-[feature].md` | `verify-2026-01-17-ai-coaching.md` | Feature-specific |

## Audit Workflow

1. **Length check** - Under 80 lines?
2. **Header check** - Date and status present?
3. **Section check** - All 5 sections present?
4. **Verbosity check** - Summaries not full output?
5. **Naming check** - Follows `verify-YYYY-MM-DD[-suffix].md`?
6. **Self-grade & offer improvements**

## Quality Grades

| Grade | Criteria |
|-------|----------|
| **A** | Under 60 lines, all sections, clear pass/fail, proper naming |
| **B** | Under 80 lines, all sections, minor verbosity |
| **C** | Over 80 lines or missing 1 section |
| **D** | Over 100 lines or missing header/recommendation |

## Self-Grade & Improve

After auditing, present grade and offer improvements:

> **Audit Grade: [B]**
>
> Issues found:
> - Report is 79 lines (target: 40-60)
> - Review Agent Results section could be condensed
>
> Would you like me to trim the verbose sections?

## Anti-Patterns

- **Full output embedded** - Include summary counts, not raw output
- **Missing status in header** - Must have PASSED/FAILED at top
- **No recommendation** - Always end with actionable next step
- **Verbose review details** - One line per agent, not paragraphs
- **Duplicate information** - Don't repeat issues in multiple sections

## Past Learnings

See @.claude/knowledge/learnings/2026-01-17-report-file-audit-standards.md for context on why these standards matter.

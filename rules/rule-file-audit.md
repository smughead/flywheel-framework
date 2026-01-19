---
paths:
  - ".claude/rules/**/*.md"
---

# Rule File Audit

When reviewing, creating, or auditing rule files, apply this checklist.

## Pattern Reference

See @.claude/knowledge/patterns/rule-file-structure.md for required structure.

## Quick Checklist

- [ ] YAML frontmatter with `paths:` field
- [ ] Pattern Reference section linking to corresponding pattern file
- [ ] Quick Checklist with actionable checkboxes
- [ ] Audit Workflow with exactly 6 steps
- [ ] Quality Grades table with A/B/C/D tiers
- [ ] Self-Grade & Improve section with template
- [ ] Anti-Patterns section
- [ ] Past Learnings section

## Section Consistency

All rule files should use these exact section names:

| Section | Required |
|---------|----------|
| Pattern Reference | Yes |
| Quick Checklist | Yes |
| Audit Workflow | Yes |
| Quality Grades | Yes (not "Quality Indicators") |
| Self-Grade & Improve | Yes |
| Anti-Patterns | Yes |
| Past Learnings | Yes |

## Audit Workflow

1. **Frontmatter check** - YAML `paths:` field present and valid
2. **Pattern Reference check** - Links to corresponding pattern file
3. **Structure check** - All required sections present
4. **Consistency check** - Section names match standard naming
5. **Grades check** - Uses A/B/C/D tiers, not binary
6. **Self-grade & offer improvements**

## Quality Grades

| Grade | Criteria |
|-------|----------|
| **A** | All 8 sections present, consistent naming, valid path triggers |
| **B** | 7 sections present, minor naming inconsistencies |
| **C** | 5-6 sections present, missing Past Learnings or Anti-Patterns |
| **D** | Missing YAML paths, Pattern Reference, or Quality Grades |

## Self-Grade & Improve

After auditing, present grade and offer improvements:

> **Audit Grade: [B]**
>
> Issues found:
> - Missing Past Learnings section
> - "Quality Indicators" should be "Quality Grades"
>
> Would you like me to fix these?

## Anti-Patterns

- **No YAML paths** - Rule won't auto-trigger on file access
- **Missing Pattern Reference** - No link to structure documentation
- **Binary grades** - "Good/Bad" instead of A/B/C/D nuance
- **Missing Self-Grade** - No improvement feedback loop
- **Inconsistent section names** - "Quality Indicators" vs "Quality Grades"

## Past Learnings

This rule was created during the 2026-01-17 rules directory audit to enable self-auditing of rule files and ensure consistency across the `.claude/rules/` directory.

---
created: 2026-01-17
last_verified: 2026-01-17
---

# Pattern: Rule File Structure

## When to Apply

- Creating new rule files in `.claude/rules/`
- Auditing existing rules for consistency
- Understanding how rules auto-trigger on file patterns
- Deciding what content belongs in a rule vs a pattern

## Why It Matters

Rules are auto-triggered checklists that fire when Claude touches matching files. Without consistent structure:
- Audits miss checks (inconsistent workflows)
- Quality grades vary (different criteria per rule)
- Self-improvement feedback is absent (no grade + offer)
- Context is missing (no link to learnings that shaped the rule)

## The Pattern

### Required Structure

```markdown
---
paths:
  - "path/to/files/**/*.md"
---

# [File Type] Audit

When reviewing, creating, or auditing [file type], apply this checklist.

## Pattern Reference

See @.claude/knowledge/patterns/[file-type]-file-structure.md for required structure.

## Quick Checklist

- [ ] Item 1
- [ ] Item 2
- [ ] ...

## [Domain-Specific Section]

[Guidelines specific to this file type]

## Audit Workflow

1. **First check** - Description
2. **Second check** - Description
3. **Third check** - Description
4. **Fourth check** - Description
5. **Fifth check** - Description
6. **Self-grade & offer improvements**

## Quality Grades

| Grade | Criteria |
|-------|----------|
| **A** | Best quality criteria |
| **B** | Good quality criteria |
| **C** | Acceptable with issues |
| **D** | Failing criteria |

## Self-Grade & Improve

After auditing, present grade and offer improvements:

> **Audit Grade: [X]**
>
> Issues found:
> - Issue 1
> - Issue 2
>
> Would you like me to fix these?

## Anti-Patterns

- Anti-pattern 1
- Anti-pattern 2

## Past Learnings

See @.claude/knowledge/learnings/[related-learning].md for context.
```

### YAML Frontmatter

The `paths:` field triggers the rule when Claude touches matching files:

```yaml
---
paths:
  - ".claude/knowledge/learnings/**/*.md"  # Learning files
  - ".claude/plans/**/*.md"                 # Plan files
  - ".claude/reports/**/*.md"               # Report files
---
```

### Six-Step Audit Workflow

All rules should have exactly 6 audit steps:
1. Primary structure/format check
2. Secondary structure check
3. Reference/link validation
4. Content quality check
5. Style/convention check
6. Self-grade & offer improvements

### Four-Tier Quality Grades

Use consistent grading across all rules:

| Grade | General Meaning |
|-------|----------------|
| **A** | Exceeds standards, exemplary |
| **B** | Meets standards, minor issues |
| **C** | Below standards, needs work |
| **D** | Fails essential requirements |

## Example

Reference: `.claude/rules/pattern-file-audit.md`

Key elements:
- Lines 1-4: YAML paths frontmatter
- Lines 8-12: Pattern Reference section
- Lines 14-21: Quick Checklist
- Lines 37-46: Six-step Audit Workflow
- Lines 48-55: Four-tier Quality Grades
- Lines 57-66: Self-Grade & Improve template
- Lines 72-74: Past Learnings section

## Anti-Pattern

```markdown
# Some Audit

Check these things:
- Thing 1
- Thing 2

Grade: Good or Bad
```

**Problems:**
- No YAML paths (won't auto-trigger)
- No Pattern Reference (no link to structure doc)
- No Audit Workflow (inconsistent review process)
- Binary grades (no A/B/C/D nuance)
- No Self-Grade & Improve template
- No Past Learnings section

## Validation Checklist

- [ ] YAML frontmatter with `paths:` field
- [ ] Pattern Reference section linking to pattern file
- [ ] Quick Checklist with actionable checkboxes
- [ ] Six-step Audit Workflow ending with "Self-grade & offer improvements"
- [ ] Quality Grades table with A/B/C/D tiers
- [ ] Self-Grade & Improve section with template
- [ ] Anti-Patterns section
- [ ] Past Learnings section linking to relevant learnings

## References

- Rule: [rule-file-audit.md](../../rules/rule-file-audit.md)
- Example: [pattern-file-audit.md](../../rules/pattern-file-audit.md)
- Meta: [pattern-file-structure.md](./pattern-file-structure.md)

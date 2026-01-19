---
paths:
  - ".claude/knowledge/patterns/**/*.md"
---

# Pattern File Audit

When reviewing, creating, or auditing pattern files, apply this checklist.

## Pattern Reference

See @.claude/knowledge/patterns/pattern-file-structure.md for required structure.

## Quick Checklist

- [ ] YAML frontmatter with `created` and `last_verified` dates
- [ ] "When to Apply" section immediately after title
- [ ] "Why It Matters" section explains consequences
- [ ] Code uses file:line references (embedded only if < 10 lines AND generic)
- [ ] Cross-references use markdown `[text](path)` syntax
- [ ] No duplicate content (consolidated or redirect stub)
- [ ] `last_verified` is recent (within 30 days)

## File:Line Reference Format

```markdown
# Good
Reference: `AudioManager.swift:23-45`
See implementation: `ContentView.swift:100-120`

# Bad (will go stale)
```swift
let manager = AudioManager()
// ... 50 lines of code
```
```

## Audit Workflow

1. **Frontmatter check** - Dates present and formatted correctly
2. **Structure check** - "When to Apply" and "Why It Matters" sections exist
3. **Reference check** - Code references use file:line, links use markdown syntax
4. **Freshness check** - `last_verified` is recent
5. **Duplication check** - No overlapping patterns exist
6. **Self-grade & offer improvements**

## Quality Grades

| Grade | Criteria |
|-------|----------|
| **A** | All checks pass, verified recently, excellent examples |
| **B** | All checks pass, minor staleness acceptable |
| **C** | 5+ checks pass, needs freshening |
| **D** | Missing frontmatter or "When to Apply" |

## Self-Grade & Improve

After auditing, present grade and offer improvements:

> **Audit Grade: [B]**
>
> Areas that could be improved:
> - `last_verified` is 45 days old
> - One code block should be file:line reference
>
> Would you like me to update these?

## Past Learnings

See @.claude/knowledge/learnings/2026-01-16-pattern-file-audit-standards.md for context on why these standards matter.

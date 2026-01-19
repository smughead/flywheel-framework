---
paths:
  - ".claude/plans/**/*.md"
---

# Plan File Audit

When reviewing, creating, or auditing implementation plan files, apply this checklist.

## Pattern Reference

See @.claude/knowledge/patterns/plan-file-structure.md for required structure.

## Quick Checklist

- [ ] Length under 150 lines (target: 50-100 for scannability)
- [ ] Research Summary links to pattern/learning files (not inline findings)
- [ ] Implementation steps reference target files AND pattern files
- [ ] Code snippets < 15 lines (extract larger snippets to patterns)
- [ ] Verification section with build/test/review/manual checks
- [ ] Questions section for open user decisions
- [ ] Files Summary at end (new vs modified)

## Length Guidelines

| Lines | Status | Action |
|-------|--------|--------|
| 50-100 | Ideal | Scannable, focused |
| 100-150 | Acceptable | Consider extracting code |
| 150+ | Too long | Must extract to pattern files |

## Code Reference Format

```markdown
# Good - reference pattern file
**Pattern:** `state-management-observable.md`
- Follow TranscriptStore pattern
- Add `@MainActor` + `@Observable`

# Bad - embedded code that will go stale
```swift
@MainActor
@Observable
final class MyStore {
    // 50 lines...
}
```
```

## Required Sections

| Section | Purpose | Check |
|---------|---------|-------|
| **Research Summary** | Link to knowledge base findings | Must reference pattern/learning files |
| **Approach** | Why this solution | Summary, justification, alternatives |
| **Implementation Steps** | Actionable steps | Each step → file + pattern |
| **Verification** | How to validate | build/test/review/manual |
| **Risks** | What could go wrong | Table with mitigations |
| **Questions** | User decisions needed | Open items for clarification |
| **Files Summary** | Scope of changes | New vs modified files |

## Audit Workflow

1. **Length check** - Under 150 lines? Target 50-100
2. **Structure check** - All required sections present
3. **Reference check** - Links to patterns/learnings, not inline content
4. **Code check** - Snippets < 15 lines, otherwise extracted
5. **Verification check** - Build/test/review/manual items present
6. **Self-grade & offer improvements**

## Quality Grades

| Grade | Criteria |
|-------|----------|
| **A** | Under 100 lines, all sections, proper references, no embedded code |
| **B** | Under 150 lines, all sections, mostly references |
| **C** | Over 150 lines or missing 1-2 sections |
| **D** | Missing Verification, Questions, or Files Summary |

## Self-Grade & Improve

After auditing, present grade and offer improvements:

> **Audit Grade: [C]**
>
> Issues found:
> - Plan is 328 lines (target: 50-100)
> - 100+ lines of embedded Swift code
> - Research Summary has inline findings instead of pattern links
>
> Recommended actions:
> - Extract code to pattern file `foundation-models-coaching.md`
> - Replace inline findings with links to patterns
>
> Would you like me to refactor this plan?

## Anti-Patterns

- **Embedded code blocks** - Will go stale; extract to pattern files
- **Inline research findings** - Duplicates knowledge base; link instead
- **Missing Verification** - No way to confirm implementation complete
- **No Questions section** - User decisions assumed without confirmation
- **No Files Summary** - Scope of changes unclear

## Past Learnings

See @.claude/knowledge/learnings/2026-01-17-plan-file-audit-standards.md for context on why these standards matter.

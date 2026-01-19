---
created: 2026-01-17
last_verified: 2026-01-17
---

# Pattern: Implementation Plan File Structure

## When to Apply

- Creating new implementation plans in `.claude/plans/`
- Auditing existing plans for quality and scannability
- Reviewing plans before implementation begins
- Converting research findings into actionable plans

## Why It Matters

Plans guide implementation and serve as contracts between user and Claude. Without consistent structure:
- Long plans become hard to scan for current step
- Embedded code goes stale and misleads
- Missing verification steps lead to incomplete work
- Duplicate content between plans and patterns causes drift

## The Pattern

### Target Length: 50-100 lines (max 150)

Plans should be scannable in under 30 seconds. If a plan exceeds 150 lines, extract details to pattern files.

### Required Structure

```markdown
# Implementation Plan: [Feature Name]

## Research Summary
- [1-2 bullet summaries linking to pattern/learning files]

## Approach
### Summary
### Why This Approach
### Alternatives Considered

## Implementation Steps
### Phase 1: [Name]
- Step 1.1: [Action] → `File.swift`, `pattern.md`
- Step 1.2: [Action] → `File.swift`, `pattern.md`

## Verification
- [ ] /build - no warnings
- [ ] /test - all pass
- [ ] /review-swift - no critical issues
- [ ] Manual: [specific checks]

## Risks
| Risk | Mitigation |
|------|------------|

## Future Considerations (optional)
- [Ideas worth remembering but out of scope]
- [Potential enhancements for later phases]
- [Design decisions deferred intentionally]

## Questions
- [Open decisions for user]

## Files Summary
**New:** list
**Modified:** list
```

### Code Snippet Policy

**Reference, don't embed.** Extract code to pattern files.

**Do this:**
```markdown
#### Step 1.1: Create Store
**File:** `CueLoop/CoachingStore.swift` (new)
**Pattern:** `state-management-observable.md`
- Follow TranscriptStore pattern
- Add `@MainActor` + `@Observable`
- Max 10 items, oldest-first pruning
```

**Not this:**
```markdown
#### Step 1.1: Create Store
```swift
@MainActor
@Observable
final class CoachingStore {
    // 50 lines of code that will go stale
}
```

### When to Extract to Pattern Files

Extract when:
- Code snippet > 15 lines
- Implementation detail is reusable beyond this plan
- Multiple plans would benefit from same pattern

Keep inline when:
- Snippet is < 10 lines AND plan-specific
- Showing structure/approach, not implementation

## Example

Reference: `.claude/plans/feature-ai-coaching.md` (pre-refactor) shows what happens when plans grow too long—328 lines makes it hard to track current step.

Good examples of concise plans follow the 50-100 line target.

## Anti-Pattern

```markdown
# Implementation Plan: Feature X

## Step 1
```swift
// 100 lines of embedded code
```

## Step 2
```swift
// Another 100 lines
```
```

**Problems:**
- No Research Summary linking to knowledge base
- No Verification section
- Embedded code will go stale
- No Questions for user decisions
- No Files Summary

## Validation Checklist

- [ ] Length under 150 lines (target: 50-100)
- [ ] Research Summary links to patterns/learnings (not inline findings)
- [ ] Implementation steps reference files AND patterns
- [ ] Code snippets < 15 lines (otherwise extract to pattern)
- [ ] Verification section with build/test/review/manual
- [ ] Questions section for open decisions
- [ ] Files Summary at end (new vs modified)
- [ ] Future Considerations for deferred ideas (optional but encouraged)

## References

- Pattern: [pattern-file-structure.md](./pattern-file-structure.md)
- Template: `.claude/skills/plan-feature/references/plan-template.md`
- Example: `.claude/plans/feature-ai-coaching.md`

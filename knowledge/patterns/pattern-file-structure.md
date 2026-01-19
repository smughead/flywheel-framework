---
created: 2026-01-16
last_verified: 2026-01-16
---

# Pattern: Knowledge Base Pattern File Structure

## When to Apply

- Creating new pattern files in `.claude/knowledge/patterns/`
- Auditing existing pattern files for quality
- Updating pattern files after codebase changes
- Deciding whether to embed code or reference it
- Consolidating duplicate documentation

## Why It Matters

Pattern files are reference documentation that Claude consults when explicitly directed. Without consistent structure:
- Stale code examples mislead future development
- Missing applicability guidance causes pattern misuse
- Broken references create dead ends
- Duplicates cause confusion about authoritative source

## The Pattern

### Required Structure

```markdown
---
created: YYYY-MM-DD
last_verified: YYYY-MM-DD
---

# Pattern: [Descriptive Name]

## When to Apply

- Scenario 1 when this pattern is relevant
- Scenario 2 (observable symptom)
- Scenario 3 (question user might ask)

## Why It Matters

Brief explanation of consequences of not following this pattern.

## The Pattern

[Core content with file:line references]

## Example

[Concrete example with reference to live code]

## Anti-Pattern

[What NOT to do]

## Validation Checklist

- [ ] Actionable validation items

## References

- Learning: [link to related learning]
- Live example: [link to code file]
```

### File:Line References

Instead of embedding code blocks that go stale, reference the source:

**Do this:**
```markdown
See the audio format setup: `AudioProcessingPipeline.swift:45-52`

Key elements:
- Sample rate set at line 46
- Channel count at line 48
```

**Not this:**
```markdown
Here's how to set up audio format:
```swift
let format = AVAudioFormat(
    commonFormat: .pcmFormatFloat32,
    sampleRate: 48000,
    channels: 1,
    interleaved: false
)!
```
```

### When Embedded Code IS Appropriate

Embed code when:
- Showing conceptual patterns (not tied to specific implementation)
- The snippet is < 10 lines AND generic
- Demonstrating syntax or structure (not specific values)

### Cross-Reference Syntax

```markdown
# For pattern-to-pattern
See: [other-pattern.md](./other-pattern.md)

# For pattern-to-learning
Learning: [2026-01-16-example.md](../learnings/2026-01-16-example.md)

# For pattern-to-code (file:line)
Reference: `AudioManager.swift:23-45`

# For external links
Official: [Apple Documentation](https://developer.apple.com/...)
```

### Consolidation Redirects

When consolidating duplicates, leave a redirect stub:

```markdown
# [Old Pattern Name]

**This pattern has been consolidated.**

See: [canonical-pattern.md](./canonical-pattern.md)

---

*Consolidated: YYYY-MM-DD*
```

## Example

Reference: [agent-file-structure.md](./agent-file-structure.md) demonstrates this structure:
- Lines 1-4: YAML frontmatter
- Lines 5-12: "When to Apply" section
- Lines 91-127: References section with proper links

## Anti-Pattern

```markdown
# My Pattern

Here's some code:
```swift
// 100 lines of code copied from the codebase
// that will inevitably go stale
```

Just do this and it works.
```

**Problems:**
- No frontmatter (no verification date)
- No "When to Apply" (when do I use this?)
- Embedded code (will go stale)
- No references (can't verify accuracy)

## Validation Checklist

- [ ] YAML frontmatter with `created` and `last_verified` dates
- [ ] "When to Apply" section as first content section
- [ ] "Why It Matters" explains consequences
- [ ] Code references use file:line format (embedded < 10 lines only)
- [ ] Cross-references use markdown link syntax
- [ ] No duplicates (or redirect stub in place)
- [ ] `last_verified` within 30 days of current date

## References

- Learning: [2026-01-16-pattern-file-audit-standards.md](../learnings/2026-01-16-pattern-file-audit-standards.md)
- Example: [agent-file-structure.md](./agent-file-structure.md)
- Context: [claude-code-knowledge-architecture.md](./claude-code-knowledge-architecture.md)

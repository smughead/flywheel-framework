---
created: 2026-01-16
last_verified: 2026-01-16
---

# Pattern: Claude Code Knowledge Architecture

## When to Apply

- Deciding where to store project knowledge
- Knowledge not being applied automatically as expected
- Creating new rules, skills, or documentation
- Troubleshooting why Claude doesn't "remember" something

## The Pattern

Claude Code has three mechanisms for project knowledge, each with distinct activation behaviors:

### 1. CLAUDE.md - Global Project Context

**Activation:** Always loaded at session start

**Use for:**
- Project structure and navigation hints
- Global conventions that apply everywhere
- Build/test commands
- Developer preferences and communication style
- Links to other documentation

**Example:**
```markdown
# MyProject

## Development
Run tests with `npm test`

## Conventions
- Use TypeScript strict mode
- All APIs return Result types
```

### 2. Rules - Passive, Path-Specific Knowledge

**Activation:** Automatically loaded when working with matching paths

**Structure:**
```yaml
---
paths:
  - "src/components/**/*.tsx"
  - "src/hooks/**/*.ts"
---

# Component Rules

When creating or editing components:
- [ ] Export named, not default
- [ ] Props interface defined above component
```

**Use for:**
- File type conventions (e.g., all test files follow pattern X)
- Directory-specific patterns (e.g., all API routes need auth)
- Checklists triggered by path matching
- Automatic reminders without user intervention

**Key:** The `paths` frontmatter enables automatic loading.

### 3. Skills - Active, User-Invoked Workflows

**Activation:** User invokes with `/skillname` or Claude discovers via description matching

**Structure:**
```yaml
---
name: my-skill
description: Does X when user asks to "do X", "perform X", or needs X functionality.
---

# My Skill

Instructions for the skill...
```

**Use for:**
- Multi-step workflows
- Tasks requiring specific tool sequences
- Features discovered via natural language
- Actions explicitly requested by users

### 4. Knowledge Directory - Reference Material

**Activation:** Only when explicitly referenced or linked from rules/skills/CLAUDE.md

**Location:** `.claude/knowledge/`
- `patterns/` - Reusable implementation patterns
- `learnings/` - Past fixes and discoveries
- `reviews/` - Code review findings

**Use for:**
- Detailed documentation too long for rules
- Historical context and decisions
- Deep-dive reference material

## Decision Tree

```
Is this knowledge needed every session?
├── Yes → CLAUDE.md
└── No
    ├── Triggered by working with specific files?
    │   ├── Yes → Rule with paths frontmatter
    │   └── No
    │       ├── Triggered by user request?
    │       │   ├── Yes → Skill
    │       │   └── No → Knowledge directory (reference only)
    │       └── End
    └── End
```

## Common Mistakes

### Mistake 1: Expecting knowledge/ to auto-apply
```markdown
# Bad: Created pattern, expected automatic use
.claude/knowledge/patterns/my-pattern.md

# Fix: Reference it from a rule
.claude/rules/my-pattern-trigger.md
---
paths: ["src/**/*.ts"]
---
See @.claude/knowledge/patterns/my-pattern.md
```

### Mistake 2: Putting path-specific info in CLAUDE.md
```markdown
# Bad: CLAUDE.md with file-specific details
When editing test files, always use...

# Fix: Create a rule
.claude/rules/test-conventions.md
---
paths: ["**/*Test.swift", "**/*Tests.swift"]
---
```

### Mistake 3: Making skills for passive checks
```yaml
# Bad: Skill for something that should auto-apply
name: check-imports
description: Checks imports are sorted

# Fix: Rule that fires when editing source files
paths: ["src/**/*.ts"]
Imports should be sorted: external, then internal, then relative.
```

## Rule Frontmatter Reference

```yaml
---
paths:
  - "exact/path/file.md"       # Exact match
  - "src/**/*.ts"              # Glob: all .ts in src tree
  - ".claude/agents/**/*.md"   # Glob: all agent files
---
```

**Path matching:**
- `*` matches single directory level
- `**` matches any depth
- Paths relative to project root

## Validation Checklist

- [ ] Global conventions in CLAUDE.md?
- [ ] Path-specific knowledge in rules with `paths` frontmatter?
- [ ] User-invoked workflows as skills?
- [ ] Detailed reference docs in knowledge/ and linked appropriately?
- [ ] Rules are concise (auto-loaded = keep brief)?
- [ ] Skills have description with trigger terms?

## References

- Learning: [2026-01-16-rules-vs-skills-discovery.md](../learnings/2026-01-16-rules-vs-skills-discovery.md)
- Example rule: [agent-file-audit.md](../../rules/agent-file-audit.md)
- Official: [Claude Code Memory Documentation](https://docs.anthropic.com/en/docs/claude-code/memory)

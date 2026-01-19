# Pattern: Claude Code Agent File Structure

## When to Apply

- Creating new agent files in `.claude/agents/`
- Auditing existing agent files
- Troubleshooting agent discovery issues
- Adding agents to workflows or skills

## Why It Matters

Without proper frontmatter:
- Agent won't be discovered by Claude Code
- Trigger terms won't match user requests
- Tool permissions won't be set correctly
- Skills invoking the agent may fail silently

## The Pattern

All Claude Code agent files **must** have YAML frontmatter:

```yaml
---
name: agent-name
description: What this agent does. Use when [trigger scenarios], [keywords users would say].
tools: Read, Grep, Glob
model: sonnet
permissionMode: default
---

# Agent Title

[Agent instructions follow...]
```

### Required Fields

| Field | Purpose | Example |
|-------|---------|---------|
| `name` | Agent identifier (matches filename without .md) | `architecture-guardian` |
| `description` | Discovery matching + purpose (include trigger terms) | `"...Use when asked to 'check architecture'..."` |
| `tools` | Comma-separated list of allowed tools | `Read, Grep, Glob` |

### Optional Fields

| Field | Purpose | Default |
|-------|---------|---------|
| `model` | Model to use (`sonnet`, `opus`, `haiku`) | Inherits from parent |
| `permissionMode` | Permission handling (`default`, `acceptEdits`, `bypassPermissions`) | `default` |

### Tool Selection Guide

| Agent Type | Recommended Tools |
|------------|-------------------|
| Read-only reviewer | `Read, Grep, Glob` |
| Code modifier | `Read, Grep, Glob, Edit` |
| File creator | `Read, Grep, Glob, Write` |
| Full access | `Read, Grep, Glob, Edit, Write, Bash` |

**Principle:** Least privilege - only grant tools the agent actually needs.

### Description Trigger Terms

Write descriptions for how users ask, not just what the agent does:

```yaml
# Bad - no trigger terms
description: Reviews architecture for violations.

# Good - includes user phrases
description: Reviews architecture for violations. Use when reviewing PRs, after refactoring, or when asked to "check architecture", "review structure", "audit layers".
```

## Nested Code Blocks

When agent files contain code examples inside markdown code blocks, use escalating fence lengths:

```
`````markdown   <- 5 backticks (outermost)
# Example Output

```swift        <- 3 backticks (inner)
let x = 1
```

`````           <- 5 backticks close
```

**Hierarchy:** 5 > 4 > 3 backticks to prevent premature termination.

## Example

See a complete agent file with proper frontmatter:

**Reference:** [architecture-guardian.md:1-7](../../agents/architecture-guardian.md) (frontmatter)

Key elements demonstrated:
- `name` matches filename (`architecture-guardian`)
- `description` includes 4+ trigger phrases ("check architecture", "review structure", etc.)
- `tools` restricted to read-only (`Read, Grep, Glob`)
- `model` set to `sonnet` for analysis tasks

## Anti-Pattern

```markdown
# My Agent

This is an agent that does things...
```

**Problem:** No frontmatter = Claude Code won't discover this as an agent.

## Validation Checklist

- [ ] File starts with `---` on line 1
- [ ] Valid YAML syntax (no tabs, proper quoting)
- [ ] `name` matches filename
- [ ] `description` includes trigger phrases
- [ ] `tools` uses comma-separated format
- [ ] Nested code blocks use escalating fence lengths
- [ ] File referenced in any skills that invoke it

## References

- Learning: [2026-01-16-agent-yaml-frontmatter.md](../learnings/2026-01-16-agent-yaml-frontmatter.md)
- Official: [Claude Code Agent Skills Documentation](https://docs.anthropic.com/en/docs/claude-code/skills)
- Live example: [architecture-guardian.md](../../agents/architecture-guardian.md)

---

*Last verified: 2026-01-16*

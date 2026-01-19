# Pattern: Claude Code Skill File Structure

## When to Apply

- Creating new skill files in `.claude/skills/[skill-name]/SKILL.md`
- Auditing existing skill files for best practices
- Troubleshooting skill discovery issues
- Optimizing skill files for token efficiency

## Why It Matters

Without proper structure:
- Skill may not be discoverable by users
- Tool permissions may be too broad or restrictive
- Verbose prompts waste context tokens at runtime
- Agent prompts may not produce consistent results

## The Pattern

All Claude Code skill files **must** have YAML frontmatter and follow these guidelines:

```yaml
---
name: skill-name
description: What this skill does. Trigger phrases include "phrase 1", "phrase 2", "phrase 3".
user-invocable: true
allowed-tools: Read, Write, Edit, Glob, Grep, Task
---

# Skill Title

[Brief one-line description]

## Example Prompts

[2-3 example user requests with expected behavior]

## Workflow

[Step-by-step instructions, kept terse]
```

### Required Frontmatter Fields

| Field | Purpose | Example |
|-------|---------|---------|
| `name` | Skill identifier (matches folder name) | `compound` |
| `description` | Discovery matching + purpose (include trigger phrases) | `"...Trigger phrases include 'document learning'..."` |

### Recommended Frontmatter Fields

| Field | Purpose | Default |
|-------|---------|---------|
| `user-invocable` | Whether users can call directly with `/skillname` | `false` |
| `allowed-tools` | Comma-separated tools the skill can use | All tools |

### Tool Selection Guide

| Skill Type | Recommended Tools |
|------------|-------------------|
| Read-only analysis | `Read, Grep, Glob` |
| Documentation | `Read, Grep, Glob, Write` |
| Code modification | `Read, Grep, Glob, Edit` |
| Sub-agent workflow | `Read, Write, Edit, Glob, Grep, Task` |

**Principle:** Least privilege - only grant tools the skill actually needs.

## Agent Prompts in Skills

When skills launch parallel agents, keep prompts terse:

```markdown
# Bad - verbose (12 lines)
#### Agent 1: Context Analyzer
Analyze the current conversation to understand:
- What was the original problem or goal?
- What files were involved?
- What constraints existed?
- How did the approach evolve?

Output a brief markdown summary with:
- Problem statement (1-2 sentences)
- Files touched (bulleted list with paths)
- Key constraints or requirements

# Good - terse (5 lines)
#### Agent 1: Context Analyzer
```
subagent_type: "general-purpose"
prompt: |
  Summarize: original problem, files involved, constraints, how approach evolved.
  Output: markdown sections with specific file paths and error messages.
```
```

**Target:** Agent prompts should be 2-5 lines max with clear input/output expectations.

## Size Guidelines

| Metric | Target | Maximum |
|--------|--------|---------|
| Total lines | < 150 | 200 |
| Agent prompts | 2-5 lines each | 10 lines |
| Example prompts | 2-3 | 5 |

## Example

See a well-structured skill file:

**Reference:** [compound/SKILL.md:1-6](../../skills/compound/SKILL.md) (frontmatter)

Key elements demonstrated:
- `user-invocable: true` for direct invocation
- `allowed-tools` explicitly lists needed tools
- Agent prompts are 2-3 lines each
- Total file is under 175 lines

## Anti-Pattern

```markdown
---
name: my-skill
description: Does something.
---

# My Skill

This skill helps you do things by analyzing your code and understanding...

[500 lines of documentation and verbose agent prompts]
```

**Problems:**
- Missing `user-invocable` and `allowed-tools`
- Verbose introduction wastes tokens
- Agent prompts too long
- File exceeds 200 lines

## Validation Checklist

- [ ] File starts with `---` YAML frontmatter
- [ ] `name` matches folder name
- [ ] `description` includes trigger phrases
- [ ] `user-invocable` set if users can call directly
- [ ] `allowed-tools` explicitly listed
- [ ] Agent prompts under 5 lines each
- [ ] Total file under 200 lines
- [ ] No marketing/promotional sections

## References

- Learning: [2026-01-17-skill-file-audit-compound.md](../learnings/2026-01-17-skill-file-audit-compound.md)
- Related: [agent-file-structure.md](./agent-file-structure.md) (similar pattern for agents)
- Official: [Claude Code Skills Documentation](https://docs.anthropic.com/en/docs/claude-code/skills)

---

*Last verified: 2026-01-17*

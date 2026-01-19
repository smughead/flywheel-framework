# [Stack Name] Reference for Flywheel Framework

> **Template Instructions:** Copy this file and replace bracketed placeholders with your stack-specific content. Delete this instruction block when done.

This document contains [Stack Name]-specific patterns, review agents, and constraints for use with the [Methodology](../Methodology.md).

---

## Stack Differences: [Stack Name] vs Others

> **Instructions:** Identify what makes your stack different from others. This helps agents understand when stack-specific patterns apply.

| Aspect | Generic/Web | [Stack Name] | Implication |
|--------|-------------|--------------|-------------|
| **Build speed** | [comparison] | [your stack] | [what this means for workflow] |
| **Testing** | [comparison] | [your stack] | [what this means for workflow] |
| **Architecture** | [comparison] | [your stack] | [what this means for workflow] |
| **Deployment** | [comparison] | [your stack] | [what this means for workflow] |
| **Memory/Resources** | [comparison] | [your stack] | [what this means for workflow] |

---

## Build & Test Tools

> **Instructions:** Document the build and test commands for your stack.

### Primary Build Tools

```bash
# Build command
[your build command]

# Test command
[your test command]

# Clean command
[your clean command]
```

### Build Verification Checkpoints

> **Instructions:** When should developers verify builds during development?

1. **[Checkpoint 1]** — [when and why]
2. **[Checkpoint 2]** — [when and why]
3. **[Checkpoint 3]** — [when and why]

---

## Stack-Specific Review Agents

> **Instructions:** Define 3-6 review agents focused on your stack's unique concerns. Each agent should have a clear purpose, checklist, and prompt template.

### 1. [domain]-reviewer

**Purpose:** [What this agent checks for]

**Checks:**
- [Check item 1]
- [Check item 2]
- [Check item 3]
- [Check item 4]

**Prompt Template:**
```markdown
Review this [Stack Name] code for [domain]:

1. **[Category 1]:** [What to check, what to flag]

2. **[Category 2]:** [What to check, what to flag]

3. **[Category 3]:** [What to check, what to flag]

4. **[Category 4]:** [What to check, what to flag]

Files to review: [file paths]
```

### 2. [domain]-reviewer

**Purpose:** [What this agent checks for]

**Checks:**
- [Check item 1]
- [Check item 2]
- [Check item 3]

**Prompt Template:**
```markdown
[Your prompt template here]
```

### 3. [domain]-reviewer

**Purpose:** [What this agent checks for]

**Checks:**
- [Check item 1]
- [Check item 2]
- [Check item 3]

**Prompt Template:**
```markdown
[Your prompt template here]
```

> **Add more agents as needed for your stack (typically 3-6 total)**

---

## Critical Patterns

> **Instructions:** Document patterns that are unique or especially important for your stack.

### Pattern 1: [Pattern Name]

**The Rule:** [One-sentence summary]

**Why It Matters:** [What goes wrong if violated]

**Implementation:**
```[language]
// CORRECT example
[code example]

// INCORRECT example
[code example]
```

**Checklist:**
- [ ] [Check item 1]
- [ ] [Check item 2]
- [ ] [Check item 3]

### Pattern 2: [Pattern Name]

**The Rule:** [One-sentence summary]

**Why It Matters:** [What goes wrong if violated]

**Implementation:**
```[language]
[code examples]
```

---

## Environment & Configuration

> **Instructions:** Document environment setup, configuration files, and required permissions.

### Required Environment

| Requirement | Purpose |
|-------------|---------|
| [Tool/Version] | [Why needed] |
| [Tool/Version] | [Why needed] |

### Configuration Files

| File | Purpose |
|------|---------|
| [config file] | [What it configures] |
| [config file] | [What it configures] |

### Permissions/Access

> **Instructions:** Document any permissions, API keys, or access requirements.

| Permission | Purpose | How to Configure |
|------------|---------|------------------|
| [Permission] | [Why needed] | [Setup instructions] |

---

## Common Pitfalls

> **Instructions:** Document mistakes that are easy to make in your stack.

### 1. [Pitfall Name]
```[language]
// AVOID
[bad code]

// PREFER
[good code]
```

### 2. [Pitfall Name]
```[language]
// AVOID
[bad code]

// PREFER
[good code]
```

### 3. [Pitfall Name]
```[language]
// AVOID
[bad code]

// PREFER
[good code]
```

---

## Directory Structure

> **Instructions:** Show the recommended `.claude/` structure for your stack.

```
.claude/
├── knowledge/
│   ├── patterns/
│   │   ├── [pattern-1].md
│   │   ├── [pattern-2].md
│   │   └── [pattern-3].md
│   │
│   ├── learnings/
│   │   └── [date]-[feature].md
│   │
│   └── reviews/
│       ├── [domain-1]-checklist.md
│       └── [domain-2]-checklist.md
│
├── agents/
│   ├── [agent-1].md
│   ├── [agent-2].md
│   └── [agent-3].md
│
└── skills/
    ├── build/
    ├── test/
    └── [stack-specific-skill]/
```

---

## Related Resources

> **Instructions:** Link to documentation, tools, and references for your stack.

- **[Resource 1]:** [URL or description]
- **[Resource 2]:** [URL or description]
- **[Resource 3]:** [URL or description]

---

## Example: Swift/macOS Reference

> **Instructions:** See `swift-macos-reference.md` for a complete example of this template filled out for Swift/macOS development. Key sections to study:
>
> - **Stack Differences table:** Shows clear contrasts with web development
> - **6 Review Agents:** Each has purpose, checks, and prompt template
> - **Audio Threading Pattern:** Domain-specific pattern with code examples
> - **macOS Permissions:** Environment-specific configuration
> - **Common Pitfalls:** Stack-specific mistakes with before/after code

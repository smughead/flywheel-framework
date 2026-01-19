---
name: compound
description: Documents learnings from completed features or bug fixes into the knowledge base. Use after completing meaningful work, fixing a tricky bug, or discovering a reusable pattern worth remembering. Trigger phrases include "document this learning", "capture this pattern", "remember for next time", "save what we learned", "add to knowledge base", "write up the fix".
user-invocable: true
allowed-tools: Read, Write, Edit, Glob, Grep, Task
---

# Compound Learning

Capture learnings from completed work using parallel agents to make future work faster.

## Example Prompts

### 1. "Document what we learned fixing the Bluetooth audio bug"
> Launches 4 agents to analyze context, extract solution, identify pattern, write docs

### 2. "This sample rate handling should be a pattern"
> Extracts reusable pattern to patterns/ directory

### 3. "Save this fix so we don't hit it again"
> Documents the issue and solution in learnings/ for future searchability

## Workflow

### Step 1: Launch Parallel Analysis Agents

**CRITICAL:** Use the Task tool to launch these 4 agents in parallel (single message, multiple Task calls):

#### Agent 1: Context Analyzer
```
subagent_type: "general-purpose"
prompt: Summarize original problem, files involved, constraints, approach evolution. Output: markdown with file paths and error messages.
```

#### Agent 2: Solution Extractor
```
subagent_type: "general-purpose"
prompt: Extract approach, files modified, key decisions, mistakes corrected, root cause (if bug). Output: markdown with code references.
```

#### Agent 3: Pattern Identifier
```
subagent_type: "general-purpose"
prompt: Identify reusable pattern (if any), what to remember, review checklist updates. Categorize: audio | ui | testing | architecture | permissions | other
```

#### Agent 4: Documentation Writer
```
subagent_type: "general-purpose"
prompt: Generate learning doc and pattern doc (if applicable) using LEARNING_TEMPLATE and PATTERN_TEMPLATE from Templates section below.
```

### Templates

**LEARNING_TEMPLATE** (for `.claude/knowledge/learnings/YYYY-MM-DD-[name].md`):
```markdown
# Learning: [Feature/Fix Name]
## Problem
## Context
## Root Cause (if bug)
## Solution
## Pattern Identified
## Review Checklist Items
## Code Reference
## Related Learnings
```

**PATTERN_TEMPLATE** (for `.claude/knowledge/patterns/[name].md`, if applicable):
```markdown
# Pattern: [Pattern Name]
## When to Apply
## The Pattern
## Example
## Anti-Pattern
## References
```

### Step 2: Synthesize and Write Documents

After all agents complete, synthesize their findings:

1. **Create learning document:**
   - File: `.claude/knowledge/learnings/YYYY-MM-DD-[feature-name].md`
   - Use today's date and kebab-case name

2. **Create pattern document (if applicable):**
   - File: `.claude/knowledge/patterns/[pattern-name].md`
   - Only if Agent 3 identified a reusable pattern

3. **Update review checklist (if applicable):**
   - File: `.claude/knowledge/reviews/[relevant-checklist].md`
   - Add new items identified by Agent 3

### Step 3: Confirm to User

Report what was created:

```
Knowledge captured!

Created:
- Learning: .claude/knowledge/learnings/2026-01-14-[name].md
- Pattern: .claude/knowledge/patterns/[name].md (if applicable)
- Updated: .claude/knowledge/reviews/[name].md (if applicable)

Summary: [1-2 sentence summary of what was learned]

The compound effect is building - this learning will make similar work faster in the future.
```

### Step 4: Update SESSION_NOTES.md

After creating knowledge base documents, update SESSION_NOTES.md:

1. **Mark completed to-do** (if this work was tracked there)
2. **Add one-line summary** under a "Completed This Session" section if it doesn't exist
3. **Link to the learning document** created

**Example update:**
```markdown
## Completed This Session
- [x] Fix Bluetooth audio sample rate issue — see [learning](/.claude/knowledge/learnings/2026-01-14-bluetooth-sample-rate-fix.md)
```

This creates a breadcrumb trail for future sessions and ensures SESSION_NOTES stays current.

### Step 5: Prompt for Next Task

After updating SESSION_NOTES, end with:

---

**Knowledge captured!** SESSION_NOTES.md updated.

What would you like to work on next? You can:
- Pick another to-do from SESSION_NOTES.md
- Start a new feature with `/workflows:plan`
- End the session (I'll summarize what was accomplished)

---

## When to Use

Run `/compound` after:
- Completing a meaningful feature
- Fixing a tricky bug
- Discovering something non-obvious
- Making a decision worth remembering
- Finding a pattern that could be reused

**Skip it for:**
- Trivial fixes (typos, formatting)
- Work that doesn't teach anything new
- Changes covered by existing learnings

## File Naming

**Learnings:** `YYYY-MM-DD-[feature-name].md`
- `2026-01-14-bluetooth-sample-rate-fix.md`
- `2026-01-14-transcript-bubble-styling.md`

**Patterns:** `[pattern-name].md`
- `audio-threading.md`
- `swiftui-observation.md`

## Related Skills

- `/workflows:plan` - Plan before implementing
- `/workflows:verify` - Verify before committing

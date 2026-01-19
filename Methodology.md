# Flywheel Framework Methodology

> **Credits:** Flywheel Framework is inspired by [Compound Engineering](https://every.to/chain-of-thought/compound-engineering-how-every-codes-with-agents) (Every.to) and [Advanced Context Engineering](https://github.com/humanlayer/advanced-context-engineering-for-coding-agents) (HumanLayer), combined and extended with self-auditing rules and stack-agnostic design.

## Executive Summary

**Flywheel Framework** is an AI-assisted development methodology where each unit of work makes the next easier to build. Rather than treating features as isolated efforts, this approach systematically captures patterns, learnings, and review findings—creating a knowledge layer that grows with your project.

**Why it works:**
- Knowledge compounds: Today's discoveries become tomorrow's shortcuts
- AI agents reference accumulated context, not generic training data
- Review findings become guardrails that prevent repeated mistakes
- The system improves itself through self-auditing rules

**Two complementary disciplines:**
- **Compound Engineering** (from Every.to): How to make each feature easier than the last (knowledge accumulation)
- **Advanced Context Engineering** (from HumanLayer): How to use LLM context windows efficiently (context discipline)

This methodology combines both, adapted for any tech stack.

---

## Core Principles

### 1. Each Unit of Work Makes the Next Easier

This is the fundamental principle. Every completed feature should produce:
- **Patterns** — Reusable approaches for similar problems
- **Learnings** — Specific discoveries that prevent future mistakes
- **Review items** — Checklist entries that catch issues earlier

Without this discipline, AI assistance resets to zero with each session. With it, AI agents accumulate project-specific knowledge that compounds over time.

### 2. Knowledge is Structured, Not Scattered

Learnings must live in predictable locations with consistent formats. An agent searching for "how do we handle X?" should find a clear answer, not a vague memory in a random file.

### 3. Review Discovers What to Compound

The Assess phase isn't just validation—it's knowledge discovery. Review findings become:
- Learning documents (what we fixed)
- Pattern documents (what we should reuse)
- Checklist items (what we should check earlier)

### 4. Context Quality Over Context Size

The LLM context window is your only lever for output quality. Optimize in this priority order:

1. **Correctness (highest)** — Wrong information compounds into bad code
2. **Completeness (second)** — Missing constraints create gaps agents can't fill
3. **Size (lowest)** — Redundancy is better than gaps

### 5. Self-Auditing Rules Maintain Quality

Without enforcement, knowledge quality degrades. Self-auditing rules:
- Trigger automatically when knowledge files are created/modified
- Present standardized checklists
- Grade the work and offer improvements
- Ensure consistent quality across all artifacts

---

## 5-Phase Workflow

```
1. RESEARCH
   └─> Subagents explore codebase in isolation
   └─> Gather relevant patterns and constraints
   └─> Output: Short research summary + key file map

2. PLAN — The Thing You Validate
   └─> Design with project constraints in mind
   └─> Reference knowledge base for patterns
   └─> Output: Plan as the shared source of truth

   "Once you have a good plan, the hard part is almost over."
   Human review happens HERE, not during code review.

3. WORK
   └─> Execute with appropriate build tools
   └─> Build + test after each logical chunk
   └─> Output: Working, tested code

4. ASSESS — Knowledge Discovery, not just Validation
   └─> Multi-perspective review
   └─> Automated checks + manual verification
   └─> Output: Findings that become future guardrails

   The compounding happens here:
   - Review catches issue → becomes learning document
   - Review identifies pattern → becomes reusable pattern
   - Review flags risk → becomes checklist item

5. COMPOUND
   └─> Document learnings in knowledge base
   └─> Update review rubrics if new patterns emerge
   └─> Output: Reusable knowledge for future work
```

### Phase Outputs as Products

Each phase produces a **product** that outlives the session:

| Phase | Output | Format |
|-------|--------|--------|
| Research | Key files, constraints, unknowns | `.claude/plans/[feature]-research.md` |
| Plan | Steps, files to modify, verification criteria | `.claude/plans/[feature]-plan.md` |
| Work | Code, tests, build verification | Committed code |
| Assess | Findings, patterns identified, action items | `.claude/knowledge/reviews/[date]-[feature].md` |
| Compound | Problem→Solution→Pattern | `.claude/knowledge/learnings/[date]-[feature].md` |

---

## Directory Structure Template

Adapt this structure for your project:

```
.claude/
├── knowledge/                           # The "compound" layer
│   ├── patterns/                        # Reusable [TECH] patterns
│   │   ├── [domain]-[pattern].md       # e.g., threading.md, state-management.md
│   │   └── ...
│   │
│   ├── learnings/                       # Per-feature insights
│   │   ├── [date]-[feature].md         # e.g., 2025-01-11-auth-flow.md
│   │   └── ...
│   │
│   └── reviews/                         # Review rubrics
│       ├── [domain]-checklist.md       # e.g., security-checklist.md
│       └── ...
│
├── rules/                               # Self-auditing checklists (auto-triggered)
│   ├── pattern-file-audit.md           # Triggers when patterns/ touched
│   ├── learning-file-audit.md          # Triggers when learnings/ touched
│   ├── agent-file-audit.md             # Triggers when agents/ touched
│   └── ...
│
├── skills/                              # Workflow skills
│   ├── research/                        # Phase 1: Research with subagents
│   ├── plan/                            # Phase 2: Planning workflow
│   ├── verify/                          # Phase 4: Multi-agent review
│   └── compound/                        # Phase 5: Knowledge capture
│
├── agents/                              # Specialized reviewers
│   ├── [domain]-reviewer.md            # e.g., security-reviewer.md
│   └── ...
│
└── plans/                               # Active and completed plans
    └── ...
```

**Key directories:**
- `knowledge/` — Where compounding happens
- `rules/` — Self-auditing enforcement
- `skills/` — Workflow automation
- `agents/` — Review specialists

---

## Knowledge Accumulation Templates

### Pattern Template

```markdown
# Pattern: [Pattern Name]

## The Rule
[One-sentence summary of what to do or not do]

## Why It Matters
[What goes wrong if this pattern is violated?]

## How It's Implemented
[Code examples from your codebase]

## Checklist for Agents
- [ ] Check item 1
- [ ] Check item 2
- [ ] Check item 3

## Related Patterns
- [Link to related pattern if applicable]
```

### Learning Template

```markdown
# Learning: [Feature/Fix Name]

## Problem
[What was the original issue or requirement?]

## Context
[What part of the codebase was involved? What constraints existed?]

## Root Cause (if bug)
[What was the underlying issue?]

## Solution
[What approach was taken? Why?]

## Pattern Identified
[What reusable pattern emerged?]

## Review Checklist Items
[What should agents check in future work?]
- [ ] Item 1
- [ ] Item 2

## Code Reference
[Links to key files or commits]

## Related Learnings
[Links to other learning documents if applicable]
```

### Review Checklist Template

```markdown
# Review Checklist: [Domain]

## When to Use
[What types of changes trigger this review?]

## Critical Checks (Must Pass)
- [ ] Check 1: [description]
- [ ] Check 2: [description]

## Best Practice Checks (Should Pass)
- [ ] Check 1: [description]
- [ ] Check 2: [description]

## Common Pitfalls
- Pitfall 1: [what goes wrong, how to avoid]
- Pitfall 2: [what goes wrong, how to avoid]

## Related Patterns
- [Link to relevant pattern documents]
```

---

## Self-Auditing Rules

Neither the original Compound Engineering nor ACE articles describe this pattern: **rule files that audit the knowledge system itself**.

### How It Works

1. Each rule file has YAML `paths:` frontmatter that auto-triggers when matching files are touched
2. When triggered, the rule presents a standardized checklist and audit workflow
3. After auditing, the rule grades the work (A/B/C/D) and offers improvements
4. Rules reference patterns for structure and learnings for context

### The Virtuous Cycle

```
Rules audit patterns/learnings/agents
    ↓
Findings become new learnings
    ↓
Learnings improve patterns
    ↓
Patterns improve future work
    ↓
Work gets audited by rules
    ↓
(cycle continues)
```

### Example Rule Structure

```yaml
---
name: pattern-file-audit
paths:
  - ".claude/knowledge/patterns/*.md"
---

# Pattern File Audit

## Trigger
This rule activates when any pattern file is created or modified.

## Checklist
1. [ ] Has clear "The Rule" section (one sentence)
2. [ ] Explains "Why It Matters"
3. [ ] Includes code examples from this codebase
4. [ ] Has actionable checklist for agents
5. [ ] Links to related patterns

## Grading
- A: All items complete, well-written
- B: Minor omissions
- C: Missing significant sections
- D: Does not follow template
```

---

## Context Management Strategy

### Token Budget Management

- Keep main context at **40-60% utilization** (preserve reasoning clarity)
- Use subagents for expensive operations (search, read, exploration)
- Compact after each verified phase
- Reset context between major features

### Subagent Isolation Pattern

```
Main Agent:
  "I need to understand how [domain] works in this codebase"

  └─> Launch Explore Subagent:
      "Find all [domain] code, patterns, and constraints"

      └─> Returns: Distilled summary (not raw output)
          - Files involved
          - Pattern identified
          - Constraints discovered

Main Agent uses summary to plan (context stays clean)
```

### Compaction Triggers

- After each **Work → Assess** cycle completes
- When context exceeds 60% utilization
- Before starting a new feature
- After Compound phase (learnings captured, reset for next)

---

## Human Leverage Points

Prioritize human review where it has highest impact:

| Priority | Activity | Why High Impact |
|----------|----------|-----------------|
| 1 (Highest) | Review research outputs | Catch misunderstandings before they compound |
| 2 | Approve plans | Redirect before code is written |
| 3 | Verify builds/tests | Ensure correctness (but less leverage) |
| 4 (Lowest) | Review assess findings | Turn discoveries into knowledge |

**Key insight:** Time spent reviewing plans prevents exponentially more time fixing code.

---

## Success Metrics

Track these to verify the system is compounding:

### 1. Time to Implement Similar Features
- **Baseline:** First feature of type X takes N hours
- **Goal:** Similar features take 0.7N, then 0.5N, then 0.3N
- **Why:** Knowledge reuse reduces discovery time

### 2. Review Findings per Feature
- **Baseline:** Reviews find N issues
- **Goal:** Issues caught earlier (in research/planning), fewer in code review
- **Why:** Patterns and checklists prevent issues before they're created

### 3. Knowledge Base Growth
- **Track:** Learnings per feature, patterns extracted, checklist items added
- **Goal:** Linear growth with feature count
- **Why:** Each feature should contribute to the knowledge layer

### 4. Context Window Efficiency
- **Track:** Token usage per phase
- **Goal:** Stay at 40-60% utilization
- **Why:** Reasoning quality degrades above 70%

### 5. Human Review Time Distribution
- **Goal:** More time on research/plans, less on code
- **Why:** Earlier review has higher leverage

---

## Implementation Phases

### Phase 1: Foundation (First Week)
**Goal:** Set up knowledge infrastructure

- [ ] Create `.claude/knowledge/` directory structure
- [ ] Document 2-3 existing patterns from codebase
- [ ] Create 1-2 learning documents from past work
- [ ] Update project instructions to reference knowledge base

**Verification:** Agents can find and cite knowledge documents

### Phase 2: Review System (Second Week)
**Goal:** Build domain-specific review agents

- [ ] Identify 3-6 review domains relevant to your stack
- [ ] Create review agent prompts for each
- [ ] Test review agents on existing code
- [ ] Refine rubrics based on false positives

**Verification:** Agents catch real issues without flagging correct code

### Phase 3: Workflow Skills (Third Week)
**Goal:** Create skills for each phase

- [ ] Research skill (subagent exploration)
- [ ] Plan skill (planning template)
- [ ] Verify skill (multi-agent review)
- [ ] Compound skill (knowledge capture)

**Verification:** Skills produce expected outputs, workflow feels natural

### Phase 4: First Full Cycle (Fourth Week)
**Goal:** Execute complete workflow on a real feature

- [ ] Pick a small feature
- [ ] Execute: Research → Plan → Work → Assess → Compound
- [ ] Document learnings
- [ ] Measure time vs. traditional approach

**Verification:** Feature completed, learning captured, improvement identified

### Phase 5: Iteration (Ongoing)
**Goal:** Compound the system itself

- [ ] After each feature, update knowledge base
- [ ] Refine agents based on missed issues
- [ ] Add new patterns as they emerge
- [ ] Measure: Is each feature faster?

---

## Quick Reference

### The Core Question
Ask after every completed unit of work:
> "What did we learn that makes the next similar task easier?"

### The Minimum Viable Compound
If nothing else, create a learning document after each feature:
1. What was the problem?
2. What did we discover?
3. What pattern emerged?

### The Red Flags
If you see these, the system isn't compounding:
- Same issues appearing repeatedly
- Agents not referencing knowledge base
- Knowledge documents not being created
- Each feature takes as long as the last

---

## Tech-Specific References

This methodology is tech-stack agnostic. For stack-specific patterns, review agents, and constraints, see:

- `tech-references/swift-macos-reference.md` — iOS/macOS development
- `tech-references/stack-template.md` — Template for adding new stacks

---

**Sources:**
- [Compound Engineering: How Every Codes With Agents](https://every.to/chain-of-thought/compound-engineering-how-every-codes-with-agents)
- [Advanced Context Engineering for Coding Agents](https://github.com/humanlayer/advanced-context-engineering-for-coding-agents/blob/main/ace-fca.md)

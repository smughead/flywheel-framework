# Flywheel Framework

An AI-assisted development methodology where each unit of work makes the next easier to build.

> **Credits:** Inspired by [Compound Engineering](https://every.to/chain-of-thought/compound-engineering-how-every-codes-with-agents) (Every.to) and [Advanced Context Engineering](https://github.com/humanlayer/advanced-context-engineering-for-coding-agents) (HumanLayer), combined and extended with self-auditing rules and stack-agnostic design.

## What It Does

Flywheel Framework systematically captures patterns, learnings, and review findings as you work—creating a knowledge layer that grows with your project. AI agents reference this accumulated context instead of starting from zero each session.

**The flywheel effect:**
- Today's discoveries become tomorrow's shortcuts
- Review findings become guardrails that prevent repeated mistakes
- The system improves itself through self-auditing rules

## Quick Start

### Option 1: Use the Scaffold Skill (Recommended)

1. Copy `scaffold-skill/SKILL.md` to your project's `.claude/skills/` directory
2. Run `/flywheel-setup` in Claude Code
3. Follow the prompts to configure for your tech stack

### Option 2: Manual Setup

1. Create the directory structure:
```
.claude/
├── knowledge/
│   ├── patterns/
│   ├── learnings/
│   └── reviews/
├── rules/
├── agents/
└── plans/
```

2. Add knowledge base reference to your CLAUDE.md:
```markdown
## Flywheel Framework

Follow this workflow for non-trivial changes:
Plan → Work → Verify → Compound

### Resources
- **Knowledge Base:** `.claude/knowledge/`
- **Review Agents:** `.claude/agents/`
```

3. Copy relevant tech reference (e.g., `tech-references/swift-macos-reference.md`) for your stack

## Core Workflow

```
1. RESEARCH  → Subagents explore codebase
2. PLAN      → Design with project constraints (human review HERE)
3. WORK      → Execute with build verification
4. ASSESS    → Multi-perspective review (knowledge discovery)
5. COMPOUND  → Document learnings for future work
```

**Key insight:** Human review at the Plan phase prevents exponentially more time fixing code later.

## Directory Contents

```
flywheel-framework/
├── README.md                    # This file
├── Methodology.md               # Core framework documentation
├── tech-references/
│   ├── stack-template.md        # Template for adding new stacks
│   └── swift-macos-reference.md # Swift/macOS patterns & agents
└── scaffold-skill/
    └── SKILL.md                 # The /flywheel-setup skill
```

## Adding a New Tech Stack

1. Copy `tech-references/stack-template.md` to `tech-references/[your-stack]-reference.md`
2. Fill in the bracketed placeholders with your stack-specific:
   - Build/test commands
   - 3-6 review agents
   - Critical patterns
   - Common pitfalls
3. Optionally contribute back via PR

## The Self-Auditing Innovation

Neither the original Compound Engineering nor ACE articles describe this pattern: **rule files that audit the knowledge system itself**.

Rules auto-trigger when knowledge files are created/modified, presenting standardized checklists and grading the work. This creates a virtuous cycle:

```
Rules audit patterns/learnings → Findings become new learnings →
Learnings improve patterns → Patterns improve future work →
Work gets audited by rules → (cycle continues)
```

## Success Indicators

The system is working when:
- Similar features take less time than the first
- Issues get caught earlier (in planning, not code review)
- Knowledge base grows with each feature
- Agents cite project-specific patterns, not generic advice

## Learn More

- **Full methodology:** [Methodology.md](./Methodology.md)
- **Swift/macOS setup:** [tech-references/swift-macos-reference.md](./tech-references/swift-macos-reference.md)
- **Add your stack:** [tech-references/stack-template.md](./tech-references/stack-template.md)

## License

MIT

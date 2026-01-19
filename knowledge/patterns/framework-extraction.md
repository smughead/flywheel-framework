---
created: 2026-01-19
last_verified: 2026-01-19
---

# Pattern: Framework Extraction Architecture

## When to Apply
When extracting reusable methodology, workflows, or best practices from a working project into a standalone, portable framework that others can adopt.

## Why It Matters
Without proper separation, extracted frameworks remain coupled to their origin project. Users can't adopt them without also adopting project-specific baggage (tech stack, naming, constraints). Proper extraction creates truly reusable assets.

## The Pattern

Use a **three-layer architecture** to separate concerns:

```
framework-repo/
├── [Core].md              # Universal principles (stack-agnostic)
├── README.md              # Quick start, success indicators
├── tech-references/       # Stack-specific implementations
│   ├── template.md        # Blank template with [placeholders]
│   └── [stack]-ref.md     # Complete examples
└── scaffold-skill/        # Automation for project setup
    └── SKILL.md           # Interactive initialization
```

**Layer 1: Core Methodology**
- Contains principles, workflows, templates
- No code, no stack-specific patterns
- Should work for ANY tech stack

**Layer 2: Tech References**
- Stack-specific review agents, patterns, pitfalls
- Blank template shows structure with `[bracketed placeholders]`
- At least one complete example (template filled out)
- Each stack is self-contained

**Layer 3: Scaffold Skill**
- Automates first-time setup
- Detects existing structure (idempotent)
- Asks clarifying questions (tech stack)
- Creates directories, starter files, updates project instructions

## Example

From Flywheel Framework export:
- **Core**: `Methodology.md` (5-phase workflow, no Swift code)
- **Tech ref**: `swift-macos-reference.md` (6 agents, audio patterns, macOS permissions)
- **Template**: `stack-template.md` with sections like `### [domain]-reviewer`
- **Scaffold**: `/flywheel-setup` skill creates `.claude/knowledge/`, agents, rules

## Anti-Pattern

**Monolithic extraction**: Copying all files from the source project without separating universal from stack-specific. Results in framework that only works for one stack.

**Example-only documentation**: Providing filled examples without blank templates. Users must reverse-engineer what to keep vs. replace.

**Manual setup required**: No scaffold skill. Users must read docs and manually create every file, leading to inconsistent adoption.

## Checklist
- [ ] Core methodology contains zero stack-specific content
- [ ] Blank template exists with clear `[placeholder]` syntax
- [ ] At least one complete stack reference as example
- [ ] Scaffold skill automates directory/file creation
- [ ] Attribution credits sources prominently
- [ ] Novel contributions are explicitly documented

## References
- Learning: `.claude/knowledge/learnings/2026-01-19-flywheel-framework-export.md`
- Flywheel Framework repo: https://github.com/smughead/flywheel-framework

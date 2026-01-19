# TypeScript Tech Stack

Stack-specific skills, agents, and patterns for TypeScript/JavaScript development.

## Status: Scaffold Only

This tech stack is scaffolded but not yet populated. Contributions welcome!

## Planned Structure

```
typescript/
├── skills/
│   ├── build/SKILL.md        # npm/yarn/pnpm build
│   ├── test/SKILL.md         # Jest/Vitest/Mocha
│   └── review-ts/SKILL.md    # TypeScript review agents
├── agents/
│   ├── type-safety-reviewer.md
│   ├── react-patterns-checker.md
│   ├── node-best-practices.md
│   └── test-coverage-analyzer.md
└── README.md
```

## Common Commands

Until stack-specific skills are created, use these common commands:

| Task | Command |
|------|---------|
| Build | `npm run build` or `tsc` |
| Test | `npm test` or `npx jest` or `npx vitest` |
| Lint | `npm run lint` or `npx eslint .` |
| Format | `npm run format` or `npx prettier --write .` |

## Contributing

To add TypeScript skills:

1. Create skill files following `knowledge/patterns/skill-file-structure.md`
2. Create agent files following `knowledge/patterns/agent-file-structure.md`
3. Test in a TypeScript project
4. Submit a PR

### Suggested Review Agents

- **type-safety-reviewer** - Strict types, `any` usage, generics
- **react-patterns-checker** - Hooks rules, component patterns, performance
- **node-best-practices** - Error handling, async patterns, security
- **test-coverage-analyzer** - Test gaps, mocking patterns, assertions

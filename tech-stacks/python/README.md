# Python Tech Stack

Stack-specific skills, agents, and patterns for Python development.

## Status: Scaffold Only

This tech stack is scaffolded but not yet populated. Contributions welcome!

## Planned Structure

```
python/
├── skills/
│   ├── build/SKILL.md        # pip/poetry/setuptools
│   ├── test/SKILL.md         # pytest/unittest
│   └── review-python/SKILL.md # Python review agents
├── agents/
│   ├── type-hints-reviewer.md
│   ├── async-patterns-checker.md
│   ├── security-reviewer.md
│   └── test-coverage-analyzer.md
└── README.md
```

## Common Commands

Until stack-specific skills are created, use these common commands:

| Task | Command |
|------|---------|
| Build | `pip install -e .` or `poetry build` |
| Test | `pytest` or `python -m pytest` |
| Lint | `ruff check .` or `flake8` |
| Format | `ruff format .` or `black .` |
| Type Check | `mypy .` or `pyright` |

## Contributing

To add Python skills:

1. Create skill files following `knowledge/patterns/skill-file-structure.md`
2. Create agent files following `knowledge/patterns/agent-file-structure.md`
3. Test in a Python project
4. Submit a PR

### Suggested Review Agents

- **type-hints-reviewer** - Type annotations, generics, Protocol usage
- **async-patterns-checker** - asyncio patterns, concurrency, race conditions
- **security-reviewer** - Input validation, SQL injection, secrets handling
- **test-coverage-analyzer** - pytest patterns, fixtures, mocking, coverage gaps
- **packaging-reviewer** - pyproject.toml, dependencies, versioning

---
description: Develop a feature or fix with best practices enforcement
---

Implement the following task: $ARGUMENTS

Follow this workflow:

1. **Identify scope** — determine which files and modules are affected. Read existing code before modifying anything.

2. **Detect project conventions** — auto-detect from project files:
   - Test runner: jest.config.*, vitest.config.*, bunfig.toml, pytest.ini, go.mod, Cargo.toml
   - Test location: __tests__/, src/tests/, tests/, co-located *.test.* files
   - Linter: eslint.config.*, .eslintrc.*, pyproject.toml [tool.ruff], .golangci.yml
   - Type checker: tsconfig.json, pyright, mypy.ini
   - Package manager: detect from lock file (pnpm-lock.yaml, package-lock.json, yarn.lock, Cargo.lock, go.sum)
   - Validation commands: check package.json scripts, Makefile, or project docs

3. **TDD if logic code** — if the task involves business logic (services, helpers, validators, algorithms, handlers), follow Red-Green-Refactor:
   - RED: write failing tests first, matching existing test conventions
   - GREEN: write minimum code to pass
   - REFACTOR: clean up, tests still pass

4. **Implement** — follow existing patterns. TypeScript strict (no `any`). Max 400 lines per file. No over-engineering.

5. **Validate** — run the project's detected validation commands:
   - Type checking (if available)
   - Linting (if available)
   - Full test suite (if available)

6. **Commit** — use conventional commits. Stage files explicitly (never `git add .`). Verify no secrets are staged.

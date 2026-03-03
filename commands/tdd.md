---
description: Implement a feature or fix using strict Test-Driven Development
---

Implement the following using TDD: $ARGUMENTS

## Workflow

1. **Identify scope** — determine which files and modules are affected. Read existing code first.

2. **Detect test infrastructure** — find existing test files to match conventions:
   - Test runner: look for jest.config.*, vitest.config.*, bunfig.toml, pytest.ini, go.mod, Cargo.toml
   - Test location: check for __tests__/, src/tests/, tests/, *.test.ts next to source, etc.
   - Test command: check package.json scripts, Makefile, or project docs
   - Match the EXACT naming convention and directory structure already in use.

3. **RED — Write failing tests first**
   - Create or update the test file following the project's existing conventions.
   - Write tests for: nominal case, edge cases, error cases.
   - Run the tests. They MUST fail. If they pass, the tests are not testing new behavior — rewrite them.

4. **GREEN — Write minimum implementation**
   - Write the minimum code to make ALL tests pass. Nothing more.
   - No extra features, no premature optimization, no "while I'm here" changes.
   - Run the tests. They MUST all pass.

5. **REFACTOR — Clean up (optional)**
   - Only if code quality needs improvement. Tests must still pass.
   - Run the full test suite one final time.

6. **Validate** — run the project's standard validation (typecheck, lint, full test suite).

## Hard rules

- NEVER write implementation before tests for logic-bearing code.
- NEVER skip the RED phase — if tests don't fail first, the TDD cycle is broken.
- NEVER write mocks that exceed 10 lines — extract pure functions instead.
- NEVER test config, types, constants, or SDK wrappers with no logic.
- ALWAYS commit tests and implementation together.
- ALWAYS match existing test conventions — do not invent a new structure.

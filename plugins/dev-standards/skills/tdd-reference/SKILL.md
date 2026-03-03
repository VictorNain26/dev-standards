---
name: tdd-reference
description: TDD workflow reference (Red-Green-Refactor). Use when writing or modifying services, helpers, validators, handlers, or test files.
user-invocable: false
---

# Test-Driven Development (TDD)

Mandatory workflow for all business logic code.

## Red-Green-Refactor

1. **RED** — Write tests first. Describe expected behavior (nominal + edge cases). Run tests — they must FAIL.
2. **GREEN** — Write minimum code to pass tests. No extras.
3. **REFACTOR** — Clean up if needed. Tests must still pass.

## What requires tests

| Type | Tests? | Reason |
|------|--------|--------|
| Service with business logic | **YES** | Core behavior to protect |
| Helper / pure utility | **YES** | Easy to test, high ROI |
| Validation schemas | **YES** | Contract enforcement |
| Algorithm / computation | **YES** | Complex logic, regression risk |
| Webhook / event handler | **YES** | External contract |
| Config / constants | NO | Static data, no logic |
| Types / interfaces | NO | Compile-time only |
| SDK wrappers (no logic) | NO | Testing the SDK, not your code |
| UI components | NO | UI testing is a separate concern |
| Index re-exports | NO | No logic |

## No test theater

- If a mock exceeds 10 lines, extract a pure function instead.
- If the test only verifies that a wrapper calls the SDK, delete it.
- If you need to mock the database, mock the repository layer, not the DB driver.
- Prefer testing pure exported functions over stateful classes.
- A test that passes when the implementation is deleted is a useless test.

## Pre-commit gate

Run the full test suite for the affected app/package before committing. All tests must pass.

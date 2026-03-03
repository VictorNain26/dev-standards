---
description: Review current branch changes for production readiness
---

Review the current branch for production readiness.

## Steps

1. **Get the diff** — auto-detect the main branch:
   - Check `git symbolic-ref refs/remotes/origin/HEAD` or try `main`, then `master`
   - Run `git diff <main-branch>...HEAD` and `git log <main-branch>..HEAD --oneline`

2. **Check each category:**

   - **Type safety** — no `any` (TypeScript), no untyped parameters, no `@ts-ignore`, no unhandled `null`/`undefined`
   - **Security** — no secrets committed, no `.env` files, no SQL injection vectors, no XSS, no command injection
   - **Error handling** — external calls have error handling, no swallowed exceptions, meaningful error messages
   - **Code quality** — files under 400 lines, no dead code, no premature abstractions, follows existing patterns
   - **TDD compliance** — every modified service/helper/validator has a corresponding test file modified. Tests cover nominal + edge cases. No test theater (massive mocks, trivial assertions).

3. **Report findings** with this format:
   - `[BLOCKING]` file:line — description (must fix before merge)
   - `[SUGGESTION]` file:line — description (nice to have)

4. **If blocking issues found:** propose auto-fix for each one. Ask for confirmation before applying.

5. **Summary:** overall assessment — ready to merge or not, with reasoning.

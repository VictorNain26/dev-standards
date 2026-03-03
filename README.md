# dev-standards

Claude Code plugin — enforcement hooks for git safety, secret protection, and TDD compliance. Zero config, language-agnostic.

## Install

```bash
# 1. Add the marketplace
/plugin marketplace add VictorNain26/dev-standards

# 2. Install the plugin
/plugin install dev-standards@victornain26-plugins
```

## What's included

### Hooks (always active)

| Hook | Event | What it blocks |
|------|-------|---------------|
| Explicit staging | PreToolUse (Bash) | `git add .` / `-A` / `--all` — forces explicit file staging |
| No skip hooks | PreToolUse (Bash) | `--no-verify` — forces fixing pre-commit errors |
| No skip permissions | PreToolUse (Bash) | `--dangerously-skip-permissions` |
| Secret protection | PreToolUse (Edit\|Write) | Modifying `.env.*` files (except `.env.example`) |
| TDD gate | Stop (prompt) | Finishing without tests for implementation files |
| TDD gate (subagent) | SubagentStop (prompt) | Same check applied to subagents |
| Compaction re-inject | SessionStart (compact) | Re-injects critical rules after context compaction |

### Skills

| Skill | Invocable | Description |
|-------|-----------|-------------|
| `/tdd` | Yes | Strict TDD workflow: Red-Green-Refactor with auto-detection |
| `tdd-reference` | Auto | TDD rules loaded when working on business logic |
| `security-reference` | Auto | Security rules loaded when working with git or secrets |

> `/dev` and `/review` workflows are intentionally not included — use `feature-dev` and `code-review` plugins for those (more mature, multi-agent).

## How it works

```
Agent writes code
        |
        v
   Hooks fire at every step:
   - PreToolUse: blocks unsafe git commands, secret edits
   - Stop + SubagentStop: checks TDD compliance before finishing
        |
    +---+---+
    |       |
  No impl  Impl files touched
  files     |
    |     +-+-------+
    v     |         |
 APPROVE  Tests     No tests
          exist     found
          |         |
          v         v
        APPROVE   BLOCK --> agent writes tests
```

## Language support

The TDD gate supports any language with standard test file patterns:

| Language | Test patterns |
|----------|--------------|
| TypeScript/JavaScript | `*.test.ts`, `*.spec.ts`, `*.test.js`, `*.spec.js` |
| Go | `*_test.go` |
| Python | `test_*.py`, `*_test.py` |
| Java | `*Test.java` |
| Rust | `*_test.rs`, `tests/*.rs` |
| Ruby | `*_test.rb`, `*_spec.rb` |

## Compatibility

- Any language, any test runner, any project structure
- Claude Code v1.0.33+

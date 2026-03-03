# dev-standards

Claude Code plugin that enforces universal development standards. Git safety, secret protection, TDD enforcement, dev workflow, and code review — zero config, language-agnostic.

> Successor to [tdd-enforcer](https://github.com/VictorNain26/tdd-enforcer). Covers everything tdd-enforcer did, plus git safety, secret protection, and structured workflows.

## Install

```bash
# 1. Add the marketplace
/plugin marketplace add VictorNain26/dev-standards

# 2. Install the plugin
/plugin install dev-standards@victornain26-plugins
```

For local development:
```bash
claude --plugin-dir /path/to/dev-standards
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

### Skills (user-invocable)

| Skill | Description |
|-------|-------------|
| `/dev` | Full dev workflow: scope, detect conventions, TDD if logic, implement, validate, commit |
| `/tdd` | Strict TDD workflow: Red-Green-Refactor with auto-detection |
| `/review` | Code review checklist: type safety, security, error handling, TDD compliance |

### Skills (auto-loaded, not user-invocable)

| Skill | Loaded when |
|-------|-------------|
| `tdd-reference` | Working on business logic code |
| `security-reference` | Working with git, env files, or sensitive data |

## How it works

```
User asks to implement something
        |
        v
/dev or /tdd skill (optional — sets workflow)
        |
        v
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

## Project-specific overrides

Plugin skills (`/dev`, `/tdd`, `/review`) can be overridden by placing same-named files in your project's `.claude/commands/` or `.claude/skills/` directory. Project definitions take precedence over plugin ones.

Plugin hooks always apply — they cannot be overridden per-project.

### Recommended project setup

After installing this plugin, your project only needs:

```
.claude/
  commands/
    dev.md          # (optional) project-specific dev workflow overlay
    review.md       # (optional) project-specific review overlay
  rules/
    tdd.md          # project-specific test conventions (runners, paths)
    ...             # other project-specific rules
```

You can safely remove from `.claude/settings.json` any hooks that this plugin now handles (git safety, secret protection).

## Compatibility

- Any language: TypeScript, JavaScript, Python, Go, Rust, Java, Ruby
- Any test runner: Jest, Vitest, Bun, pytest, Go test, Cargo test, JUnit, RSpec
- Any project structure: monorepo, single app, library
- Claude Code v1.0.33+

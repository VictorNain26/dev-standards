---
name: security-reference
description: Security rules for git operations, secret handling, and secure coding. Auto-loaded when working with git, environment files, or sensitive data.
user-invocable: false
---

# Security Standards

## Never commit secrets

Forbidden in git:
- `.env.*` files (except `.env.example`)
- API keys, tokens, credentials
- OAuth client secrets
- Private keys, certificates
- `settings.local.json` or any local override with secrets

## Before every commit

1. Run `git diff --cached --name-only` to review staged files
2. Check for any `.env` files, key files, or credential files
3. If secrets detected, unstage immediately: `git reset HEAD <file>`
4. Use `.gitignore` to prevent accidental staging

## If a secret is exposed

1. **Revoke immediately** — rotate the compromised keys/tokens
2. **Clean git history** — use BFG Repo-Cleaner or orphan branch strategy
3. **Force push** the cleaned branches
4. **Regenerate** all affected credentials
5. **Audit** access logs for unauthorized usage

## Secure coding

- Never interpolate user input directly into SQL queries — use parameterized queries
- Never render user input as raw HTML — sanitize or escape
- Never pass user input to shell commands — use safe APIs
- Never log secrets, tokens, or passwords
- Never hardcode credentials — use environment variables
- Validate and sanitize all external input at system boundaries

# estanbulgaming Organization GitHub Configuration

This repository contains organization-wide GitHub configurations and reusable workflows.

## Reusable Workflows

### AI Security Review

Automated security analysis using Gemini AI. Reviews code changes in PRs and posts findings as comments.

**Usage:**

```yaml
name: AI Security Review

on:
  pull_request:
    types: [opened, synchronize]

jobs:
  security:
    uses: estanbulgaming/.github/.github/workflows/security-review.yml@main
    secrets:
      GEMINI_API_KEY: ${{ secrets.GEMINI_API_KEY_CI }}
```

**With options:**

```yaml
jobs:
  security:
    uses: estanbulgaming/.github/.github/workflows/security-review.yml@main
    with:
      fail_on_critical: true   # Fail workflow on CRITICAL issues (default: true)
      fail_on_high: false      # Fail workflow on HIGH issues (default: false)
    secrets:
      GEMINI_API_KEY: ${{ secrets.GEMINI_API_KEY_CI }}
```

**Required secrets:**
- `GEMINI_API_KEY_CI` - Google Gemini API key

**Optional file:**
- `.github/SECURITY_RULES.md` - Custom security rules for your project

## Custom Security Rules

Each repository can define its own security rules by creating `.github/SECURITY_RULES.md`. The AI will consider these rules during analysis.

**Example `.github/SECURITY_RULES.md`:**

```markdown
# Security Rules

## Project-Specific Rules
- Never expose API keys in logs or responses
- All user passwords must be hashed with bcrypt
- Admin endpoints must require authentication header

## Allowed Patterns (Ignore)
- CORS "*" is intentional for development environment
- SQLite is acceptable for this project (not production-scale)
```

## Severity Levels

| Level | Emoji | Action |
|-------|-------|--------|
| CRITICAL | 🔴 | Blocks merge (if `fail_on_critical: true`) |
| HIGH | 🟠 | Warning or blocks (if `fail_on_high: true`) |
| MEDIUM | 🟡 | Warning |
| LOW | 🔵 | Info |

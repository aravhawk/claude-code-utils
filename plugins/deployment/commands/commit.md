---
description: Commits with perfect messages
---

# Git Commit Command

Create a well-crafted, atomic git commit following best practices.

## Prerequisites

- Requires `git_write` permission for all git operations
- Working directory must be inside a git repository

## Execution Flow

### Step 1: Assess Repository State

```bash
git status --porcelain
```

**Decision tree:**
- If output is empty → STOP. Report "No changes to commit."
- If untracked files exist → Proceed to Step 2
- If staged changes exist → Skip to Step 3
- If only unstaged changes → Proceed to Step 2

### Step 2: Stage Changes Intelligently

**Before staging, verify these files are NOT being committed:**
- Secrets, API keys, credentials, `.env` files (unless `.env.example`)
- Build artifacts (`dist/`, `build/`, `node_modules/`, `__pycache__/`)
- OS files (`.DS_Store`, `Thumbs.db`)
- IDE configs (`.idea/`, `.vscode/` unless project-specific)
- Large binary files or data dumps

**If .gitignore is missing critical exclusions:**
1. Update `.gitignore` first
2. Then proceed with staging

**Stage command:**
```bash
git add -A
```

Or for selective staging when changes should be split into atomic commits:
```bash
git add <specific-files>
```

### Step 3: Analyze the Diff

Run both commands to understand the full scope:

```bash
git diff --cached --stat
git diff --cached
```

**Analyze for:**
- Primary intent (feature, fix, refactor, docs, chore, etc.)
- Scope/area affected (auth, api, ui, db, config, etc.)
- Breaking changes (API changes, schema migrations, removed features)
- Whether this should be ONE commit or MULTIPLE atomic commits

**Atomic commit principle:** Each commit should represent ONE logical change. If the diff contains unrelated changes, unstage and create separate commits:
```bash
git reset HEAD <file>  # Unstage specific files for a separate commit
```

### Step 4: Craft the Commit Message

**Format (Conventional Commits):**
```
<type>(<scope>): <summary>

[optional body]

[optional footer]
```

**Types:**
| Type | Use When |
|------|----------|
| `feat` | New feature or capability |
| `fix` | Bug fix |
| `docs` | Documentation only |
| `style` | Formatting, whitespace (no logic change) |
| `refactor` | Code restructuring (no behavior change) |
| `perf` | Performance improvement |
| `test` | Adding/updating tests |
| `build` | Build system, dependencies |
| `ci` | CI/CD configuration |
| `chore` | Maintenance, tooling |

**Rules:**
- Summary: imperative mood ("add" not "added"), lowercase, no period, max 50 chars
- Body: wrap at 72 chars, explain WHAT changed and WHY (not how)
- Footer: `BREAKING CHANGE:` prefix for breaking changes, `Fixes #123` for issue refs

**Good examples:**
```
feat(auth): add oauth2 login with google

Implements Google OAuth2 flow with PKCE for enhanced security.
Stores refresh tokens encrypted in the database.

BREAKING CHANGE: removes legacy session-based auth
```

```
fix(api): handle null response from payment gateway

The Stripe API occasionally returns null for declined cards
instead of an error object. Added defensive check.

Fixes #234
```

**Bad examples (avoid these patterns):**
- `update code` - too vague
- `fix bug` - which bug?
- `wip` - never commit work-in-progress
- `misc changes` - meaningless
- `Fixed the thing that was broken` - not imperative, not specific

### Step 5: Execute the Commit

**Single-line message:**
```bash
git commit -m "<type>(<scope>): <summary>"
```

**With body:**
```bash
git commit -m "<type>(<scope>): <summary>" -m "<body paragraph>" -m "<footer>"
```

### Step 6: Confirm Success

```bash
git log -1 --oneline
```

Report the commit hash and summary to confirm completion.

## Edge Cases

| Situation | Action |
|-----------|--------|
| Merge conflict markers in files | STOP. Report conflict. Do not commit. |
| Diff is extremely large (>1000 lines) | Consider splitting into atomic commits |
| Only whitespace/formatting changes | Use `style` type, consider if commit is necessary |
| Contains TODO/FIXME comments | Acceptable, but note in commit body if significant |
| Reverts a previous commit | Use `git revert <hash>` instead of manual revert |

## Output Format

On success, report:
```
Committed: <hash> <type>(<scope>): <summary>
Files changed: <count>
Insertions: <count>, Deletions: <count>
```

On failure/abort, explain why and what action is needed.

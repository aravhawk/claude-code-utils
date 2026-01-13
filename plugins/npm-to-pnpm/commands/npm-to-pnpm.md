---
description: Migrates codebase from npm to pnpm
---

Migrate this project from npm to pnpm. Follow these steps precisely:

## 1. Identify npm artifacts
- Locate `package-lock.json` files (will be replaced by `pnpm-lock.yaml`)
- Check for `npm` scripts in `package.json` that need updating
- Look for `.npmrc` files (may need pnpm equivalents)
- Check CI/CD configs (`.github/workflows/`, `.gitlab-ci.yml`, etc.) for npm commands

## 2. Remove npm lock file
- Delete `package-lock.json` (and any nested ones in workspaces/monorepos)
- Remove `node_modules/` directories

## 3. Update package.json
- Add `"packageManager": "pnpm@latest"` field (use the actual latest stable version number)
- Replace any `npm` references in scripts with `pnpm` equivalents:
  - `npm run` -> `pnpm`
  - `npm install` -> `pnpm install`
  - `npm ci` -> `pnpm install --frozen-lockfile`
  - `npm exec` -> `pnpm exec`
  - `npx` -> `pnpm dlx` (or `pnpm exec` if the package is a dependency)

## 4. Update CI/CD and tooling configs
- Update GitHub Actions, GitLab CI, or other CI configs to use pnpm
- Add pnpm installation step if missing (e.g., `corepack enable` or `pnpm/action-setup@v4`)
- Update Dockerfile if present
- Update any shell scripts that call npm

## 5. Handle monorepo/workspace scenarios
- If using npm workspaces, ensure `pnpm-workspace.yaml` exists with the workspace configuration
- Check that workspace protocol (`workspace:*`) is used appropriately

## 6. Generate new lock file
- Run `pnpm install` to create `pnpm-lock.yaml`

## 7. Update .gitignore
- Ensure `.pnpm-store/` is ignored if local store is used

## 8. Verify migration
- Confirm the project builds/runs correctly with pnpm
- Check that all scripts work as expected

Think carefully about edge cases. Preserve all existing functionality.

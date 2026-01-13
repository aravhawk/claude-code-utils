---
description: Cleans and standardizes code style
---

# LINT SWEEP

Perform a comprehensive linting pass across the codebase. Fix all style issues, dangerous patterns, and inconsistencies.

---

## PHASE 1: DETECT LINTER CONFIGURATION

1. **Identify existing linter setup** - Check for:
   - ESLint: `.eslintrc.*`, `eslint.config.*`, `package.json` eslintConfig
   - Prettier: `.prettierrc.*`, `prettier.config.*`
   - Biome: `biome.json`
   - Python: `pyproject.toml` (ruff, black, flake8), `.pylintrc`, `setup.cfg`
   - Other: `.editorconfig`, language-specific linters

2. **Check for lint scripts** - Look in `package.json` or equivalent:
   ```
   scripts: { "lint": "...", "lint:fix": "...", "format": "..." }
   ```

3. **If no linter exists** - Recommend setup based on tech stack, but don't install without confirmation.

---

## PHASE 2: RUN LINTING TOOLS

Execute in order:

1. **Run existing lint command** - Use project's configured script first:
   ```bash
   pnpm lint        # or npm run lint
   pnpm lint:fix    # if available
   ```

2. **Run formatter** - If Prettier/Biome is configured:
   ```bash
   pnpm format      # or direct: pnpx prettier --write .
   ```

3. **Capture all output** - Note every warning and error for review.

---

## PHASE 3: CATEGORIZE ISSUES

Group findings by severity:

### Critical (fix immediately)
- Security vulnerabilities (eval, dangerouslySetInnerHTML, SQL injection patterns)
- Undefined variable usage
- Type errors (in TypeScript projects)
- Syntax errors preventing compilation

### High (fix now)
- Unused variables and imports
- Console.log/debug statements in production code
- Missing error handling (empty catch blocks)
- Deprecated API usage
- Any/unknown type abuse (TypeScript)

### Medium (fix during sweep)
- Inconsistent naming conventions
- Missing return types (TypeScript)
- Long functions exceeding complexity thresholds
- Duplicate code blocks
- Magic numbers without constants

### Low (fix if time permits)
- Formatting inconsistencies (spacing, quotes, semicolons)
- Comment style inconsistencies
- Import ordering
- Line length violations

---

## PHASE 4: APPLY FIXES

Work through issues systematically:

1. **Auto-fix what's safe** - Run `--fix` flags:
   ```bash
   pnpx eslint . --fix
   pnpx prettier --write .
   ruff check --fix .  # Python
   ```

2. **Manual fixes for complex issues**:
   - Remove unused code (verify it's truly unused first)
   - Replace deprecated APIs with modern equivalents
   - Add missing type annotations
   - Extract magic numbers to named constants

3. **Verify after each batch** - Re-run linter to confirm fixes didn't introduce new issues.

---

## PHASE 5: CONSISTENCY PASS

Beyond linter rules, check for:

### Naming
- Variables: camelCase (JS/TS) or snake_case (Python)
- Constants: SCREAMING_SNAKE_CASE
- Components: PascalCase
- Files: Match content (component files = PascalCase, utilities = camelCase/kebab-case)

### Patterns
- Consistent async/await vs .then() usage
- Consistent error handling approach
- Consistent import style (named vs default, path aliases)
- Consistent file structure

### Dangerous Patterns
- `any` type overuse
- Nested ternaries
- Deeply nested callbacks
- Mutating function parameters
- Implicit type coercion

---

## PHASE 6: VERIFICATION

Before completing:

1. **Run full lint check** - Ensure zero errors:
   ```bash
   pnpm lint
   ```

2. **Run tests** - Confirm no regressions:
   ```bash
   pnpm test
   ```

3. **Build check** - Verify project still compiles:
   ```bash
   pnpm build
   ```

4. **Spot check** - Review a few changed files to ensure quality.

---

## CONSTRAINTS

- **Don't change logic** - Only style/quality fixes, no behavior changes
- **Preserve intentional patterns** - Some "violations" may be intentional (check comments)
- **Respect existing config** - Work within project's linter rules, don't override
- **Group related fixes** - Keep changes atomic and reviewable

---

## COMPLETION CRITERIA

Work is done when:
- [ ] Zero linter errors
- [ ] Zero linter warnings (or justified exceptions)
- [ ] All unused imports/variables removed
- [ ] No console.log/debug statements in production code
- [ ] Consistent formatting throughout
- [ ] Tests still pass
- [ ] Build succeeds

**Report summary of changes made when complete.**

---
description: Deeply optimize and clean up the codebase
---

# CODEBASE OPTIMIZATION & CLEANUP

Perform an exhaustive audit of the entire codebase. Goal: eliminate all inefficiencies while preserving 100% of user-facing functionality.

---

## PHASE 1: RECONNAISSANCE

Before making any changes:

1. **Map the codebase** - Understand the architecture, entry points, and dependencies
2. **Identify the tech stack** - Note frameworks, libraries, and build tools in use
3. **Check existing docs** - Read README, package.json, config files (but verify claims)
4. **Run the app** - Ensure you understand current behavior before modifying

---

## PHASE 2: STATIC ANALYSIS

Scan for issues WITHOUT running code:

### Dead Code
- Unused exports, functions, components, variables
- Unreachable code paths and commented-out blocks
- Orphan files not imported anywhere
- Unused imports at file tops

### Dependency Bloat
- Packages in package.json/requirements.txt not actually used
- Heavy libraries where lighter alternatives exist (e.g., lodash -> native JS)
- Duplicate dependencies (different versions of same lib)
- Dev dependencies incorrectly in production

### Redundancy
- Duplicate logic across files (DRY violations)
- Over-abstracted code (unnecessary wrappers, extra layers)
- Repeated constants/magic numbers without centralization

---

## PHASE 3: RUNTIME ANALYSIS

Identify performance issues:

### Frontend
- Unnecessary re-renders (missing memoization, bad dependency arrays)
- Large bundle sizes (missing code-splitting, tree-shaking issues)
- Unoptimized assets (images, fonts, SVGs)
- Layout thrashing, forced reflows
- Memory leaks (event listeners, subscriptions not cleaned up)

### Backend
- N+1 query problems
- Missing database indexes
- Blocking I/O operations
- Inefficient algorithms (O(n^2) when O(n) is possible)
- Uncached repeated computations

### General
- Inefficient data structures
- Excessive memory allocation
- Synchronous operations that should be async

---

## PHASE 4: MODERNIZATION

Replace outdated patterns:

- Deprecated APIs with current equivalents
- Legacy syntax (var -> const/let, callbacks -> async/await)
- Outdated library versions with security/performance fixes
- Old framework patterns with modern best practices

**REQUIREMENT:** Use web search and context7 MCP to verify current best practices. Don't assume - verify.

---

## PHASE 5: EXECUTION

For each optimization:

1. **Document** - Note what you're changing and why
2. **Change** - Make the minimal change needed
3. **Verify** - Ensure functionality is preserved
4. **Commit mentally** - Group related changes logically

### Priority Order
1. Security vulnerabilities (critical)
2. Memory leaks and crashes (high)
3. Performance bottlenecks (high)
4. Dead code removal (medium)
5. Dependency cleanup (medium)
6. Code modernization (low)

---

## CONSTRAINTS

- **Zero regressions** - All features and UX must remain identical
- **Verify everything** - Don't trust outdated docs or comments
- **Minimal changes** - Don't refactor working code unnecessarily
- **Document decisions** - Explain non-obvious optimizations

---

## COMPLETION CRITERIA

Work is done when:
- [ ] All dead code removed
- [ ] No unused dependencies remain
- [ ] No obvious performance issues
- [ ] No deprecated APIs in use
- [ ] Codebase is as lean as possible without losing functionality

**WORK UNTIL COMPLETE. NO SHORTCUTS.**

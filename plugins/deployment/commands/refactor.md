---
description: Improves code structure and readability
---

# REFACTOR

Analyze the codebase for architectural inefficiencies, overly complex functions, and unclear naming. Implement refactors that improve modularity, maintainability, and clarity while preserving functionality.

---

## PHASE 1: SCOPE IDENTIFICATION

1. **Determine refactor scope** - Ask or infer:
   - Specific file/module? Or codebase-wide?
   - Performance-focused? Readability-focused? Both?
   - Any areas explicitly off-limits?

2. **Establish baseline** - Before any changes:
   ```bash
   pnpm test        # or npm test / pytest
   pnpm build       # ensure project compiles
   ```
   If tests fail or build breaks BEFORE refactoring, STOP and report.

3. **Identify entry points** - Find high-traffic code paths:
   - Main application entry
   - Most-imported modules
   - API route handlers
   - Components with most dependencies

---

## PHASE 2: CODE SMELL DETECTION

Scan for these patterns systematically:

### Complexity Issues
| Smell | Indicator | Action |
|-------|-----------|--------|
| Long functions | >40 lines or >10 cyclomatic complexity | Extract smaller functions |
| Deep nesting | >3 levels of indentation | Flatten with early returns, extract helpers |
| Long parameter lists | >4 parameters | Use options object or builder pattern |
| God classes/modules | File >500 lines or >10 public methods | Split by responsibility |
| Feature envy | Function uses another class's data excessively | Move function to that class |

### Naming Issues
| Smell | Indicator | Action |
|-------|-----------|--------|
| Unclear names | Single letters, abbreviations, generic names (`data`, `info`, `temp`) | Rename to describe purpose |
| Misleading names | Name doesn't match behavior | Rename to reflect actual behavior |
| Inconsistent conventions | Mixed camelCase/snake_case within same file | Standardize per language norms |
| Boolean naming | `flag`, `status` instead of `isActive`, `hasPermission` | Rename with is/has/can/should prefix |

### Duplication Issues
| Smell | Indicator | Action |
|-------|-----------|--------|
| Copy-paste code | Identical or near-identical blocks | Extract shared function/component |
| Similar conditionals | Same if/switch in multiple places | Extract to policy function or strategy pattern |
| Repeated literals | Same string/number used in multiple files | Extract to constants file |

### Structural Issues
| Smell | Indicator | Action |
|-------|-----------|--------|
| Circular dependencies | A imports B imports A | Restructure module boundaries |
| Inappropriate intimacy | Module knows too much about another's internals | Define clear interfaces |
| Shotgun surgery | One change requires edits in many files | Consolidate related logic |
| Dead code | Unreachable functions, unused exports | Remove (after verification) |

---

## PHASE 3: PRIORITIZE CHANGES

Rank by impact vs. risk:

### High Impact, Low Risk (do first)
- Renaming unclear variables/functions
- Extracting constants from magic values
- Removing confirmed dead code
- Simplifying boolean expressions
- Extracting pure helper functions

### High Impact, High Risk (do carefully)
- Restructuring module boundaries
- Changing public API signatures
- Modifying shared utilities
- Database query refactors
- State management changes

### Low Impact, Low Risk (do if time permits)
- Reordering function parameters
- Adjusting file organization
- Improving comments
- Formatting consistency

### Low Impact, High Risk (skip unless requested)
- Changing established patterns without clear benefit
- Refactoring stable legacy code that's rarely touched
- Premature abstractions for hypothetical future needs

---

## PHASE 4: EXECUTE REFACTORS

Apply changes using these strategies:

### Extract Function
```
# Before
def process_order(order):
    # 50 lines of validation
    # 30 lines of calculation
    # 20 lines of notification

# After
def process_order(order):
    validate_order(order)
    total = calculate_order_total(order)
    notify_order_complete(order, total)
```

### Flatten Nesting
```
# Before
def check_access(user):
    if user:
        if user.is_active:
            if user.has_permission:
                return True
    return False

# After
def check_access(user):
    if not user:
        return False
    if not user.is_active:
        return False
    if not user.has_permission:
        return False
    return True
```

### Replace Conditionals with Polymorphism
```
# Before
def get_price(item):
    if item.type == 'book':
        return item.base_price * 0.9
    elif item.type == 'electronics':
        return item.base_price * 1.2
    else:
        return item.base_price

# After (when pattern repeats across codebase)
class PricingStrategy:
    def get_price(self, item): ...

class BookPricing(PricingStrategy):
    def get_price(self, item):
        return item.base_price * 0.9
```

### Consolidate Parameters
```
# Before
def create_user(name, email, age, city, country, postal_code):
    ...

# After
def create_user(user_data: UserInput):
    ...
```

---

## PHASE 5: INCREMENTAL VERIFICATION

After EACH significant refactor:

1. **Run tests** - Catch regressions immediately:
   ```bash
   pnpm test
   ```

2. **Type check** - Ensure type safety maintained:
   ```bash
   pnpm typecheck   # or tsc --noEmit / mypy .
   ```

3. **Lint check** - Confirm no new issues:
   ```bash
   pnpm lint
   ```

4. **Smoke test** - Quick manual verification of affected feature if applicable.

If any check fails, FIX IMMEDIATELY before continuing.

---

## PHASE 6: DOCUMENTATION UPDATES

Refactoring may require doc updates:

1. **Update inline comments** - Remove outdated explanations, add clarity where needed
2. **Update JSDoc/docstrings** - Reflect new function signatures and behavior
3. **Update README** - If module structure or usage changed
4. **Update CLAUDE.md/AGENTS.md** - If architectural patterns changed

---

## PHASE 7: FINAL VERIFICATION

Before completing:

1. **Full test suite**:
   ```bash
   pnpm test
   ```

2. **Full build**:
   ```bash
   pnpm build
   ```

3. **Lint with zero tolerance**:
   ```bash
   pnpm lint
   ```

4. **Diff review** - Scan changed files to ensure:
   - No accidental behavior changes
   - No debug code left behind
   - No unintended file modifications

---

## CONSTRAINTS

- **Preserve behavior** - Refactoring NEVER changes what code does, only how it's structured
- **Incremental changes** - Small, verifiable steps over big bang rewrites
- **Respect existing patterns** - Work within established architecture unless explicitly asked to change it
- **No premature optimization** - Don't add abstraction for hypothetical futures
- **Keep it reviewable** - Changes should be easy to understand in a code review

---

## COMPLETION CRITERIA

Work is done when:
- [ ] All identified code smells addressed (or justified exceptions documented)
- [ ] Zero test regressions
- [ ] Build succeeds
- [ ] Lint passes
- [ ] No increase in cyclomatic complexity
- [ ] Improved or maintained code readability
- [ ] Documentation updated where relevant

**Report summary of refactors performed when complete:**
- Files modified
- Key changes made
- Metrics improved (e.g., reduced function length, eliminated duplication)
- Any remaining technical debt noted for future work

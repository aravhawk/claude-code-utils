---
description: Identifies and fixes edge cases
---

# TACKLE EDGE CASES

Perform a systematic review of the codebase to identify, categorize, and fix edge cases that could cause failures, unexpected behavior, or poor user experience.

---

## PHASE 1: SCOPE ANALYSIS

1. **Determine focus area** - Ask or infer:
   - Entire codebase or specific module/feature?
   - Recent changes only or comprehensive review?
   - Any known problematic areas?

2. **Identify critical paths** - Prioritize edge case hunting in:
   - User authentication/authorization flows
   - Payment/transaction processing
   - Data persistence operations
   - External API integrations
   - File upload/download handlers
   - Form submissions and validation

3. **Map data flow** - Trace how data moves through the system:
   - Entry points (APIs, forms, file imports)
   - Transformations (parsing, formatting, calculations)
   - Exit points (responses, exports, database writes)

---

## PHASE 2: EDGE CASE CATEGORIES

Systematically scan for these categories:

### Input Boundaries
| Edge Case | What to Check | Fix Pattern |
|-----------|---------------|-------------|
| Empty/null values | Strings, arrays, objects passed as null/undefined/empty | Default values, early returns, null coalescing |
| Boundary values | Min/max integers, empty strings, zero-length arrays | Explicit boundary checks before processing |
| Type mismatches | String where number expected, object where array expected | Type validation/coercion at boundaries |
| Malformed input | Invalid JSON, corrupted files, truncated data | Try-catch with graceful degradation |
| Oversized input | Extremely long strings, massive arrays, huge file uploads | Size limits, pagination, streaming |
| Unicode/encoding | Emojis, RTL text, special characters, mixed encodings | Proper encoding handling, normalization |
| Injection attempts | SQL, XSS, command injection patterns | Sanitization, parameterized queries, escaping |

### State & Timing
| Edge Case | What to Check | Fix Pattern |
|-----------|---------------|-------------|
| Race conditions | Concurrent writes, double-submits, parallel requests | Locks, idempotency keys, optimistic locking |
| Stale state | Cache invalidation, outdated UI, expired tokens | TTL checks, refresh mechanisms, versioning |
| Partial state | Interrupted operations, partial saves, half-migrations | Transactions, rollback logic, atomic operations |
| Initialization order | Accessing before ready, undefined dependencies | Lazy initialization, dependency injection |
| Timeout scenarios | Long-running operations, unresponsive services | Timeout limits, abort controllers, fallbacks |

### Error Handling
| Edge Case | What to Check | Fix Pattern |
|-----------|---------------|-------------|
| Uncaught exceptions | Missing try-catch, unhandled promise rejections | Wrap risky operations, global error handlers |
| Silent failures | Swallowed errors, empty catch blocks | Log errors, fail explicitly or degrade gracefully |
| Error propagation | Errors lost in promise chains, callbacks | Proper error bubbling, error boundaries (React) |
| Retry exhaustion | What happens after max retries? | Final fallback, user notification, logging |
| Cascading failures | One failure causes chain of failures | Circuit breakers, isolation, graceful degradation |

### Network & External Services
| Edge Case | What to Check | Fix Pattern |
|-----------|---------------|-------------|
| Network offline | Complete network failure | Offline detection, cached responses, queue requests |
| Slow responses | High latency, degraded performance | Loading states, timeouts, skeleton UIs |
| Partial responses | Truncated data, incomplete payloads | Response validation, checksums |
| API rate limits | 429 responses, throttling | Exponential backoff, request queuing |
| Service unavailable | 5xx errors, maintenance windows | Retry logic, fallback services, maintenance mode |
| API contract changes | Breaking changes from external services | Version checking, defensive parsing |

### User Behavior
| Edge Case | What to Check | Fix Pattern |
|-----------|---------------|-------------|
| Double clicks/submits | Multiple form submissions, duplicate requests | Debouncing, disabled states, idempotency |
| Back button | Unexpected navigation, stale forms | History management, form state preservation |
| Refresh mid-operation | Page reload during async operations | Operation recovery, local storage backup |
| Multiple tabs/windows | Conflicting sessions, stale data | Tab sync, session management |
| Copy-paste issues | Formatted text, hidden characters, whitespace | Input sanitization, trim/normalize |
| Browser-specific | Safari date parsing, IE compatibility, mobile quirks | Feature detection, polyfills, testing matrix |

### Data Edge Cases
| Edge Case | What to Check | Fix Pattern |
|-----------|---------------|-------------|
| First/last items | Empty lists, single item lists, pagination boundaries | Boundary-aware logic, guard clauses |
| Date/time issues | Timezones, DST transitions, leap years, epoch boundaries | UTC storage, timezone-aware display |
| Floating point | Precision errors in calculations, currency math | Decimal libraries, integer cents, rounding strategies |
| Large numbers | Integer overflow, precision loss | BigInt, string representations, validation |
| Missing relationships | Orphaned records, broken foreign keys | Cascade handling, null checks on joins |

---

## PHASE 3: DETECTION METHODS

Use these techniques to find edge cases:

### 1. Code Pattern Search
```bash
# Find potential null issues
grep -rn "\.length" --include="*.ts" --include="*.js" | head -50

# Find unhandled promises
grep -rn "async\|await\|\.then\|\.catch" --include="*.ts" | head -50

# Find TODO/FIXME markers (often mark known edge cases)
grep -rn "TODO\|FIXME\|HACK\|XXX" --include="*.ts" --include="*.js"
```

### 2. Logical Analysis
For each function, ask:
- What happens if every parameter is null/undefined?
- What happens if every array is empty?
- What happens if every string is empty or extremely long?
- What happens if the operation takes 30 seconds?
- What happens if it's called twice simultaneously?
- What happens if the network fails mid-operation?

### 3. Boundary Tracing
For each external integration:
- What's the minimum valid response?
- What's the maximum expected response size?
- What happens on 4xx errors? 5xx errors? Timeouts?
- What's the retry strategy?

---

## PHASE 4: PRIORITIZE FIXES

Rank by severity and likelihood:

### Critical (fix immediately)
- Security vulnerabilities (injection, auth bypass)
- Data corruption risks
- Payment/transaction failures
- Crashes/unhandled exceptions in core flows

### High (fix before deploy)
- User-facing errors without graceful handling
- Race conditions in common operations
- Missing validation on public inputs
- Broken error recovery paths

### Medium (fix soon)
- Poor UX in edge scenarios (loading states, error messages)
- Non-critical feature failures
- Performance degradation in edge cases
- Missing retry logic for external services

### Low (fix when convenient)
- Cosmetic issues in rare scenarios
- Verbose logging in edge cases
- Minor UX inconsistencies

---

## PHASE 5: IMPLEMENT FIXES

Apply fixes using defensive patterns:

### Guard Clauses
```typescript
// Before: buried null check
function process(data) {
  if (data) {
    if (data.items) {
      return data.items.map(x => x.value);
    }
  }
  return [];
}

// After: early returns
function process(data) {
  if (!data?.items?.length) return [];
  return data.items.map(x => x.value);
}
```

### Safe Access Patterns
```typescript
// Defensive object access
const value = response?.data?.user?.name ?? 'Unknown';

// Safe array operations
const first = items?.[0] ?? defaultItem;
const mapped = (items ?? []).map(transform);
```

### Error Boundaries
```typescript
// Wrap risky operations
async function fetchData() {
  try {
    const response = await api.get('/data');
    return response.data;
  } catch (error) {
    if (error.code === 'NETWORK_ERROR') {
      return getCachedData();
    }
    throw new AppError('Failed to fetch data', { cause: error });
  }
}
```

### Timeout Protection
```typescript
// Add timeouts to external calls
const controller = new AbortController();
const timeoutId = setTimeout(() => controller.abort(), 5000);

try {
  const response = await fetch(url, { signal: controller.signal });
} finally {
  clearTimeout(timeoutId);
}
```

---

## PHASE 6: VERIFICATION

After implementing fixes:

1. **Test the edge case directly**:
   - Manually trigger the edge case scenario
   - Write a unit test capturing the edge case
   - Verify the fix handles it gracefully

2. **Run existing tests**:
   ```bash
   pnpm test
   ```

3. **Check for regressions**:
   ```bash
   pnpm build
   pnpm lint
   ```

4. **Verify error handling** - Ensure failures are:
   - Logged appropriately
   - Reported to the user clearly (if user-facing)
   - Recoverable where possible

---

## CONSTRAINTS

- **Don't over-engineer** - Fix real edge cases, not hypothetical ones
- **Maintain readability** - Defensive code should still be clear
- **Preserve performance** - Don't add expensive checks in hot paths
- **Document decisions** - Comment why edge cases are handled a certain way
- **Test the fix** - Every edge case fix should have a corresponding test

---

## COMPLETION CRITERIA

Work is done when:
- [ ] All critical paths reviewed for edge cases
- [ ] Input validation covers boundary conditions
- [ ] Error handling exists for all external calls
- [ ] No uncaught promise rejections
- [ ] Loading and error states exist for async operations
- [ ] Tests pass
- [ ] Build succeeds

**Report summary when complete:**
- Edge cases identified (categorized by severity)
- Fixes implemented
- Tests added
- Any remaining edge cases noted for future work

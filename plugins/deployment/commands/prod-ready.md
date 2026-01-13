---
description: Gets ready to push for prod
---

# PRODUCTION READY

Perform a comprehensive production-readiness audit. Verify the codebase meets security, performance, reliability, and quality standards before deployment.

---

## PHASE 1: PRE-FLIGHT CHECKS

1. **Verify baseline stability** - Run before any changes:
   ```bash
   pnpm build
   pnpm test
   pnpm lint
   ```
   If any fail, STOP and fix first. Don't proceed with a broken baseline.

2. **Determine deployment scope** - Ask or infer:
   - Full application or specific feature/module?
   - Environment target (staging vs production)?
   - Any known risk areas to focus on?

3. **Review recent changes** - Check what's new since last deploy:
   ```bash
   git log --oneline -20
   git diff main --stat
   ```

---

## PHASE 2: CODE CLEANUP

Scan and remove development artifacts:

### Debug Code Removal
| Pattern | Check | Action |
|---------|-------|--------|
| Console statements | `console.log`, `console.debug`, `console.trace` | Remove or replace with proper logging |
| Debugger statements | `debugger` keyword | Remove entirely |
| Debug flags | `DEBUG=true`, `VERBOSE=true` | Remove or ensure disabled in prod config |
| Test data | Hardcoded test users, mock data in production code | Remove or move to test files |
| Commented code | Large blocks of commented-out code | Remove if no longer needed |

### Incomplete Work Markers
| Pattern | Check | Action |
|---------|-------|--------|
| TODO comments | `TODO`, `FIXME`, `HACK`, `XXX` | Resolve critical ones, document others |
| Placeholder values | `placeholder`, `xxx`, `test123` | Replace with real values |
| Temporary implementations | `// temporary`, `// workaround` | Evaluate if prod-safe |
| Unfinished features | Half-implemented functions | Complete or remove |

### Search commands:
```bash
# Find debug statements
grep -rn "console\.\(log\|debug\|trace\)" --include="*.ts" --include="*.tsx" --include="*.js"

# Find debugger statements
grep -rn "debugger" --include="*.ts" --include="*.tsx" --include="*.js"

# Find TODO markers
grep -rn "TODO\|FIXME\|HACK\|XXX" --include="*.ts" --include="*.tsx" --include="*.js"
```

---

## PHASE 3: SECURITY AUDIT

Critical security checks before production:

### Authentication & Authorization
| Check | What to Verify | Risk Level |
|-------|----------------|------------|
| Auth on all routes | Every API endpoint has proper auth guards | Critical |
| Role verification | Permission checks before sensitive operations | Critical |
| Token expiration | Tokens expire appropriately, refresh works | High |
| Password handling | Never logged, properly hashed, not in URLs | Critical |
| Session management | Secure cookies, proper invalidation | High |

### Input Validation
| Check | What to Verify | Risk Level |
|-------|----------------|------------|
| SQL injection | Parameterized queries, no string concatenation | Critical |
| XSS prevention | Output escaping, CSP headers, sanitized HTML | Critical |
| Command injection | No user input in shell commands | Critical |
| Path traversal | No user input in file paths without validation | Critical |
| Request size limits | Max upload sizes, body limits configured | Medium |

### Data Protection
| Check | What to Verify | Risk Level |
|-------|----------------|------------|
| Sensitive data exposure | PII not logged, masked in errors | Critical |
| HTTPS enforcement | All endpoints require HTTPS | Critical |
| CORS configuration | Only allowed origins, proper methods | High |
| API key security | Not hardcoded, not in client bundles | Critical |
| Database credentials | Environment variables, not in code | Critical |

### Search commands:
```bash
# Find potential secrets in code
grep -rn "password\|secret\|api_key\|apikey\|token" --include="*.ts" --include="*.tsx" --include="*.js" --include="*.json" | grep -v node_modules

# Find dangerous patterns
grep -rn "eval\|innerHTML\|dangerouslySetInnerHTML" --include="*.ts" --include="*.tsx" --include="*.js"
```

---

## PHASE 4: ERROR HANDLING & RESILIENCE

Verify graceful failure handling:

### Error Boundaries
| Check | What to Verify | Action |
|-------|----------------|--------|
| Global error handler | Catches unhandled exceptions | Add if missing |
| Promise rejections | No unhandled promise rejections | Wrap with try-catch or .catch() |
| API error responses | Consistent error format, no stack traces in prod | Standardize error responses |
| Fallback UI | Error boundaries around major components (React) | Add where critical |
| Offline handling | Graceful degradation when network unavailable | Add offline detection |

### External Service Resilience
| Check | What to Verify | Action |
|-------|----------------|--------|
| Timeout configuration | All HTTP calls have timeouts | Add timeouts (5-30s typical) |
| Retry logic | Transient failures trigger retries | Add with exponential backoff |
| Circuit breakers | Failing services don't cascade | Add for critical dependencies |
| Fallback behavior | App works (degraded) if service is down | Define fallbacks |

### Logging for Production
| Check | What to Verify | Action |
|-------|----------------|--------|
| Error logging | All errors logged with context | Add structured logging |
| Request tracing | Request IDs for correlation | Add request ID middleware |
| No sensitive data in logs | PII, passwords, tokens filtered | Sanitize log output |
| Log levels | Appropriate verbosity for production | Set to warn/error |

---

## PHASE 5: CONFIGURATION VALIDATION

### Environment Variables
Verify all required environment variables:

1. **List required variables** - Check `.env.example` or config files
2. **Validate presence** - Ensure prod environment has all required values
3. **No defaults for secrets** - Secrets must be explicitly set, not fallback values
4. **Correct environment** - `NODE_ENV=production`, debug flags disabled

```bash
# Find all env var usage
grep -rn "process\.env\|import\.meta\.env" --include="*.ts" --include="*.tsx" --include="*.js"
```

### Feature Flags
| Check | What to Verify | Action |
|-------|----------------|--------|
| Beta features | Disabled or properly gated | Review flag states |
| A/B tests | Correct variants for prod | Verify configuration |
| Kill switches | Critical features can be disabled | Ensure toggles work |

### Third-Party Services
| Check | What to Verify | Action |
|-------|----------------|--------|
| API endpoints | Pointing to production, not staging | Verify URLs |
| Credentials | Production credentials, not test keys | Verify env vars |
| Rate limits | Production quotas sufficient | Check limits |
| Webhooks | Pointing to production URLs | Verify endpoints |

---

## PHASE 6: PERFORMANCE VERIFICATION

### Bundle & Build
| Check | What to Verify | Action |
|-------|----------------|--------|
| Production build | Minified, tree-shaken, optimized | `pnpm build` |
| Bundle size | Within acceptable limits | Analyze bundle |
| Source maps | Configured for error tracking (not public) | Verify setup |
| Dead code | Unused dependencies removed | `depcheck` or equivalent |

### Runtime Performance
| Check | What to Verify | Action |
|-------|----------------|--------|
| N+1 queries | No excessive database calls | Review data fetching |
| Memory leaks | No obvious leak patterns | Check event listeners, intervals |
| Expensive operations | Heavy computation not blocking UI | Move to workers if needed |
| Caching | Appropriate cache headers, memoization | Add where beneficial |

### Analyze commands:
```bash
# Build and check size (JS/TS)
pnpm build
du -sh dist/

# Check for unused dependencies
pnpx depcheck
```

---

## PHASE 7: DEPENDENCY AUDIT

### Security Vulnerabilities
```bash
# Check for known vulnerabilities
pnpm audit
# For Python
pip-audit
```

### Dependency Health
| Check | What to Verify | Action |
|-------|----------------|--------|
| Critical vulnerabilities | Zero critical/high severity | Update or patch |
| Outdated packages | Major versions reasonably current | Plan updates |
| License compliance | Compatible licenses for production | Review licenses |
| Lock file | Committed and up-to-date | Commit lock file |

---

## PHASE 8: TESTING VERIFICATION

### Test Coverage
| Check | What to Verify | Action |
|-------|----------------|--------|
| Tests pass | All tests green | `pnpm test` |
| Critical paths covered | Auth, payments, core features tested | Add if missing |
| No skipped tests | No `.skip()` or `@pytest.mark.skip` on critical tests | Unskip or document |
| E2E tests | Happy paths verified | Run E2E suite |

### Pre-Deploy Testing
```bash
# Full test suite
pnpm test

# E2E tests if available
pnpm test:e2e

# Type checking
pnpm typecheck  # or tsc --noEmit
```

---

## PHASE 9: DOCUMENTATION CHECK

### Required Documentation
| Doc | What to Verify | Action |
|-----|----------------|--------|
| README | Updated with current setup/deploy instructions | Update if stale |
| CLAUDE.md / AGENTS.md | Reflects current architecture | Update if changed |
| API documentation | Endpoints documented, examples current | Verify accuracy |
| Environment variables | All vars documented with descriptions | Update .env.example |
| CHANGELOG | Recent changes documented | Add entry for release |

### Code Documentation
| Check | What to Verify | Action |
|-------|----------------|--------|
| Complex logic | Non-obvious code has explanatory comments | Add where needed |
| Public APIs | Functions/classes have doc comments | Add JSDoc/docstrings |
| Deprecations | Deprecated code marked clearly | Add @deprecated tags |

---

## PHASE 10: FINAL VERIFICATION

Run the complete verification suite:

```bash
# 1. Clean install (verify lock file integrity)
rm -rf node_modules
pnpm install --frozen-lockfile

# 2. Type check
pnpm typecheck

# 3. Lint (zero tolerance)
pnpm lint

# 4. Full test suite
pnpm test

# 5. Production build
pnpm build

# 6. Security audit
pnpm audit
```

All commands must pass with zero errors.

---

## PHASE 11: DEPLOYMENT CHECKLIST

Final manual checks:

- [ ] All critical bugs fixed
- [ ] Database migrations ready (if any)
- [ ] Rollback plan documented
- [ ] Monitoring/alerting configured
- [ ] Health check endpoint verified
- [ ] Rate limiting configured
- [ ] Backup verified (database)
- [ ] Team notified of deployment

---

## CONSTRAINTS

- **Don't skip security checks** - Every item in Phase 3 must be verified
- **Don't deploy with critical vulnerabilities** - Fix first, deploy after
- **Don't remove error handling** - When cleaning up, preserve proper error handling
- **Don't rush** - A bad deploy costs more than a delayed deploy
- **Don't deploy on Fridays** - Unless you have weekend support ready

---

## COMPLETION CRITERIA

Work is done when:
- [ ] All debug code removed
- [ ] Security audit passed
- [ ] Error handling verified
- [ ] Configuration validated
- [ ] Dependencies audited (zero critical vulns)
- [ ] All tests passing
- [ ] Production build succeeds
- [ ] Documentation current

**Report summary when complete:**
- Issues found and fixed
- Security concerns addressed
- Outstanding items (if any) with justification
- Confidence level for deployment (high/medium/low)
- Recommended: immediate deploy / deploy after X / do not deploy

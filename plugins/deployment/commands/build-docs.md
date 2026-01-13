---
description: Creates and updates documentation
---

# DOCUMENTATION GENERATION & MAINTENANCE

Create comprehensive, accurate, and maintainable documentation. Goal: any developer should understand the project within 5 minutes of reading.

---

## PHASE 1: RECONNAISSANCE

Before writing anything:

1. **Scan the codebase** - Understand architecture, entry points, and core flows
2. **Identify the audience** - Internal devs? Open source contributors? End users? API consumers?
3. **Check existing docs** - Note what exists, what's outdated, what's missing
4. **Run the project** - Document what you actually observe, not what you assume

---

## PHASE 2: IDENTIFY DOCUMENTATION NEEDS

Determine what documentation this project requires:

### Always Required
- **README.md** - Project overview, quick start, basic usage
- **Installation instructions** - All dependencies and setup steps
- **Configuration** - Environment variables, config files, options

### If Applicable
- **API documentation** - Endpoints, request/response formats, auth
- **Architecture overview** - System design, component relationships
- **Contributing guide** - How to set up dev environment, submit PRs
- **Changelog** - Version history and breaking changes
- **CLI reference** - Commands, flags, examples
- **Deployment guide** - Production setup, CI/CD, infrastructure

### Check For
- Undocumented features
- Outdated screenshots or examples
- Broken links
- Missing error handling guidance
- Unclear edge cases

---

## PHASE 3: README STRUCTURE

A production README must include:

```
1. Project name + one-line description
2. Badges (if applicable: build status, version, license)
3. Key features (bullet list, 3-7 items)
4. Quick start (copy-paste commands to get running)
5. Installation (detailed steps)
6. Usage examples (real, working code)
7. Configuration (all options explained)
8. API reference (or link to full docs)
9. Contributing (or link to CONTRIBUTING.md)
10. License
```

### Quality Standards
- Every code example must be tested and working
- Commands should be copy-pasteable without modification
- Use consistent formatting and heading hierarchy
- Prefer tables over prose for reference data
- Include expected output for commands when helpful

---

## PHASE 4: CODE DOCUMENTATION

### Inline Comments
- Explain WHY, not WHAT (code shows what)
- Document non-obvious decisions and edge cases
- Mark TODO/FIXME with context, not just the keyword
- Remove outdated or misleading comments

### Function/Class Documentation
- Purpose (one sentence)
- Parameters with types and descriptions
- Return value and type
- Exceptions/errors that can be thrown
- Usage example for complex functions

### File Headers
- Only add if file purpose isn't obvious from name/contents
- Keep brief: 1-2 lines max

---

## PHASE 5: WRITING STANDARDS

### Tone
- Direct and concise
- Active voice ("Run the command" not "The command should be run")
- Second person ("You can configure..." not "Users can configure...")

### Formatting
- Use code blocks with language hints for syntax highlighting
- Use tables for structured data (env vars, CLI flags, etc.)
- Use admonitions sparingly (Note, Warning, Tip)
- Keep paragraphs short (3-4 sentences max)

### Avoid
- Marketing language ("blazingly fast", "revolutionary")
- Unnecessary jargon
- Walls of text without structure
- Placeholder text or TODOs in shipped docs
- Excessive emojis

---

## PHASE 6: VERIFICATION

Before finalizing:

1. **Test all commands** - Run every code example and command
2. **Check all links** - Verify internal and external links work
3. **Fresh eyes test** - Does it make sense to someone unfamiliar?
4. **Version accuracy** - Do versions/dependencies match reality?
5. **Consistency check** - Same terms, formatting throughout

---

## EXECUTION RULES

- **Update, don't duplicate** - Modify existing docs before creating new files
- **Single source of truth** - Each piece of info lives in one place
- **Match existing style** - Follow conventions already in the project
- **Verify with code** - Documentation must match actual behavior
- **Use web search and context7** - Confirm current best practices and library-specific conventions

---

## COMPLETION CRITERIA

Work is done when:
- [ ] README exists and covers all required sections
- [ ] All code examples are tested and working
- [ ] No outdated information remains
- [ ] No broken links
- [ ] Installation works from scratch following only the docs
- [ ] Configuration options are fully documented
- [ ] Writing is clear, concise, and consistent

**DOCUMENTATION MUST BE ACCURATE AND COMPLETE. VERIFY EVERYTHING.**

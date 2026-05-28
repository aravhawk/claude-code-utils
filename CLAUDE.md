# AGENTS.md

These are defaults and priorities — not an exhaustive checklist. Apply what fits the task; when project conventions or correctness clearly matter more, follow those instead.

## Guardrails

- **Simplicity:** As simple as necessary, as complex as required. Senior-engineer test: if they'd call it overcomplicated, rewrite it. No speculative features, single-use abstractions, or unrequested flexibility.
- **Clarity:** State assumptions. Multiple interpretations → present options; don't pick silently. Unclear → ask, don't guess.
- **Correctness:** Root cause, not patches. Verify before done — proportionate to the change (tests, logs, or a concrete demo for non-trivial work).
- **Fresh info:** Web search for APIs, version-specific behavior, and facts that may be stale in training data. When adding dependencies, update the project's requirements/manifest.
- **Python envs:** Conda only — never venvs.
- **Meta:** Keep `AGENTS.md` and `CLAUDE.md` lean (architecture, dependencies, durable rules only). Never edit this file or `/Users/aravhawk/.claude/CLAUDE.md` without explicit confirmation.

## Workflow

- **Plan when:** Multi-step or architectural work, or when the approach is uncertain — spec first; re-plan if stuck. Simple sequential tasks don't need a formal plan. Cover obvious failure modes; don't over-spec hypotheticals.
- **Subagents:** Use for research, exploration, or parallel independent work — one focused task each. Don't delegate work that's faster to do directly.
- **Scope:** Change only what the task requires; match existing project style. Remove unused imports/variables only if your changes introduced them. Unrelated dead code or issues → mention, don't fix unless asked. Fix failing CI when you encounter it. Don't ask for guidance unless genuinely blocked.
- **Tasks:** Break complex work into trackable items (built-in task tools, else `tasks/todo.md`). Summarize progress at major steps.
- **Lessons (`tasks/lessons.md`):** On meaningful corrections (recurring mistakes, wrong assumptions, missed conventions) → log `Pattern:` + one-line `Rule:`. Skim at session start if present. Keep the file useful, not ceremonial — prune stale entries; stay under ~150 lines (compress before adding). Default home is lessons; promote to `AGENTS.md`/`CLAUDE.md` only for core architecture rules.

## Preferences

### Defaults for new work

When **starting fresh** Node/web projects unless told otherwise:

- Package manager: `pnpm`
- Framework: latest Next.js with Turbopack
- Language: TypeScript
- Git remotes: SSH

These are starting defaults — not overrides. In an **existing repo**, follow whatever it already uses (`npm`, plain JS, webpack, HTTPS, Django, etc.) even when it differs from the above.

### Tooling

- **Codex:** Prefer for code review and when you're stuck on a hard problem. For routine implementation, debugging, and research, use your normal workflow first — don't route everything through Codex. Use the built-in code-reviewer only when explicitly asked.
- **Questions:** When you need a user decision or preference, use the ask-user tool — don't guess.
- **Dev servers:** If one is already running (e.g. `pnpm dev`), leave it running unless the task specifically requires restarting or reconfiguring it.

### About Arav

- Call me **Arav** in conversation. My full name is Arav Jain — don't use it in chat.
- GitHub username: **aravhawk**
- Environment: latest macOS on Apple Silicon (M3 Pro MacBook Pro), unless stated otherwise.
- Tone: warm and collaborative. That doesn't relax correctness, scope discipline, or attention to detail.

### Releases

When creating GitHub releases, title them `vX.Y.Z` unless asked otherwise.

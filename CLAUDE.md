# Core rules (must follow)
- Make every change as simple as possible, but as complex as necessary. Senior-engineer test: if they'd call it overcomplicated, rewrite it. No speculative features, abstractions for single-use code, or unrequested "flexibility".
- Think before coding. State assumptions explicitly; if multiple interpretations exist, present them -- don't pick silently. If something is unclear, stop and ask rather than guess.
- Always trace issues to their root cause and fix permanently -- never patch over symptoms.
- Default to web search for technical docs, API references, code examples, and any factual content (e.g., current info for portfolios, showcases, or apps) -- don't rely on training data for specifics that may be outdated.
- Plan every edge case when architecting any plan/product/feature.
- Keep requirements file(s) up to date.
- If an `ARCHITECTURE.mmd` file exists, keep it up to date with the latest changes in product architecture.
- If you create a `CLAUDE.md` or `AGENTS.md` file, keep it lean. Include learned rules, dependencies, and core architecture. Be intelligent about what you include -- cover important details without overcomplicating.
- Keep project details, learned codebase rules, and core changes up to date in `CLAUDE.md` and/or `AGENTS.md` (whichever exist).
- Ensure `.gitignore` excludes unnecessary files/folders (e.g., `.DS_Store`, `__pycache__`, `.idea`). Do not exclude `CLAUDE.md`, `AGENTS.md`, or `.cursorrules`.
- Never use venvs. When project-specific environments are needed, use conda.
- Minimize use of emojis, especially in code and docs.
- Never edit the global CLAUDE.md (/Users/aravhawk/.claude/CLAUDE.md) file.

## Workflow Discipline

### Planning
- If a task has 3+ steps or involves architectural decisions, enter plan mode first.
- If the current approach isn't working, stop and re-plan from scratch.
- Write detailed specs upfront. Verify the plan before implementing.
- Use plan mode for verification steps too, not just building.

### Subagent Strategy
- Use subagents to keep the main context window clean. Assign one focused task per subagent.
- Offload research, exploration, and parallel analysis to subagents. Launch multiple concurrently for independent subtasks.
- For large codebases, prefer `code-explorer` over the regular Explore tool.

### Self-Improvement Loop
- After ANY correction: add the mistake pattern and a prevention rule to `tasks/lessons.md`.
- At session start, review `tasks/lessons.md` for the relevant project (if it exists).
- Prune `tasks/lessons.md` aggressively: if a lesson becomes redundant, outdated, or no longer needed (e.g., the underlying code/API changed, or the rule was promoted into `CLAUDE.md`/`AGENTS.md`), delete it immediately.
- If `tasks/lessons.md` ever exceeds 500 lines, compress it by at least 40% in the same pass -- merge duplicates, tighten wording, and drop low-value entries -- to make room for new content.
- Example entry:
  `### Incorrect import paths`
  `Pattern: Used relative imports instead of the project's alias convention.`
  `Rule: Always check existing imports in the file before adding new ones. Match the project's import style.`

### Surgical Changes
- Touch only what the task requires. Don't "improve" adjacent code, comments, or formatting; don't refactor what isn't broken.
- Match existing style even if you'd do it differently. If you spot unrelated dead code, mention it -- don't delete it.
- Clean up only your own mess: remove imports/variables/functions your changes made unused; leave pre-existing dead code alone unless asked.
- Test: every changed line should trace directly to the user's request.

### Verification Before Done
- Define verifiable success criteria before implementing. "Fix the bug" → "write a test that reproduces it, then make it pass." Strong criteria let you loop independently; weak ones ("make it work") cause rework.
- Never mark a task complete without proving it works: run tests, check logs, demonstrate correctness.
- Before presenting a solution, review it as if auditing someone else's PR. Ask: "Would I approve this in a code review?"
- Diff behavior between main and your changes when relevant.

### Elegance (non-trivial changes only)
- For non-trivial changes, pause and consider whether a cleaner approach exists. Simplicity is the default; pursue elegance only when the simple path is a workaround rather than a real solution.
- If a fix works around the problem rather than solving it directly, step back and implement the direct solution.
- Skip this entirely for straightforward fixes.

### Bug Fixing
- Resolve bugs end-to-end: read logs, trace the error, fix the root cause, verify the fix. No plan mode needed unless the fix requires architectural changes.
- Do not ask for intermediate guidance unless genuinely blocked on missing context.
- Fix failing CI tests proactively when encountered.

## Task Management
- Break complex work into discrete, trackable tasks before starting. Use the agent's built-in task tools when available; fall back to `tasks/todo.md` with checkable items otherwise.
- Summarize what changed at each major step.
- Document results when the work is complete.

## Preferences
- Always use Codex for code reviews or for solving things that you're stuck on (only use your own code-reviewer agent when specifically told to do so).
- Prefer `pnpm` over `npm` unless instructed otherwise or a project is built with npm.
- Always use the latest Next.js (with Turbopack) for Node/pnpm projects unless instructed otherwise.
- Prefer TypeScript for Next.js projects unless instructed otherwise.
- Prefer `git` via SSH over HTTPS unless instructed otherwise.
- Call me Arav.
- My full name is Arav Jain (don't call me that in a conversation).
- My GitHub username is aravhawk.
- Assume I'm on the latest macOS unless stated otherwise.
- I use an M3 Pro MacBook Pro.
- Be friendly: warm, collaborative, and helpful; do not let that affect your code quality, technical accuracy, attention to detail, or diversion from the task at hand.
- GitHub Release titles should always be in the vX.Y.Z format unless requested otherwise.
- For asking me questions, use your inbuilt ask user question tool/feature.
- If there is a dev server running (i.e. pnpm dev), do not mess with it.

## gstack
- Use the `/browse` skill from gstack for all web browsing. Never use `mcp__claude-in-chrome__*` tools.
- Available gstack skills: `/office-hours`, `/plan-ceo-review`, `/plan-eng-review`, `/plan-design-review`, `/design-consultation`, `/design-shotgun`, `/design-html`, `/review`, `/ship`, `/land-and-deploy`, `/canary`, `/benchmark`, `/browse`, `/connect-chrome`, `/qa`, `/qa-only`, `/design-review`, `/setup-browser-cookies`, `/setup-deploy`, `/retro`, `/investigate`, `/document-release`, `/codex`, `/cso`, `/autoplan`, `/plan-devex-review`, `/devex-review`, `/careful`, `/freeze`, `/guard`, `/unfreeze`, `/gstack-upgrade`, `/learn`.

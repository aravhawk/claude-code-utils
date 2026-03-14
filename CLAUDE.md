# Core rules (must follow)
- Don't overcomplicate tasks.
- Default to web search for technical docs, API references, code examples, and any factual content (e.g., current info for portfolios, showcases, or apps)—don't rely on training data for specifics that may be outdated.
- Plan every edge case when architecting any plan/product/feature.
- Keep requirements file(s) up to date.
- If an `ARCHITECTURE.mmd` file exists, always keep it up to date with the latest changes in product architecture.
- If you create a `CLAUDE.md` or `AGENTS.md` file, don't bloat it; agents are great at exploring codebases and figuring out commands typically. Keep learned rules, dependencies, and core architecture in there. Be intelligent about what you put in there. Don't leave out important details, but don't overcomplicate it either.
- Keep project details, learned codebase rules, and core changes in the codebase up to date in `CLAUDE.md` and/or `AGENTS.md` (whichever exist).
- Ensure `.gitignore` excludes unnecessary files/folders (e.g., `.DS_Store`, `__pycache__`, `.idea`). Use judgment so no unnecessary files are added to git. Do not exclude `CLAUDE.md`, `AGENTS.md`, or `.cursorrules`.
- Never use venvs. When project-specific environments are needed, use conda.
- Minimize use of emojis, especially in code and docs.

## Preferences
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

---
name: arch-mmd
description: Creates/updates an ARCHITECTURE.mmd file mapping out the codebase/product. Use whenever the user asks to see the architecture of the codebase/product or when the architecture changes and the current ARCHITECTURE.mmd file is out of date.
---

# Architecture Diagram Generator

Create or update an `ARCHITECTURE.mmd` Mermaid diagram that maps out the codebase's structure, components, and data flow.

---

## PHASE 1: EXPLORE THE CODEBASE

Thoroughly scan the project to understand its architecture:

1. **Identify project type** — framework, language, monorepo vs single app
2. **Map top-level structure** — directories, entry points, config files
3. **Identify key modules** — the actual files/packages that form the backbone
4. **Trace data flow** — how data moves through the system (user action -> entry point -> handler -> service -> store, etc.)
5. **Find external dependencies** — databases, APIs, queues, caches, third-party services
6. **Note boundaries** — client/server split, module boundaries, package boundaries in monorepos

---

## PHASE 2: BUILD THE DIAGRAM

Use `graph TD` (top-down) as the default direction. Use `graph LR` (left-right) only if the architecture is pipeline-shaped or heavily horizontal.

### Node labels

Include the **file/module name** and a **brief description** using `<br/>` for multi-line labels:

```
ENGINE["engine.py<br/>CodeLoader + Orchestrator"]
CLI["cli.py<br/>Arg parsing + validation"]
```

For simple/obvious nodes, a single-line label is fine:

```
DB[(PostgreSQL)]
```

### Node IDs

Use short, uppercase IDs: `CLI`, `ENGINE`, `AUTH`, `DB`, `API`

### Connections

- Use `-->` for standard flow
- Label edges with the technology, protocol, or relationship when it adds clarity: `-->|"REST"| `, `-->|"queries"|`
- Use `&` fan-in syntax when multiple nodes feed into one target:

```
P1 & P2 & P3 --> AGENT
```

### Color coding with `style`

Color-code nodes by their **role** to make the diagram scannable at a glance. Pick a small, consistent palette (4-6 colors). Always include `color:#fff` for readability.

Role-to-color suggestions (adapt to the project):

| Role | Color | Example |
|---|---|---|
| Entry point / user-facing | `#e74c3c` (red) | CLI, UI, API gateway |
| Core engine / orchestrator | `#2980b9` (blue) | Main engine, coordinator |
| Agent / processor | `#8e44ad` (purple) | Workers, agents, handlers |
| External service / API | `#e67e22` (orange) | Third-party APIs, LLMs |
| Output / result | `#27ae60` (green) | Reports, exports, responses |
| Infrastructure / runtime | `#16a085` (teal) | Runners, browsers, terminals |

Example:
```
style ENGINE fill:#2980b9,color:#fff
style REPORT fill:#27ae60,color:#fff
```

### Node shapes

- `["Label"]` — default rectangle for most components
- `[(Database)]` — databases and data stores only
- No other shapes needed in most cases; keep it uniform

---

## PHASE 3: WRITE THE FILE

1. Write the diagram to `ARCHITECTURE.mmd` in the project root
2. If `ARCHITECTURE.mmd` already exists, read it first, then update it to reflect the current state — don't start from scratch unless the existing diagram is fundamentally wrong
3. The file should contain ONLY the Mermaid diagram — no markdown fences, no frontmatter, no prose

---

## CONSTRAINTS

- **Map real modules, not abstractions** — nodes should correspond to actual files, packages, or services in the codebase, not vague concepts like "Business Logic Layer"
- **Include brief descriptions** — use `<br/>` in labels so someone unfamiliar with the codebase can understand each node's role
- **Always color-code** — a diagram without `style` directives is incomplete
- **Right level of granularity** — show the key files/modules that form the architecture, not every single file, and not just 3 abstract boxes
- **No orphan nodes** — every node must connect to at least one other node
- **Accuracy over completeness** — only include components confirmed to exist in the codebase
- **Update, don't recreate** — when updating an existing diagram, preserve the user's structural choices and only change what's outdated

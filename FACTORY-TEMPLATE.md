# AI Factory Setup — Interactive Guide v2

> Drop this file into any new project root and paste its contents into your AI assistant (Claude Code, Cursor, Aider, Windsurf, or any AI with file-write access).
> The AI will interview you phase by phase and write every factory file to disk.

---

## AI INSTRUCTIONS

You are a **factory setup assistant**. Your job is to guide the developer through setting up the AI Factory Method for a new project by asking questions and generating all factory files directly to disk.

### Rules

1. **Phases are sequential.** Run Phase 0 through Phase 9 in order. Never generate a later phase's files before the current phase is answered.
2. **Ask, then generate.** Present each phase's questions as a numbered list. Wait for the developer's answers before writing any files.
3. **Confirm after each phase.** After writing files, report exactly what was written, then ask: "Ready to continue to Phase N — [Name]?"
4. **Handle skips gracefully.** If the developer says "skip", "default", or "not sure", choose a sensible default, document it in the file under `## Open Questions`, and continue.
5. **Write complete files.** Populate every section of every template with the developer's actual answers. Do not leave placeholder text unfilled.
6. **Create directories as needed** before writing files.
7. **End with a summary.** After Phase 9, output a table of every file generated.

### Context File Name by Tool

| Tool | Context file name |
|------|-------------------|
| Claude Code | `CLAUDE.md` |
| Cursor | `.cursorrules` |
| GitHub Copilot | `.github/copilot-instructions.md` |
| Windsurf | `.windsurfrules` |
| Aider | `CONVENTIONS.md` |
| Other / Unknown | `PROJECT-CONTEXT.md` |

### Skill Auto-Discovery Location by Tool

| Tool | Auto-discovery location | File format |
|------|------------------------|-------------|
| Claude Code | `.claude/commands/` | `factory-<name>.md` |
| Cursor | `.cursor/rules/` | `factory-<name>.mdc` (front matter: `alwaysApply: false`) |
| Windsurf | `.windsurf/rules/` | `factory-<name>.md` |
| GitHub Copilot | `.github/copilot-instructions.md` (appended) | inline instructions block |
| Aider | no auto-discovery — reference `.factory/skills/` manually in chat | n/a |
| Other / Unknown | `.factory/skills/` only | n/a |

### How to Begin

Start your response with exactly this introduction, then immediately ask the Phase 0 questions — do not wait for the developer to prompt you further:

> "I'm your factory setup assistant. I'll guide you through the AI Factory Method setup by asking questions phase by phase and writing every factory file to disk as we go.
>
> **Phase 0 — Bootstrap.** Three quick questions to get started:"

---

## PHASE 0 — Bootstrap

### Questions to ask

1. What is the name of your project?
2. Which AI tool are you using? *(Claude Code / Cursor / GitHub Copilot / Windsurf / Aider / Other)*
3. What is the project root path? *(press Enter or type `.` to use the current directory)*

### After answering — create the scaffold

Run this in a shell (or equivalent for the current platform):

```
mkdir -p .factory/docs .factory/tasks .factory/skills .factory/logs .factory/ux
```

Then write these files:

**`.factory/logs/factory.log`**
```
# Factory Log — {project_name}
# Format: [YYYY-MM-DD] TASK-NNN | STATUS: done/skipped/blocked | <summary>
# This file is append-only. Do not edit existing entries.

[{today}] FACTORY-INIT | STATUS: done | Project scaffold created — docs, tasks, skills stub, log, status
```

**`.factory/status.md`**
```
# Status — {project_name}

## Current Phase
`BOOTSTRAP` → `FILL DOCS` → `DISCOVERY` → `SKILLS` → `TASKS` → `EXECUTE` → `EXTRACT`

Current: **FILL DOCS**

## Current Task
_None yet — complete docs and discovery (Phases 1–7) before generating tasks._

## Factory Phase (execution)
`INIT` → `SCAFFOLD` → `CORE` → `FEATURES` → `POLISH`

## Completed Tasks

| Task | Title | Date |
|------|-------|------|
| FACTORY-INIT | Project scaffold created | {today} |

## Pending Tasks

| Task | Title | Phase |
|------|-------|-------|

## Blockers
_None_

## Last Note
Factory scaffold created. Completing docs now.
```

**`.factory/tasks/task-TEMPLATE.md`**
```
# Task NNN — <Title>

> Copy this file, rename to task-NNN.md, fill in all sections. Delete this line.

## Objective
_What needs to be built or changed. One clear paragraph._

## Context
_Which docs are relevant to this task._

- [ ] Read .factory/docs/app.md — always; domain model and feature scope live here
- [ ] Read .factory/docs/architecture.md — if this task touches structure or stack
- [ ] Read .factory/docs/frontend.md — if this task touches client code
- [ ] Read .factory/docs/backend.md — if this task touches server code
- [ ] Read .factory/docs/database.md — if this task touches schema or queries
- [ ] Read .factory/docs/ux.md — if this task touches UI or interaction
- [ ] Check .factory/ux/ for reference assets — if this task touches UI

## Scope

| Action | File |
|--------|------|
| Create | path/to/file.ts |
| Modify | path/to/other.ts |

## Acceptance Criteria

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Unit tests pass
- [ ] Build succeeds
- [ ] Lint passes

## Dependencies
_Tasks that must be complete before this one starts._

## Developer Review (optional — do after the AI cycle completes)
- [ ] Open the app / endpoint and verify the feature works end-to-end
- [ ] Check for visual regressions on affected screens
- [ ] Confirm no unintended side-effects on adjacent features
- [ ] {task-specific spot-check if obvious from the objective}

## Notes
_Fill in after execution: decisions made, issues encountered, anything deferred._
```

**`.factory/FACTORY.md`**
```
# AI Factory — {project_name}

## What Is the AI Factory Method?
A structured way to build software with AI assistance:
- Docs keep the AI and developer on the same context at all times
- Skills are generated from the stack — real invocations, not placeholders
- Tasks are atomic, testable units — no task starts until the previous is verified
- Logs create a traceable history of every factory order

---

## File Organization

### Directory Tree

  <project-root>/
  ├── {context_file}               ← AI context file — auto-loaded every session
  ├── status.md                    ← current project state dashboard
  ├── .<tool>/                     ← tool-specific skill auto-discovery (Phase 8)
  │   └── commands/ (or rules/)   ← derived copies of .factory/skills/ files
  └── .factory/
      ├── FACTORY.md               ← this file; factory process and structure reference
      ├── docs/                    ← living design documents (read before coding)
      │   ├── app.md               ← product vision — read first
      │   ├── architecture.md      ← stack and infrastructure decisions
      │   ├── ux.md                ← design language, screens, theme tokens
      │   ├── frontend.md          ← frontend conventions and folder structure
      │   ├── backend.md           ← backend conventions and layer architecture
      │   └── database.md          ← schema, migrations, delete strategy
      ├── tasks/                   ← numbered task files
      │   ├── task-TEMPLATE.md     ← blank schema; copy for every new task
      │   └── task-NNN.md          ← NNN = zero-padded 3-digit number (001, 002, ...)
      ├── skills/                  ← canonical skill source (all tools)
      │   ├── factory-next-task.md
      │   ├── factory-run-tasks.md
      │   ├── factory-log-progress.md
      │   ├── factory-manage-tasks.md
      │   ├── factory-customize.md
      │   └── factory-extract-template.md
      ├── ux/                      ← reference assets for the UX phase
      │   ├── theme.css            ← CSS custom property tokens (generated Phase 3)
      │   ├── links.md             ← per-screen reference links (optional)
      │   ├── palette.css          ← color token overrides (optional — takes precedence)
      │   └── screens/             ← per-screen low-fidelity HTML mocks (optional)
      └── logs/
          └── factory.log          ← append-only order log

### Naming Conventions

  Docs      → kebab-case.md — one concern per file (security.md, deployment.md)
  Tasks     → task-NNN.md  — number only, zero-padded 3 digits; title lives inside the file
  Skills    → factory-verb-noun.md — action-first, kebab-case, always prefixed with factory-
  Logs      → factory.log only — never create additional log files

### Rules for Adding New Files

  Docs     — go in .factory/docs/; register in context file Active Docs + task-TEMPLATE.md Context checklist + FACTORY.md Directory Tree
  Tasks    — go in .factory/tasks/; register in status.md Pending Tasks
  Skills   — go in .factory/skills/ (canonical); also write to tool-specific auto-discovery; update FACTORY.md Directory Tree
  Logs     — all entries go in .factory/logs/factory.log (append-only)
  Root     — only the context file and status.md live here; never add other files to root

---

## Factory Phases

  Phase 0 — Bootstrap    → scaffold dirs, stub files, factory.log
  Phase 1 — App Design   → fill app.md (product vision, domain, features)
  Phase 2 — Architecture → fill architecture.md (stack, monorepo, infra)
  Phase 3 — UX           → fill ux.md (design language, screens, theme tokens)
  Phase 4 — Frontend     → fill frontend.md (structure, conventions, state)
  Phase 5 — Backend      → fill backend.md (structure, layers, services)
  Phase 6 — Database     → fill database.md (schema, migrations, seeds)
  Phase 7 — Discovery    → lock scope decisions, record in factory.log
  Phase 8 — Skills       → generate context file + stack-specific skills (canonical + tool copy)
  Phase 9 — Tasks        → generate numbered task list from completed docs

---

## Task Execution Cycle (mandatory for every task)

  1. Read .factory/status.md          — identify the current task
  2. Read .factory/tasks/task-NNN.md  — load objective, scope, criteria
  3. Read every doc in the task's Context section
  4. Write all code
  5. Run tests   — fix — run tests again — must be green
  6. Run build   — must succeed
  7. Run lint    — must pass
  8. Run factory-log-progress skill — append factory.log + advance status.md

Loop limit: Stop after 3 failed fix attempts. Escalate — never loop silently.

---

## Factory Log Format

.factory/logs/factory.log is append-only:

  [YYYY-MM-DD] TASK-NNN | STATUS: done    | <summary>
  [YYYY-MM-DD] TASK-NNN | STATUS: skipped | <reason>
  [YYYY-MM-DD] TASK-NNN | STATUS: blocked | <blocker>
```

### Phase 0 completion message

> "Phase 0 complete. Scaffold created — 5 files written to `.factory/`.
>
> Ready to continue to **Phase 1 — App Design**?"

---

## PHASE 1 — App Design (`app.md`)

> This is the most important doc. All architecture, UX, and task generation decisions depend on it. Take your time here.

### Questions to ask

Present all questions at once as a numbered list. The developer can answer in sequence.

1. **Vision** — In one paragraph: what is this product, who is it for, and why does it exist? Be specific. *"A task manager" is not a vision. "A Kanban board for solo developers who need to ship side projects without context-switching overhead" is.*

2. **Target user** — Who is the primary user (role, context, technical level)? What are they trying to accomplish? What do they currently do without this app?

3. **Problem statement** — What specific friction or gap does this product remove? Describe the "before" and "after."

4. **Core user journey** — Walk through the primary flow in 4–8 steps. What does the user see first? What do they do? What happens next? What do they achieve?

5. **Domain model** — What are the main concepts in this domain and how do they relate? Write in plain language. *(e.g., "A Project has many Tasks. A Task belongs to one Project and can have many Comments.")*

6. **Core features (v1)** — What features must exist for the app to be minimally useful? Write each as a user-facing capability. *(e.g., "User can create a project", "System sends notification when task is overdue")*

7. **Out of scope (v1)** — What is explicitly NOT being built in v1? Name it clearly to prevent scope creep.

8. **Success criteria** — How will you know the product works? What can the user accomplish without reading documentation?

9. **Constraints** — Answer each of these:
   - Single user or multi-user?
   - Auth required?
   - Online only or offline capable?
   - Desktop, mobile, or both?
   - Real-time simulation, event-driven, or CRUD-only?
   - Persistent data or ephemeral?

### File to generate

Write `.factory/docs/app.md` — fully populated with the developer's answers:

```
# App Design — {project_name}

## Vision
{vision_answer}

## Target User
{target_user_answer}

## Problem Statement
{problem_statement_answer}

## Core User Journey
{journey_as_numbered_steps}

## Domain Model
{domain_model_in_plain_language}

## Core Features (v1)
{features_as_numbered_list}

## Out of Scope (v1)
{out_of_scope_list}

## Success Criteria
{success_criteria_list}

## Constraints

| Constraint | Decision |
|-----------|---------|
| Single user or multi-user? | {answer} |
| Auth required? | {answer} |
| Online only or offline capable? | {answer} |
| Desktop, mobile, or both? | {answer} |
| Real-time, event-driven, or CRUD-only? | {answer} |
| Persistent data or ephemeral? | {answer} |

## Open Questions
{any_decisions_deferred_to_later_phases_as_checkboxes}
```

### Phase 1 completion message

> "Phase 1 complete. `.factory/docs/app.md` written.
>
> Ready to continue to **Phase 2 — Architecture**?"

---

## PHASE 2 — Architecture (`architecture.md`)

### Questions to ask

1. What are the main moving parts of the system? *(e.g., React frontend, Node API, PostgreSQL database, Redis cache)*
2. Monorepo or separate repos? If monorepo, which tool? *(Nx / Turborepo / pnpm workspaces / other)*
3. Frontend framework? *(React / Vue / Svelte / Next.js / Angular / other)*
4. Backend runtime and framework? *(Node/Express / Node/Fastify / Python/FastAPI / Ruby/Rails / other)*
5. API style? *(REST / GraphQL / tRPC / other)*
6. Database? *(PostgreSQL / MySQL / SQLite / MongoDB / other)*
7. ORM or query builder? *(Drizzle / Prisma / SQLAlchemy / ActiveRecord / none / other)*
8. Auth strategy? *(JWT / sessions / OAuth / none)*
9. Real-time strategy? *(WebSocket / SSE / polling / none)*
10. Docker for local development? *(yes / no — if yes, where is the compose file?)*
11. Code style enforcement? *(ESLint / Biome / Prettier / Ruff / other / none)*

### File to generate

Write `.factory/docs/architecture.md`:

```
# Architecture — {project_name}

## System Overview
{describe_main_moving_parts}

## Stack Decisions

| Decision | Choice | Notes |
|----------|--------|-------|
| Repository structure | {monorepo_or_polyrepo} | {tool_if_monorepo} |
| Frontend framework | {frontend_framework} | |
| Backend runtime + framework | {backend_runtime_framework} | |
| API style | {api_style} | |
| Database | {database} | |
| ORM / query builder | {orm} | |
| Auth strategy | {auth} | |
| Real-time strategy | {realtime} | |
| Docker (local dev) | {yes_or_no} | {compose_file_path_if_yes} |
| Code style enforcement | {linting_tool} | |

## Project Structure
{top_level_directory_layout_based_on_answers}

## Open Questions
{deferred_decisions_as_checkboxes}
```

### Phase 2 completion message

> "Phase 2 complete. `.factory/docs/architecture.md` written.
>
> Ready to continue to **Phase 3 — UX**?"

---

## PHASE 3 — UX (`ux.md`)

### Before asking questions — check for references

List the contents of `.factory/ux/`. The developer may have placed reference materials there using this structure:

- `links.md` — sections per feature/screen, each section lists 2–3 URLs to real apps as visual reference
- `palette.css` — color tokens (overrides the AI-generated palette if provided)
- `screens/` subfolder — one HTML+inline-CSS file per core screen as a low-fidelity mock, named after the screen (e.g., `game-calendar.html`, `player-profile.html`)

**If files are found:** Read or analyze every file. Summarize what you found *(e.g., "I found links.md with references for 3 screens and palette.css with brand colors — I'll use these as the basis for the UX doc.")*. Extract concrete color/font values from them if present. If `links.md` is present, acknowledge each link and note what design pattern is being borrowed. Use findings to pre-fill as many answers as possible, then ask only the questions the references do not already answer.

**If the folder is empty:** Tell the developer *(e.g., "No references found in `.factory/ux/` — I'll ask all questions and define the UX from scratch.")*, then ask all questions below.

> **Tip for the developer:** You can place reference materials in `.factory/ux/` before running this phase — `links.md` with per-screen reference URLs, `palette.css` with brand colors, `screens/` with low-fidelity HTML mocks. The AI will work with whatever is there.

### Questions to ask

*(Skip any question already answered by the references.)*

1. What is the visual metaphor or design language? *(e.g., "dark sci-fi dashboard", "clean minimal productivity tool", "retro terminal aesthetic")*
2. Dark theme, light theme, or both?
3. Desktop-first, mobile-first, or fully responsive?
4. UI component library? *(shadcn/ui / Ant Design / MUI / Radix / none / other)*
5. Animation library? *(Framer Motion / GSAP / CSS-only / none / other)*
6. Data visualization? *(D3 / Recharts / Chart.js / none / other)*
7. What are the core screens or pages? *(list them with a one-line description of each)*
8. Primary brand color? *(hex or description — if unsure, describe the mood and the AI will suggest one based on the design language)*
9. Neutral palette style? *(warm greys / cool greys / pure black-and-white)*
10. Typography: heading font and body font? *(or "system font stack")*
11. Base spacing unit? *(4 px / 8 px)*
12. Border radius style? *(sharp — 0px / subtle — 4px / rounded — 8–12px / pill — 9999px)*

### Files to generate

Write `.factory/docs/ux.md` — fully populated:

```
# UX Design — {project_name}

## Design Language
{design_language_answer}

## Theme
{theme_answer}

## Responsive Strategy
{responsive_strategy_answer}

## Libraries

| Concern | Choice |
|---------|--------|
| UI components | {component_library} |
| Animation | {animation_library} |
| Data visualization | {dataviz_library} |

## Core Screens

{screens_as_numbered_list_with_one_line_description_each}

## Color & Typography

### Palette
| Token | Value | Usage |
|-------|-------|-------|
| --color-primary | {primary_hex} | CTAs, active states |
| --color-primary-dark | {primary_dark_hex} | Hover on primary |
| --color-surface | {surface_hex} | Page background |
| --color-surface-raised | {surface_raised_hex} | Cards, panels |
| --color-border | {border_hex} | Dividers |
| --color-text | {text_hex} | Body copy |
| --color-text-muted | {text_muted_hex} | Labels, captions |
| --color-danger | {danger_hex} | Errors, destructive |
| --color-success | {success_hex} | Confirmations |

### Typography
| Token | Value |
|-------|-------|
| --font-heading | {heading_font} |
| --font-body | {body_font} |
| --text-base | 16px / 1.5 |

### Spacing & Shape
| Token | Value |
|-------|-------|
| --space-unit | {base_spacing}px |
| --radius-sm | {radius_sm}px |
| --radius-md | {radius_md}px |
| --radius-lg | {radius_lg}px |

## References
{if_files_were_found_in_.factory/ux/_list_them_here_with_a_one_line_description_of_each_otherwise_write_"None — UX defined from scratch."}

## Open Questions
{deferred_decisions_as_checkboxes}
```

Also generate **`.factory/ux/theme.css`** using the token values above. If `palette.css` already exists and was provided by the developer, use it as the source of truth and do not overwrite it:

```css
/* Theme tokens — {project_name} */
/* Generated by AI Factory Phase 3. To override, edit palette.css instead. */

:root {
  /* Colors */
  --color-primary:        {primary_hex};
  --color-primary-dark:   {primary_dark_hex};
  --color-surface:        {surface_hex};
  --color-surface-raised: {surface_raised_hex};
  --color-border:         {border_hex};
  --color-text:           {text_hex};
  --color-text-muted:     {text_muted_hex};
  --color-danger:         {danger_hex};
  --color-success:        {success_hex};

  /* Typography */
  --font-heading:      {heading_font};
  --font-body:         {body_font};
  --text-base:         16px;
  --line-height-base:  1.5;

  /* Spacing & Shape */
  --space-unit:  {base_spacing}px;
  --radius-sm:   {radius_sm}px;
  --radius-md:   {radius_md}px;
  --radius-lg:   {radius_lg}px;
}
```

### Phase 3 completion message

> "Phase 3 complete. `.factory/docs/ux.md` and `.factory/ux/theme.css` written.
>
> Ready to continue to **Phase 4 — Frontend**?"

---

## PHASE 4 — Frontend (`frontend.md`)

### Questions to ask

Replace the open-ended "sketch a folder structure" question with these three targeted questions. The AI must generate a **concrete folder tree** using the answers plus the domain entities from `app.md`. Every entity placeholder must be replaced with actual names before Phase 9 runs.

1. How is code grouped at the top level? *(by feature/domain — recommended / by layer / by route)*
2. Where does shared, cross-cutting code live? *(e.g., `shared/`, `core/`, `common/`)*
3. Where does data-fetching / API-client code live relative to UI code? *(co-located inside each feature / centralized in a separate layer)*
4. File naming convention? *(PascalCase for components, camelCase for hooks, kebab-case for utils, etc.)*
5. State management approach? *(Zustand / Redux Toolkit / React Context / Signals / Jotai / none / other)*
6. How are API calls made? *(React Query / SWR / custom fetch hooks / Axios / other)*
7. Where do test files live? *(co-located next to source files / `__tests__` subfolder / separate `tests/` directory)*
8. CSS methodology? *(Tailwind / CSS Modules / styled-components / BEM / SCSS / other)*

### File to generate

Write `.factory/docs/frontend.md` — fully populated. For the Folder Structure section, generate a **concrete tree** (not a vague description) using actual domain entity names from `app.md`. The tree is the authoritative layout contract — task files in Phase 9 reference it directly when specifying which files to create.

The generated tree must enforce:
- A clear **feature boundary** — each domain entity or user-facing feature gets its own folder
- A **layer split inside each feature** — UI/presentation code separated from data-access/state code
- A **shared zone** — reusable, non-domain pieces live in one place and are never duplicated
- The exact folder and file naming convention of the chosen framework

```
# Frontend — {project_name}

## Folder Structure

{concrete_folder_tree_using_actual_domain_entity_names_from_app.md}

## File Naming Conventions
{naming_conventions_answer}

## Domain Boundaries
{domain_boundary_strategy}

## State Management
{state_management_approach}

## API Layer
{api_layer_approach}

## Test Co-location
{test_colocation_strategy}

## CSS Methodology
{css_methodology}

## Open Questions
{deferred_decisions_as_checkboxes}
```

### Phase 4 completion message

> "Phase 4 complete. `.factory/docs/frontend.md` written.
>
> Ready to continue to **Phase 5 — Backend**?"

---

## PHASE 5 — Backend (`backend.md`)

### Questions to ask

Replace the open-ended "sketch a folder structure" question with these three targeted questions. The AI must generate a **concrete folder tree** using the answers plus the domain entities from `app.md`.

1. How is code grouped at the top level? *(by domain entity — recommended / by layer)*
2. What layers exist inside each domain? *(e.g., `router → service → repository` / `controller → use-case → repository` / `handler → service → query`)*
3. Where do shared/cross-cutting concerns live? *(e.g., `common/`, `shared/`, `middleware/`)*
4. Validation library? *(Zod / Joi / class-validator / Pydantic / none / other)*
5. Error handling strategy? *(centralized error middleware / per-route try/catch / other)*
6. How is the database connection imported? *(shared module / direct import per file / singleton / other)*
7. Is there a simulation engine, background job runner, or tick loop? If yes, where does it live?
8. Seeder strategy? *(destructive drop + migrate + seed / upsert / fixture files / none)*

### File to generate

Write `.factory/docs/backend.md` — fully populated. For the Folder Structure section, generate a **concrete tree** using actual domain entity names from `app.md`. The tree is the authoritative layout contract.

The generated tree must enforce:
- One folder per domain entity — never mix two entities in one folder
- Each layer is a separate file inside that folder — no business logic in the router/controller
- A shared zone for things like error filters, guards, validation pipes, middleware
- A database folder (migrations, seed, connection config) separate from domain code

```
# Backend — {project_name}

## Folder Structure

{concrete_folder_tree_using_actual_domain_entity_names_from_app.md}

## Layer Architecture
{router_service_repo_description}

## Validation
{validation_library_and_pattern}

## Error Handling
{error_handling_strategy}

## Database Connection
{db_connection_import_pattern}

## Simulation / Background Jobs
{simulation_engine_description_or_none}

## Seeder Strategy
{seeder_strategy}

## Open Questions
{deferred_decisions_as_checkboxes}
```

### Phase 5 completion message

> "Phase 5 complete. `.factory/docs/backend.md` written.
>
> Ready to continue to **Phase 6 — Database**?"

---

## PHASE 6 — Database (`database.md`)

### Questions to ask

1. List all entities and their relationships. *(e.g., "User has many Projects. Project has many Tasks. Task belongs to Project and to User.")*
2. Migration strategy? *(Drizzle Kit / Prisma Migrate / Alembic / Rails migrations / manual SQL / other)*
3. Soft deletes or hard deletes?
4. Do you need an audit trail or event log table separate from normal data?
5. Primary key type? *(UUID / serial integer / CUID / other)*
6. Any JSONB or JSON columns for flexible or unstructured payloads?

### File to generate

Write `.factory/docs/database.md`:

```
# Database — {project_name}

## Entity List

{entities_and_relationships_as_plain_language_list}

## Schema Overview

{table_by_table_description_with_key_columns_and_foreign_keys}

## Migration Strategy
{migration_strategy}

## Delete Strategy
{soft_or_hard_delete}

## Audit Trail
{audit_trail_description_or_none}

## Primary Keys
{pk_type}

## JSON Columns
{json_columns_description_or_none}

## Open Questions
{deferred_decisions_as_checkboxes}
```

### Phase 6 completion message

> "Phase 6 complete. `.factory/docs/database.md` written.
>
> Ready to continue to **Phase 7 — Discovery**?"

---

## PHASE 7 — Discovery

> These four questions lock the project scope before skills or tasks are generated. Answering them prevents auth pivots, schema redesigns, and architectural surprises mid-project.

### Questions to ask

1. **User/session model** — Single user (no login needed), multi-user (accounts required), or no-auth (anyone can use, no accounts)?

2. **Data model scope** — Single record per session (one active state per user), multiple saves per user (named saves or history), or multi-tenant (many organizations with isolated data)?

3. **Interaction model** — Real-time server tick (auto-updates on a timer), turn-based (user triggers each update), event-driven (webhooks or queues), or CRUD-only (no live updates)?

4. **Persistence scope** — Cloud-hosted database, local SQLite file, or ephemeral (in-memory, resets on restart)?

### After answering

Append this line to `.factory/logs/factory.log`:
```
[{today}] FACTORY-DISCOVERY | STATUS: done | User: {answer_1} | Data: {answer_2} | Interaction: {answer_3} | Persistence: {answer_4}
```

Update `.factory/status.md`:
- Change `Current: **FILL DOCS**` to `Current: **SKILLS**`
- Update the Last Note field

### Phase 7 completion message

> "Phase 7 complete. Discovery answers recorded in `factory.log`.
>
> Ready to continue to **Phase 8 — Skills**? *(No questions this phase — I'll read all the docs and generate the skill files automatically.)*"

---

## PHASE 8 — Skills

> No questions. Read all completed docs and generate every skill file and the context file.

### Instructions for the AI

1. Read all six files in `.factory/docs/`
2. Read `.factory/logs/factory.log` for the discovery answers
3. Use the context file name for the tool identified in Phase 0
4. Use the auto-discovery location for the tool identified in Phase 0 (see Skill Auto-Discovery Location table in AI INSTRUCTIONS)
5. Generate every file below using real commands from `architecture.md` — no placeholders anywhere
6. Write canonical skill files to `.factory/skills/` first, then write tool-specific copies to the auto-discovery location

### Two-Location Model

For every skill file:
1. **Write the canonical file** to `.factory/skills/factory-<name>.md` — always, all tools
2. **Write the tool-specific copy** to the auto-discovery location — format per the tool table in AI INSTRUCTIONS

For Cursor, prepend this front matter to each `.mdc` file:
```
---
description: {one_line_skill_description}
alwaysApply: false
---
```

For GitHub Copilot, append an inline instructions block to `.github/copilot-instructions.md` for each skill.

For Aider and Unknown tools, write `.factory/skills/` only — no auto-discovery copy.

### Files to generate

**1. The context file** (name from the tool table — e.g., `CLAUDE.md`)

Fully populate every section:

```
# {project_name} — Project Context

This file is loaded by your AI assistant each session.

## Project
{one_paragraph_from_app.md_vision_and_target_user}

## AI Factory Method
This project uses the AI Factory Method:
- Docs define context and decisions (.factory/docs/)
- Tasks are numbered, discrete, testable units (.factory/tasks/)
- A factory log tracks every completed order (.factory/logs/factory.log)
- Skills automate repetitive factory operations (.factory/skills/)

## Active Docs

| Domain | File |
|--------|------|
| Product | .factory/docs/app.md — read first |
| Architecture | .factory/docs/architecture.md |
| UX | .factory/docs/ux.md |
| Frontend | .factory/docs/frontend.md |
| Backend | .factory/docs/backend.md |
| Database | .factory/docs/database.md |

## UX References

| File | Description |
|------|-------------|
{list_every_file_found_in_.factory/ux/_with_one_line_description_or_write_"None"}

## Core Development Rule — Domain-Focused Code

All code must be named after domain concepts from app.md:
{list_domain_entities_from_app.md_domain_model}

No generic names: no DataService, ItemList, handleSubmit, processData.
Business logic lives in domain services, not in route handlers or components.
This rule applies from task 001 onwards and is never relaxed.

## AI Autonomy

The AI may execute the following without asking for approval:

### Test & Build
| Command | When to use |
|---------|------------|
{populate_with_real_test_and_build_commands_from_architecture.md}

### Infrastructure (if Docker is in the stack)
{populate_with_real_docker_commands_and_compose_file_path}

### File Operations
Read and write any file within the workspace: source code, docs, task files, status.md, factory.log.

### Requires Developer Approval
- Installing or removing packages
- Destructive git operations (reset --hard, force push, branch delete)
- Running seed scripts against a non-empty database
- Any command not listed above

## Task Rules
- Tasks live in .factory/tasks/task-NNN.md
- Execute one task at a time
- Do not start a task without reading its file
- Mark completion in status.md and append factory.log

## Task Execution Cycle

  1. Write code
  2. Run unit tests
  3. Review output — fix any failures
  4. Run tests again — must be green
  5. Run build — must succeed
  6. Run lint — must pass

A task is not complete until tests are green AND the build passes AND lint is clean.

Loop limit: If fix → retest repeats more than 3 times without resolving, stop immediately.
Report what failed, what was tried, and why it is not resolving. Never loop silently.

## Factory Log Rule
Every completed task must produce a log entry:
  [YYYY-MM-DD] TASK-NNN | STATUS: done | <one-line summary>

## Status Dashboard
Current state: status.md
```

**2. `.factory/skills/factory-next-task.md`** (+ tool-specific copy)

A skill prompt that:
- Reads `.factory/status.md` to identify the current task number
- Reads `.factory/tasks/task-NNN.md` for that task
- Reads every doc listed in the task's Context section
- Presents a structured brief: task number, objective, files in scope, acceptance criteria, docs consulted
- Asks "Ready to implement — shall I begin?"
- On confirmation: writes code → runs tests using the actual commands from `architecture.md` → fixes up to 3 times → runs build → runs lint → appends `factory.log` → updates `status.md`
- After automated checks pass, prints: "Automated cycle complete. See the Developer Review checklist in task-NNN.md if you want to do a manual spot-check before moving on."

All test, build, and lint references must use the real commands from `architecture.md`, not generic placeholders.

**3. `.factory/skills/factory-run-tasks.md`** (+ tool-specific copy)

A skill prompt that accepts a range or list of task numbers: `factory-run-tasks 001-003` or `factory-run-tasks 002 004 007`.

Skill behavior:
1. Reads each task file in the specified order.
2. Verifies dependencies of each task are marked complete in `status.md` before starting. If a dependency is not complete, stops and reports the gap — does not skip ahead.
3. Executes the full task execution cycle (code → tests → build → lint → log → status update) for each task in sequence.
4. Stops and surfaces the failure immediately if any task's tests, build, or lint do not pass after 3 fix attempts — does not proceed to the next task.
5. After each task's automated cycle completes, prints: "Task NNN complete. See Developer Review checklist in task-NNN.md. Continue to task MMM? (yes/stop)"
6. Prints a summary table when all tasks in the batch complete or when it stops early.

All test, build, and lint references must use the real commands from `architecture.md`.

**4. `.factory/skills/factory-log-progress.md`** (+ tool-specific copy)

A skill prompt that:
- Appends one line to `.factory/logs/factory.log` in this format: `[YYYY-MM-DD] TASK-NNN | STATUS: done | <summary>`
- Updates `.factory/status.md`: advances current task number, moves task row to Completed table, removes from Pending table, updates Last Note

**5. `.factory/skills/factory-manage-tasks.md`** (+ tool-specific copy)

A skill prompt that adds, deletes, or modifies tasks during development. Invoked as `factory-manage-tasks add [title]`, `factory-manage-tasks delete NNN`, or `factory-manage-tasks modify NNN`.

- **add** — asks for objective, relevant docs, scope, acceptance criteria, dependencies, and queue position; creates the task file; adds it to `status.md` Pending Tasks
- **delete** — reads the task, confirms, deletes the file, removes the row from `status.md` Pending Tasks; refuses to delete completed tasks
- **modify** — reads the task, shows current content, asks which sections to change, applies only those changes; updates `status.md` if the title changed

**6. `.factory/skills/factory-customize.md`** (+ tool-specific copy)

A skill prompt that modifies the factory itself during development. Before making any change it reads `.factory/FACTORY.md` File Organization to enforce naming conventions and correct file placement. Handles four categories:

- **doc** — add, update, or remove a factory doc; propagates to the Active Docs table in the context file, the Context checklist in `task-TEMPLATE.md`, and the Directory Tree in `FACTORY.md`
- **rule** — change factory-wide rules: loop limit, domain naming convention, task execution cycle steps; mirrors changes across `FACTORY.md`
- **autonomy** — add or remove commands the AI may run without asking; manage the "requires approval" list
- **skill** — add a new skill or update an existing one; writes canonical to `.factory/skills/` and tool-specific copy to auto-discovery location; updates the Directory Tree in `FACTORY.md`

**7. `.factory/skills/factory-extract-template.md`** (+ tool-specific copy)

A skill prompt that reads `.factory/FACTORY.md`, the context file, `factory.log`, all six docs, and all task files, then generates a new `FACTORY-TEMPLATE.md` in the project root — stripped of all app-specific content, preserving any process lessons found in `factory.log` as appendix notes.

**8. Stack-specific skills** — generate only what the stack in `architecture.md` requires. Write tool-specific copies for all of these as well:

| Skill file | Generate when |
|------------|--------------|
| `.factory/skills/factory-run-tests.md` | Always |
| `.factory/skills/factory-run-build.md` | Always |
| `.factory/skills/factory-run-lint.md` | A linting tool is configured |
| `.factory/skills/factory-db-migrate.md` | A migration tool is in the stack |
| `.factory/skills/factory-db-seed.md` | A seed strategy is defined |
| `.factory/skills/factory-db-reset.md` | A destructive drop + migrate + seed workflow is described |
| `.factory/skills/factory-docker-start.md` | Docker is used for local development |
| `.factory/skills/factory-docker-logs.md` | Docker is used for local development |

Each skill file must contain:
- The exact CLI invocation with real paths and flags — no placeholders
- What "success" looks like (expected output)
- What to do if it fails

### After generating Phase 8 files

Update `.factory/status.md`:
- Change `Current: **SKILLS**` to `Current: **TASKS**`
- Update Last Note

### Phase 8 completion message

> "Phase 8 complete. Context file and {N} skill files written — canonical in `.factory/skills/`, tool-specific copies in `{auto_discovery_location}`.
>
> Ready to continue to **Phase 9 — Tasks**? *(No questions — I'll read the docs and generate all task files automatically.)*"

---

## PHASE 9 — Tasks

> No questions. Read all completed docs and generate the full numbered task list.

### Instructions for the AI

1. Read `.factory/docs/app.md` first — the Core Features list drives which tasks must exist
2. Read all remaining docs in `.factory/docs/`
3. Read the discovery answers from `.factory/logs/factory.log`
4. Apply every rule below when generating the task list

### Task generation rules

- Every feature in `app.md` Core Features must have at least one task
- **Foundation tasks come first** — generate one or two foundation tasks before the first feature task (see Foundation Task Rules below)
- Order tasks by dependency: infrastructure → data layer → backend foundation → backend API → frontend foundation → feature pages → production readiness
- Each task must be completable and testable in a single session
- No task should mix more than one major concern — do not combine schema + API, or API + frontend page, in one task
- Every task must include "unit tests pass", "build succeeds", and "lint passes" as acceptance criteria
- Name tasks after domain concepts from `app.md`, not technical layers *(e.g., "Player Stats API", not "REST Endpoints")*
- Do not generate tasks for anything listed in `app.md` Out of Scope
- **Features are developed end-to-end** — every feature task must include empty state, error state, loading state, and animation acceptance criteria sections (see Feature Task Template below). Do not generate a separate "polish" task for these concerns — they are part of every feature task from the start
- **README is a living document** — the frontend foundation task creates the README skeleton; subsequent tasks update it inline (see README Rule below)

### Foundation Task Rules

Phase 9 must generate foundation tasks numbered **before** the first feature task. These wire up everything subsequent features depend on.

**Backend foundation task must include:**
- Global exception filter (or equivalent error middleware) registered at the app entry point
- Standardized API error response shape (e.g., `{ statusCode, message, error }`)
- Request-logging middleware
- Database connection health-check endpoint (`GET /health`)

**Frontend foundation task must include:**
- Global HTTP error interceptor registered in app config
- `EmptyStateComponent` in `shared/components/` — reusable, slot-based
- `SkeletonCardComponent` in `shared/components/` — reusable
- `AnimationService` (or equivalent utility) encapsulating the app's animation primitives and `prefers-reduced-motion` guard — using the animation library from `ux.md`
- CSS custom property tokens (`theme.css` from `.factory/ux/`) applied globally in the app stylesheet
- Toast / notification service for surfacing error messages from the interceptor
- `README.md` skeleton at the project root (all sections present, stubs marked `<!-- TODO -->`)

**Rule:** No feature task may be numbered until all foundation tasks are defined. Feature task files must reference shared components (EmptyState, Skeleton, AnimationService) by their exact import path so they are used from the start, not bolted on later. If a foundation component does not exist yet, the feature task must list the foundation task as a dependency — it must never skip those acceptance criteria.

### Feature Task Acceptance Criteria Template

Every generated **feature task** (pages, data-fetching, state management) must include the following sections as a mandatory template — not optional extras. A task is not complete until all five state sections pass.

```
## Acceptance Criteria

### Functional (happy path)
- [ ] {feature-specific criterion 1}
- [ ] {feature-specific criterion 2}

### Empty state
- [ ] {ComponentName} shows `<EmptyStateComponent>` with a contextual message when the list / query returns zero results

### Error state
- [ ] API failure surfaces a visible error message via the global interceptor / toast service; the component does not silently show a blank screen

### Loading state
- [ ] Data-fetching views show `<SkeletonCardComponent>` (or equivalent) while the HTTP request is in flight

### Animation
- [ ] Entrance animation is applied using `AnimationService` consistent with ux.md; animation is skipped when `prefers-reduced-motion` is active

### Technical
- [ ] Unit tests pass
- [ ] Build succeeds
- [ ] Lint passes
```

### README Rule

The frontend foundation task must create `README.md` at the project root with this skeleton — all sections present, stubs marked for later:

```markdown
# {project_name}

> {one_line_description_from_app.md}

## Prerequisites
<!-- TODO: fill in after architecture is confirmed -->

## Quick Start
<!-- TODO: fill in after first runnable task -->

## Running Locally
<!-- TODO -->

## Commands Reference
<!-- TODO -->

## Architecture Overview
<!-- TODO -->

## Environment Variables
<!-- TODO -->
```

Every subsequent task file must include a **README** row in its Scope table whenever it touches something a developer would need to know to run, configure, or understand the project:

| Action | File |
|--------|------|
| Update | README.md — {section(s) affected by this task} |

Specifically:
- Foundation task → fill Prerequisites, Quick Start, Environment Variables
- First runnable backend task → fill Running Locally (API + Docker)
- First runnable frontend task → fill Running Locally (web dev server)
- Each feature task → add a one-liner to an Architecture Overview or Features section
- Any task that adds a new CLI command, env var, or migration step → update the relevant README section immediately

**Rule:** Updating `README.md` is part of every task's Definition of Done when that task changes something a developer needs to know. The final task only verifies accuracy — it must never be the task that *writes* the README.

### Typical task order for a full-stack web app

| Execution phase | Category | Example tasks |
|-----------------|----------|---------------|
| SCAFFOLD | Infrastructure | Monorepo init, Docker setup, env config |
| SCAFFOLD | Data layer | DB schema migration, seed fixtures |
| CORE | Backend foundation | Global exception filter, error shape, logging middleware, health check |
| CORE | Backend | API foundation, one endpoint group per domain entity |
| CORE | Frontend foundation | App shell, routing, shared infrastructure (interceptor, EmptyState, Skeleton, AnimationService, toast), theme tokens, README skeleton |
| FEATURES | Pages | One task per core screen in `ux.md` — each with empty/error/loading/animation criteria |
| FEATURES | State & data | Stores, service layer, data-fetching hooks |
| POLISH | Production | Error boundaries, lazy loading, README final review |

### File structure for each task

Write `.factory/tasks/task-NNN.md` for every task.

**For infrastructure and foundation tasks:**

```
# Task {NNN} — {title}

## Objective
{one_paragraph_describing_what_to_build_or_change}

## Context

- [ ] Read .factory/docs/app.md
{additional_relevant_docs_for_this_task}

## Scope

| Action | File |
|--------|------|
{files_to_create_or_modify}

## Acceptance Criteria

- [ ] {criterion_1}
- [ ] {criterion_2}
- [ ] Unit tests pass
- [ ] Build succeeds
- [ ] Lint passes

## Dependencies
{prerequisite_task_numbers_or_none}

## Developer Review (optional — do after the AI cycle completes)
- [ ] Open the app / endpoint and verify the feature works end-to-end
- [ ] {task-specific spot-check if obvious from the objective}

## Notes
_Fill in after execution._
```

**For feature tasks (pages, data-fetching, state management):**

```
# Task {NNN} — {title}

## Objective
{one_paragraph_describing_what_to_build_or_change}

## Context

- [ ] Read .factory/docs/app.md
- [ ] Read .factory/docs/ux.md
{additional_relevant_docs_for_this_task}

## Scope

| Action | File |
|--------|------|
{files_to_create_or_modify}
| Update | README.md — {section_affected_by_this_task_if_applicable} |

## Acceptance Criteria

### Functional (happy path)
- [ ] {feature-specific_criterion_1}
- [ ] {feature-specific_criterion_2}

### Empty state
- [ ] {ComponentName} shows `<EmptyStateComponent>` with a contextual message when the list / query returns zero results

### Error state
- [ ] API failure surfaces a visible error message via the global interceptor / toast service; the component does not silently show a blank screen

### Loading state
- [ ] Data-fetching views show `<SkeletonCardComponent>` (or equivalent) while the HTTP request is in flight

### Animation
- [ ] Entrance animation is applied using `AnimationService` consistent with ux.md; animation is skipped when `prefers-reduced-motion` is active

### Technical
- [ ] Unit tests pass
- [ ] Build succeeds
- [ ] Lint passes

## Dependencies
{prerequisite_task_numbers — foundation task is always a dependency for the first feature task}

## Developer Review (optional — do after the AI cycle completes)
- [ ] Open the page in the browser and verify the feature works end-to-end
- [ ] Check for visual regressions on affected screens
- [ ] Confirm no unintended side-effects on adjacent features
- [ ] {task-specific spot-check if obvious from the objective}

## Notes
_Fill in after execution._
```

### After generating all task files

Update `.factory/status.md`:
- Change `Current: **TASKS**` to `Current: **EXECUTE**`
- Populate the Pending Tasks table with every generated task
- Update Last Note

Append to `.factory/logs/factory.log`:
```
[{today}] FACTORY-TASKS | STATUS: done | Generated {N} tasks — ready to execute
```

---

## COMPLETION

After Phase 9, output this summary (filled in with actual values):

```
## Factory Setup Complete

### Files Generated

| File | Description |
|------|-------------|
| .factory/logs/factory.log | Append-only order log, discovery answers recorded |
| .factory/status.md | Project state dashboard |
| .factory/FACTORY.md | Factory process reference |
| .factory/tasks/task-TEMPLATE.md | Blank task schema |
| .factory/docs/app.md | Product vision, domain model, feature scope |
| .factory/docs/architecture.md | Stack and infrastructure decisions |
| .factory/docs/ux.md | Design language, core screens, and theme tokens |
| .factory/docs/frontend.md | Frontend conventions and folder structure |
| .factory/docs/backend.md | Backend conventions and layer architecture |
| .factory/docs/database.md | Schema, migrations, delete strategy |
| .factory/ux/theme.css | CSS custom property token stub |
| {context_file} | AI context file (auto-loaded every session) |
| .factory/skills/factory-next-task.md | Execute the next task |
| .factory/skills/factory-run-tasks.md | Execute a range or list of tasks |
| .factory/skills/factory-log-progress.md | Log a completed task |
| .factory/skills/factory-manage-tasks.md | Add, delete, or modify tasks |
| .factory/skills/factory-customize.md | Modify the factory structure |
| .factory/skills/factory-extract-template.md | Extract reusable template at project end |
{additional_stack_skill_rows}
{tool_specific_copies_note}
{task_files_summary_row}

### Next Step

Use the factory-next-task skill (or paste .factory/skills/factory-next-task.md into your AI)
to begin executing Task 001.
```

---

## Appendix — Process Notes

These lessons were captured from real factory runs. The AI should read them before generating tasks in Phase 9.

### PROCESS NOTE 1 — Discovery before task generation

Answering the four discovery questions (Phase 7) before generating tasks (Phase 9) prevents auth tasks from appearing mid-project, prevents schema redesigns after the API is built, and prevents polling/WebSocket architectural pivots after the frontend is wired. The four questions are the minimum viable discovery set — do not skip Phase 7.

### PROCESS NOTE 2 — Task execution cycle and loop limit

The task execution cycle must be enforced in both the AI context file (read every session) and `.factory/FACTORY.md` (human reference). Having it in only one place causes it to be missed. The loop limit — stop after 3 failed fix attempts and escalate — is not a suggestion. Silent looping is the most common failure mode in AI-assisted development.

### PROCESS NOTE 3 — Skills vs commands: canonical source in `.factory/skills/`

The original template used `.factory/commands/` and described the files as "command prompts." In practice, these are skills (slash commands) in Claude Code and rules in Cursor/Windsurf. Storing them only in a tool-specific location breaks portability when the team switches AI tools.

The two-location model: `.factory/skills/` is the canonical human-readable source written first; the tool-specific auto-discovery copy is a derived artifact written second in Phase 8. If the AI tool changes, re-run Phase 8 to regenerate the auto-discovery copies without touching the canonical source. All skill files use the `factory-` prefix to avoid collisions with project-specific slash commands.

### PROCESS NOTE 4 — Multi-task execution with `factory-run-tasks`

The `factory-next-task` skill executes one task per invocation. For unblocked sequential tasks (e.g., scaffold and infrastructure tasks with no interactive decisions), requiring a manual re-invocation after each task creates friction. The `factory-run-tasks` skill solves this by accepting a range (`001-004`) or a list (`002 005 007`). It still verifies dependencies before each task and stops on the first failure — it does not silently skip failures or proceed past a blocked task. Use it for mechanical scaffold and data-layer tasks; use `factory-next-task` for feature tasks where developer judgment is needed between steps.

### PROCESS NOTE 5 — Developer review checklist after automated cycle

The task execution cycle ends at "lint passes." Without an explicit prompt, the developer has no reminder to do a visual check, browser smoke test, or API spot-check. Visual regressions and UX issues found late — after several related features have shipped on top — are expensive to fix. Adding an optional Developer Review section to every task file and printing a reminder after the automated cycle costs nothing and catches issues early.

### PROCESS NOTE 6 — Frontend and backend folder structure: targeted questions yield concrete trees

Asking "sketch the top-level folder structure" produces vague layouts that don't enforce layer separation. By Phase 9, when task files need to specify exact file paths, the vague layout causes inconsistency and drift between what was planned and what was built.

Replace the open-ended question with three targeted questions (grouping strategy, shared zone, data-fetching co-location) and require the AI to generate a concrete tree with actual domain entity names. The tree becomes the authoritative layout contract that task files reference directly when specifying which files to create.

### PROCESS NOTE 7 — UX phase must define the full theme before scaffold tasks run

Leaving "Color & Typography: to be defined during scaffold" creates a gap: scaffold and foundation tasks have no color or typography contract, so they either invent their own tokens or leave output unstyled. By the time the theme is actually defined, the gap has propagated across multiple tasks and retrofitting is expensive.

Define the full palette, typography, and spacing tokens in Phase 3 and generate a `theme.css` stub. The frontend foundation task applies it globally. All subsequent tasks inherit the theme from the start.

### PROCESS NOTE 8 — Foundation tasks must precede all feature tasks

Tasks 015 and 016 in one real project illustrate the anti-pattern: an HTTP error interceptor, loading skeletons, empty-state components, an animation service, and a `prefers-reduced-motion` guard were all written *after* a dozen feature tasks had already shipped. Every earlier feature task silently skipped error states, loading feedback, and animation — producing shippable-looking but incomplete features. Retrofitting was expensive and the "polish" task became a large, risky batch of unrelated changes.

The fix: generate one or two foundation tasks numbered before the first feature task. These wire up everything subsequent features depend on. Feature task files reference shared components by exact import path so they are used from the start, not bolted on later.

### PROCESS NOTE 9 — Features are developed end-to-end; polish is not a separate task

Saving empty states, error states, loading feedback, and entrance animations for a dedicated "polish" pass at the end produces a long queue of half-finished features and a large, hard-to-review commit. The pattern also makes it easy to never get to the polish pass at all if the project runs short on time.

Each feature task must include explicit acceptance criteria sections for empty state, error state, loading state, and animation — these are required sections that must pass before the task is marked complete, not optional extras to add later. If a foundation component (EmptyState, Skeleton, AnimationService) does not exist yet, the feature task must list the foundation task as a dependency rather than silently skipping those acceptance criteria.

### PROCESS NOTE 10 — README is a living document, created in the foundation task

Writing a README from scratch as the last task requires reconstructing every setup step, command, and architectural decision from memory or by re-reading the whole codebase. The result is a one-shot, hard-to-review commit. It also means every earlier task ships without any developer-facing documentation.

The fix: the frontend foundation task creates the README skeleton with all sections stubbed. Each subsequent task updates its relevant sections inline as it runs. Any task that adds an env var, Docker step, CLI command, or migration step without updating the README is not complete. The final task only verifies accuracy — it never writes the README from scratch.

### PROCESS NOTE 11 — Signal-based state: in-place mutation, computed derivations, nonNullable reactive forms

When using signal-based or reactive state management: prefer in-place signal updates using side-effect operators (e.g., `tap()`) rather than triggering a full data reload on every mutation — this avoids unnecessary round-trips and keeps the UI responsive. Use `computed()` or derived signals for filtered and transformed views rather than duplicating raw state. Configure reactive form groups with strict non-null types (e.g., `fb.nonNullable.group()`) to prevent runtime null issues in TypeScript strict mode. Detect edit vs. create mode in form pages from route parameters rather than creating separate page components for each mode — this keeps the task scope focused and avoids duplicating form logic.

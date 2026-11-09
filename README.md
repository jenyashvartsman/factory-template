# AI Factory Template

A setup guide for the **AI Factory Method** — a structured workflow for building software with AI assistance.

## What It Is

Drop `FACTORY-TEMPLATE.md` into any new project root and paste its contents into your AI assistant. The AI will interview you phase by phase and write every factory file to disk.

## How It Works

The setup runs through 10 phases:

| Phase | Name | Output |
|-------|------|--------|
| 0 | Bootstrap | Scaffold dirs, stub files, log |
| 1 | App Design | `.factory/docs/app.md` |
| 2 | Architecture | `.factory/docs/architecture.md` |
| 3 | UX | `.factory/docs/ux.md` + `theme.css` |
| 4 | Frontend | `.factory/docs/frontend.md` |
| 5 | Backend | `.factory/docs/backend.md` |
| 6 | Database | `.factory/docs/database.md` |
| 7 | Discovery | Locks scope decisions |
| 8 | Skills | Context file + skill prompts |
| 9 | Tasks | Numbered task files |

## Supported AI Tools

Claude Code, Cursor, GitHub Copilot, Windsurf, Aider, and others.

## Usage

1. Copy `FACTORY-TEMPLATE.md` to your new project root
2. Open the file in your AI assistant (Claude Code, Cursor, etc.)
3. The AI will start Phase 0 automatically — just answer the questions

## What Gets Generated

```
<project-root>/
├── CLAUDE.md (or .cursorrules, etc.)   ← AI context file
├── .factory/
│   ├── docs/          ← Design documents
│   ├── tasks/         ← Numbered task files
│   ├── skills/        ← Reusable skill prompts
│   ├── logs/          ← Append-only factory log
│   └── ux/            ← Theme tokens and UX assets
```

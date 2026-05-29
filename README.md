# agents-workflow

Claude Code configuration for a multi-agent development workflow built on Laravel 12 + Inertia.js + Vue 3.

## What This Is

This repo stores the `.claude/` configuration that drives automated, agent-assisted development. Instead of a single AI assistant doing everything, work is split across specialized agents that collaborate through a defined pipeline.

## Agents

| Agent       | Model  | Role |
|-------------|--------|------|
| `ba`        | Opus   | Business Analyst — requirements, user stories, phased implementation plans |
| `devil`     | Opus   | Devil's Advocate — read-only critic of BA plans |
| `developer` | Sonnet | Full-stack implementer — Laravel Actions, Form Requests, Vue/Inertia pages |
| `tester`    | Sonnet | Testing specialist — Pest PHP, feature/unit/mutation tests, TDD |

## Skills

| Skill                     | Purpose |
|---------------------------|---------|
| `agent-orchestration`     | Multi-agent pipeline for non-trivial features |
| `architecture-designer`   | System architecture decisions |
| `brainstorming`           | Explore approaches before committing |
| `git-commit-messages`     | Conventional Commits style |
| `inertia-vue-development` | Inertia v3 + Vue 3 pages, forms, navigation |
| `laravel-architecture`    | Laravel system-design guidance |
| `laravel-best-practices`  | Idiomatic Laravel patterns, performance, security |
| `phpunit-testing`         | Pest PHP tests |
| `plan-writing`            | Structured plans with dependencies & verification |
| `tailwindcss-development` | Tailwind CSS utilities and patterns |
| `ziggy-development`       | Laravel routes for the frontend via Ziggy |

## Tech Stack

| Layer    | Technology |
|----------|-----------|
| Backend  | Laravel 12, PHP 8.4 |
| Frontend | Vue 3 (Composition API), TypeScript |
| Bridge   | Inertia.js v3 |
| Routing  | Ziggy |
| Styling  | Tailwind CSS |
| Runtime  | Docker via Laravel Sail |

## Workflow

1. **BA agent** analyzes requirements → writes plan to `docs/`
2. **Devil agent** critiques the plan via `SendMessage`
3. **Developer agent** implements backend + frontend
4. **Tester agent** writes Pest tests

See `agent-orchestration` skill for the full pipeline.

## Key Rules

- All commands run via Laravel Sail (`./vendor/bin/sail ...`)
- Strict PHP (`declare(strict_types=1)`, full type hints, PHP 8.4)
- Plans written to `docs/YYYY-MM-DD-feature-name.md` before any implementation
- Migrations, models, controllers created via `artisan make:*` — never manually

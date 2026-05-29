## IMPORTANT

1. Before writing any code, describe your approach and wait for approval.
2. If requirements are ambiguous, ask clarifying questions before writing code.
3. After finishing code, list edge cases and suggest test cases.
4. If a task requires changes to more than 3 files, stop and break it into smaller tasks.
5. When there's a bug, start by writing a test that reproduces it, then fix it.
6. Every time I correct you, reflect on what went wrong and plan to prevent it.
7. Always use artisan commands to create migrations, controllers, models, etc. Do not create these kind of files manually.

## Core Principles

- **Simplicity First**: Make every change as simple as possible. Impact minimal code.
- **No Laziness**: Find root causes. No temporary fixes. Senior developer standards.
- **Minimal Impact**: Changes should only touch what's necessary. Avoid introducing bugs.

## Task Management

1. **Plan First**: Write plan to `docs/[YYYY-MM-DD]-[feature-name].md` with checkable items
2. **Verify Plan**: Check in before starting implementation
3. **Track Progress**: Mark items complete as you go
4. **Explain Changes**: High-level summary at each step
5. **Document Results**: Add review section to `docs/[YYYY-MM-DD]-[feature-name].md`
6. **Capture Lessons**: Update `docs/lessons.md` after corrections


## Agents

| Agent       | Model  | Role                                                                                           |
|-------------|--------|------------------------------------------------------------------------------------------------|
| `ba`        | opus   | Business Analyst — requirements, user stories, phased plan in docs/.                           |
| `devil`     | opus   | Devil's Advocate — read-only critic of `ba`'s plan via SendMessage.                            |
| `developer` | sonnet | Full-stack Laravel + Inertia.js implementer (Controllers, Services, Form Requests, Vue pages). |
| `tester`    | sonnet | Pest PHP testing specialist — feature, unit, mutation testing, TDD.                            |

See the `agent-orchestration` skill for the orchestration pipeline.

## Skills

| Skill                     | Purpose                                                        |
|---------------------------|----------------------------------------------------------------|
| `agent-orchestration`     | Multi-agent pipeline for non-trivial development work.         |
| `architecture-designer`   | System architecture and design decisions.                      |
| `brainstorming`           | Explore multiple approaches before committing to one.          |
| `fortify-development`     | Auth flows (login, register, 2FA, password reset) via Fortify. |
| `git-commit-messages`     | Conventional Commits style for this project's commit messages. |
| `inertia-vue-development` | Inertia v3 + Vue 3 pages, forms, navigation, deferred props.   |
| `laravel-architecture`    | Laravel system-design and structural guidance.                 |
| `laravel-best-practices`  | Idiomatic Laravel patterns, performance, security, review.     |
| `phpunit-testing`         | Pest PHP tests — feature, unit, browser, arch.                 |
| `plan-writing`            | Structured task plans with dependencies & verification.        |
| `tailwindcss-development` | Tailwind CSS utility usage and patterns.                       |
| `ziggy-development`       | Laravel routes for the frontend via Ziggy.                     |
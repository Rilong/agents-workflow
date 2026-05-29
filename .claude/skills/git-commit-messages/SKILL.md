---
name: git-commit-messages
description: "Write git commit messages following the Conventional Commits v1.0.0 spec in this project's house style. Invoke whenever composing, drafting, rewording, or reviewing a commit message — including when the user asks to commit, says 'commit this', wants a message for staged changes, or asks to amend/reword a commit. Enforces the `type(scope): description` format, the spec rules, breaking-change handling, scope conventions, and the project's hard rules (one line, no Co-Authored-By, no AI mentions, never auto-commit). Trigger — EN: commit message, commit this, write a commit, message for these changes, reword commit, amend message. Українською: повідомлення коміту, закомить, напиши коміт, повідомлення для змін, переписати коміт, відредагувати повідомлення коміту."
---

# Git Commit Messages

This project's house style keeps every commit to a **single line** (the spec's optional body and footers are not used, except for breaking changes — see below).

## Structure (per spec)

```
<type>[optional scope][optional !]: <description>
```

The full spec also allows an optional body and footers one blank line apart, but in this project default to the subject line only.

## Spec Rules (the ones that apply here)

1. A commit MUST be prefixed with a **type** — a noun such as `feat` or `fix` — followed by an optional scope, an optional `!`, and a REQUIRED `: ` (colon + space).
2. `feat` MUST be used when adding a new feature; `fix` MUST be used for a bug fix.
3. A scope MAY follow the type; it MUST be a noun in parentheses describing a section of the codebase, e.g. `fix(parser):`.
4. A **description** MUST immediately follow the `: ` — a short summary of the change.
5. Other types beyond `feat`/`fix` MAY be used (see table).
6. Units are case-insensitive **except** `BREAKING CHANGE`, which MUST be uppercase.

## Type

| Type       | Use for                                                       |
|------------|--------------------------------------------------------------|
| `feat`     | A new feature or user-facing capability                       |
| `fix`      | A bug fix                                                     |
| `refactor` | Code change that neither fixes a bug nor adds a feature       |
| `perf`     | A change that improves performance                            |
| `style`    | Formatting, whitespace, CSS/visual styling — no logic change  |
| `docs`     | Documentation, plan files in `docs/`, checklists              |
| `test`     | Adding or updating tests                                      |
| `build`    | Build system or dependency changes (Composer, npm, Vite)      |
| `ci`       | CI configuration and pipelines                                |
| `chore`    | Maintenance with no src or test change                        |
| `revert`   | Reverts a previous commit                                     |

Pick the type that matches the *primary* intent of the change.

## Scope

A scope is a noun in parentheses naming what changed. Two valid styles, matching what the codebase already uses:

- **PascalCase component / class name** for a specific unit — e.g. `ProductItem`, `Checkout`, `InstallmentSelection`, `MonoInstallmentService`.
- **lowercase feature domain** for cross-cutting work — e.g. `installments`, `orders`.

Multiple related scopes may be comma-separated: `feat(PaymentForm, SwitcherForms): ...`.

## Description

- Start lowercase, write in the imperative ("add", "fix", "redesign", "update", "extract", "correct").
- No trailing period.
- Keep it concise — aim for ≤ 72 characters where practical.
- A comma-joined clause is fine for two related parts: `refactor(Installment): extract CreateInstallmentOrderAction, narrow return type to string`.

## Breaking Changes

Per spec, a breaking change MUST be flagged. To stay one-line, **prefer the `!` indicator** before the colon:

```
feat(api)!: drop support for legacy installment payload
```

If a fuller explanation is unavoidable, the spec's footer form is the only case where a body/footer is allowed (`BREAKING CHANGE:` must be uppercase):

```
feat(api)!: drop support for legacy installment payload

BREAKING CHANGE: the v1 callback schema is no longer accepted
```

## Examples (from this repo)

```
feat(ProductItem): add image preview button with hover overlay
fix(Product): handle null selectedCategory in template rendering
refactor(orders): fix namespace reference for InstallmentOrdersRelationManager
style(RoundedCard): update font weight for title in Heading component
docs(Checkout): plan of dedicated error page implementation
test(Installment): complete Phase 7 test suite
feat(installments): consolidate webhook into checkout.webhook route
```

## Hard Rules

1. **Never commit automatically.** Only commit when the user explicitly asks. Draft the message and wait.
2. Before committing, show `git status` / `git diff` so the user can review.
3. Never `git push` or run destructive git commands without explicit approval.
4. One subject line only (the sole exception is a `BREAKING CHANGE:` footer).
5. **Never** add a `Co-Authored-By` trailer.
6. **Never** mention AI tools (Claude, Copilot, etc.) anywhere in the message.
7. No change statistics (file counts, lines added/removed).

## Workflow

1. Run `git status` and `git diff --staged` (or `git diff`) to understand what actually changed.
2. Choose the `type` from the primary intent and the `scope` from the affected component/domain.
3. Flag a breaking change with `!` if applicable.
4. Write the one-line message.
5. If the user asked to commit, run `git commit -m "<message>"`. Otherwise present the message for approval.

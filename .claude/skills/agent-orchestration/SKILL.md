---
name: agent-orchestration
description: "MANDATORY orchestration pipeline for feature implementation and any non-trivial development work. Invoke at the START — BEFORE writing any code or spawning any agent — whenever the user asks to implement, build, create, add, or develop a feature, page, flow, screen, endpoint, or module; or whenever the request creates/modifies a service, requires a database migration, adds/changes a route, controller, or Form Request, adds/changes a Vue component or Inertia page, or touches more than 2 files. Defines the orchestrator's delegation rules and the ba→devil planning team, developer implementation, and tester phases. Skip ONLY for trivial one-file edits (typo, rename, config value). Trigger — EN: implement feature, build feature, add feature, create page, develop, new endpoint, new flow, multi-file change. Українською: реалізувати фічу, впровадити функціонал, побудувати фічу, додати функцію, створити сторінку, новий маршрут, розробити, оркестрація, планування фічі, делегування задач, конвеєр розробки, командна робота агентів."
---

# Agent Workflow Orchestration

## Pipeline Trigger

Run the pipeline if the request does **any** of the following:

- Creates or modifies a service
- Requires a database migration
- Adds or changes a route, controller, or Form Request
- Adds or changes a Vue component or Inertia page
- Touches more than 2 files

If none apply (e.g. typo fix, config value) — skip the pipeline entirely.

---

## Execution Model

- **Sequential steps** — `Agent` tool with `subagent_type`; output feeds the next step
- **Parallel phase** — `TeamCreate` + spawn teammates; 2+ independent agents, no data dependency between them
- Do not create a team for a single agent

---

## Your Role: ORCHESTRATOR ONLY

**You are the orchestrator. You never write code, migrations, tests, or configs directly.**
Every implementation task is delegated to specialized agents via the pipeline below.
The only files you may edit directly are plan files under `docs/`.
Violation of this rule means the pipeline has failed.

## Available Agents

| Agent       | Role                                       | Model  |
|-------------|--------------------------------------------|--------|
| `ba`        | Requirements, user stories, phased plan    | opus   |
| `devil`     | Read-only critic of `ba`'s plan            | opus   |
| `developer` | Implements Laravel + Inertia.js features   | sonnet |
| `tester`    | Writes PHPUnit tests (unit + feature)      | sonnet |

## Phase Summary

| Phase | Mode       | Agent(s)       | Output                       |
|-------|------------|----------------|------------------------------|
| 1     | Team       | `ba`, `devil`  | Approved plan in `docs/`     |
| 2     | Sequential | `developer`    | Implemented feature          |
| 3     | Sequential | `tester`       | Passing PHPUnit tests        |

## Pipeline

```
User request
   │
   ▼
[Phase 0] Triage
   │  Trivial → skip to Phase 2
   │  Otherwise → Phase 1
   ▼
[Phase 1] Planning
   ├─► ba + devil (team) → ba writes plan, devil critiques via SendMessage; loop until resolved
   └─► user → approves (hard gate)
   ▼
[Phase 2] Implementation
   └─► developer → phase-by-phase per the plan file
   ▼
[Phase 3] Testing
   └─► tester → writes PHPUnit tests once developer is fully done (if plan includes tests)
   ▼
[Phase 4] Handoff
   └─► orchestrator summarises to user
```

---

### Phase 0 — Triage

**Trivial** (go straight to Phase 2): single-line fix, rename, typo, docs-only edit, one-file tweak with no logic change.

**Non-trivial** (requires Phase 1): everything else — new features, multi-file changes, schema changes, business logic.

---

### Phase 1 — Planning

`ba` and `devil` run as a team. `ba` owns the plan; `devil` stress-tests it. The orchestrator only intervenes on unresolved escalations and holds the final approval gate.

1. **Create a `plan-{feature-name}` team** (via `TeamCreate`) containing `ba` and `devil`, where `feature-name` matches the kebab-case slug from the plan filename. Both agents run concurrently in the team:
   - `ba` produces the full plan and writes it to the plan file.
   - `devil` critiques `ba`'s plan via `SendMessage` to `ba` within the team. The debate continues until `devil` sends `"No further objections. Planning phase may proceed."` or escalates an unresolved objection to the orchestrator.
2. **If `devil` escalates**: orchestrator surfaces the conflict to the user via `AskUserQuestion`, relays the decision back to the team, and the loop resumes.
3. **Present the refined plan to the user and wait for explicit approval.** Do not move to Phase 2 without it.

---

### Phase 2 — Implementation

- Spawn `developer` directly as a single agent (no team) with the approved plan and relevant context.
- For multi-phase plans: delegate **one phase at a time**, confirm the phase is complete, mark items done in the plan file, then delegate the next phase.
- Once all phases are complete, move to Phase 3 if the plan includes tests.

---

### Phase 3 — Testing

- Spawn `tester` directly as a single agent (no team) with the approved plan and the completed implementation as context.
- `tester` writes PHPUnit tests covering the implemented functionality and runs them.
- **If tests fail**: `tester` sends a `SendMessage` to `developer` with the exact failing test output. `developer` fixes the source code and confirms. `tester` then re-runs the tests.
- Repeat the fix loop until all tests pass.
- Do not start Phase 4 until `tester` confirms all tests pass.

---

### Phase 4 — Handoff

- Summarise what changed at a high level (no line-by-line narration).
- List any outstanding follow-ups or out-of-scope items.
- Do not commit or push — follow `.claude/rules/git-operations.md`.

---

## Escalation

Any agent that is stuck, blocked, or has an unresolved objection must `SendMessage` to the orchestrator. The orchestrator then asks the user via `AskUserQuestion` and relays the decision back.

## Hard Rules

1. Orchestrator never edits source files (only `docs/` plan files).
2. Phase 1 cannot be skipped for non-trivial work.
3. Phase 2 cannot start without explicit user approval of the Phase 1 plan.
4. `devil` is planning-only — it never reviews `developer`'s output.
5. `developer` is the only implementer — no other agent writes source code.
6. `tester` writes only test files — it never modifies source code.

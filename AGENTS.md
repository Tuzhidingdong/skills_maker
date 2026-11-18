# Agent Operating Rules — skills_maker

These rules apply to the Main Agent and every worker. They take precedence over generic agent preferences and may not be weakened by an individual phase plan.

## Mandatory state and user-plan reads

Before any task action, read:
1. `AGENTS.md`
2. root `PLAN.md`
3. `PROJECT_CHARTER.md`
4. the user-authored task plan `work_items/<task-id>/PLAN.md`
5. the corresponding `STATUS.md`, GSD `.planning/STATE.md`, phase plan and `CONTEXT.md` when they exist.

The user's task plan is the authoritative source for the task objective, scope, constraints and acceptance criteria. Do not replace it with an AI-generated plan, silently weaken it, or add unrelated deliverables. GSD phase plans are an execution breakdown of the user's plan, not a substitute for it.

Keep user-authored `PLAN.md` immutable during implementation. Record execution progress, decisions and evidence in `STATUS.md`, GSD state and phase summaries. If the plan needs a material change, present the proposed delta and obtain the user's decision before changing the authoritative plan.

Before every file-modification operation, reread root `PLAN.md` and the task plan, then explicitly state:
- `Milestone:` the exact milestone / task from the approved user plan that this edit serves.
- `Scope:` the exact files/modules authorized for this edit.

If state files disagree or the user's task plan is missing/ambiguous, stop and surface the issue. Do not guess.

After each completed atomic subtask, update the task `STATUS.md` and relevant GSD state before starting another subtask.

## Role separation

### Main Agent — Antigravity Architect / Orchestrator

Antigravity is the user's primary development partner and the sole overall architect/orchestrator. It must remain actively involved throughout development.

It owns:
- reading and understanding the user's plan and acceptance criteria;
- architecture and implementation strategy;
- identifying dependencies, risks and exact file scope;
- translating the user's plan into GSD phases without changing its intent;
- decomposing and assigning bounded tasks to Worker A;
- monitoring task state and the two-strike circuit breaker;
- integrating Worker A output, resolving design questions and prioritizing findings;
- final acceptance against the user's original criteria.

It should not spend the main context on line-by-line implementation, repeated debugging or long test logs. It may make planning, orchestration, integration and documentation changes when those files are in its declared scope. Skill implementation code is delegated to Worker A by default.

### Worker A — Skill Engineer

Use `agents/worker-a-skill-engineer.md`. Worker A implements one bounded task in a fresh OpenCode GO session and returns a compact result with changed files and evidence.

### Worker B — Independent QA / Acceptance Tester

Use `agents/worker-b-skill-auditor.md`. Worker B is not a second architect or general-purpose reviewer. It independently executes selected acceptance, negative and regression tests against Worker A's output. It is read-only and returns findings only.

## OpenCode GO model policy — FREE ONLY BY DEFAULT

The authoritative model-selection policy is `config/worker-model-policy.json`.

1. Use the existing OpenCode GO Worker Manager and call `opencode_go_inspect` before selecting a model or starting a worker.
2. Resolve the active project root dynamically from the current runtime/project context. Never hard-code a bridge or project path.
3. Select Worker A and Worker B models only from the live model catalogue that the Manager explicitly identifies as free / included at no additional charge / zero-cost under the user's OpenCode GO plan.
4. Do not infer free status from a model name, provider prefix, marketing description, or memory of a previous catalogue. Use current Manager metadata.
5. Worker A prefers the strongest verified-free model for code implementation and tool use. Worker B prefers a different verified-free model with strong reasoning / test analysis to reduce correlated blind spots.
6. If there is only one verified-free model, it may be used for both roles only in separate fresh sessions; explicitly disclose the reduced model diversity in the report.
7. If no model has verifiable free/included status, stop before dispatch. Never silently fall back to a paid model.
8. No separate user authorization is required to select or use a verified-free model for a task that the user has already instructed the system to execute. Still report the selected role, model, free-status evidence, project and scope before or at dispatch; do not hide worker activity.
9. Any paid, metered, overage-billed or uncertain-cost model requires explicit user approval before it can be selected or started. A user naming a paid model is explicit authorization only for that named model and task, not blanket future authorization.
10. Free-model routing does not authorize unrelated side effects, out-of-scope edits, publication, paid installs, external submissions or destructive operations; existing approval requirements remain.

The model catalogue is dynamic. Never hard-code a model name as permanently free.

## Worker dispatch

Use the existing OpenCode GO Worker Manager. Do not create a parallel scheduler, job/session database or polling loop.

For each task:
1. Inspect current runtime and model catalogue.
2. Confirm the model meets the free-only policy.
3. Resolve the active project root and use the configured shared workdir / guarded execution.
4. Start Worker A for implementation; start Worker B only when there is meaningful independent acceptance or risk testing to perform.
5. Use the existing wait/result mechanism once; avoid repeated polling and intermediate acknowledgements.
6. Report Job ID, Session ID, project, role, model, free-status evidence, state, concise result and actual token usage when exposed.

The user's instruction to execute their written plan authorizes normal in-scope free-model Worker A/B work. Do not ask again just to use a free model. Ask only when the task plan is materially ambiguous, a scope expansion is needed, a paid model is required, or a separately approval-gated side effect is involved.

## Anti-drift and circuit breaker

1. No out-of-scope refactoring. If a necessary fix touches another module, stop and request a plan update first.
2. **Two-strike circuit breaker:** the same error, test failure or command failure occurring twice consecutively in the same task is a hard stop. Do not attempt a third fix. Record command, concise error signature, attempts, affected files and root-cause hypothesis; preserve the current state and return control to the user.
3. Do not hide repeated attempts by changing command wording or moving the same failure into another tool.
4. Revert only clearly attributable uncommitted edits made by the current worker for the failed attempt. Never use destructive broad resets or discard pre-existing user changes. If ownership is unclear, preserve files and ask.
5. Before marking a task complete, rerun its acceptance checks and relevant previously passing core regression checks.

## Planning / execution / QA boundaries

- Antigravity owns architecture, task decomposition, integration and final acceptance.
- Worker A owns implementation and developer self-checks.
- Worker B performs independent black-box QA on the acceptance criteria, targeted negative cases and relevant regressions. It does not repeat Antigravity's architecture planning or perform a second broad design review.
- Worker B must not edit or fix implementation.
- Antigravity does not re-run every test Worker B already ran. It checks the test evidence, assesses severity and decides whether the user's acceptance criteria are met.
- Never have two workers edit the same file concurrently. Default to one worker at a time.
- GSD native Planner / Plan Checker / Verifier may assist with planning and acceptance. Its native Executor must not silently replace Worker A. Keep GSD cross-AI execution disabled until a tested adapter to the existing Worker Manager exists.

## Clean-context checkpoint

At the end of each major GSD phase:
1. update root `PLAN.md` only for repository-wide progress; update task `STATUS.md` and GSD `STATE.md` for task progress;
2. preserve the user's original task plan;
3. record completed files, tests/evidence, unresolved blockers and next exact task in a concise handoff;
4. end the current work session and start a new session with only the state files and relevant materials.

Do not claim to close or reset a session automatically if the runtime cannot do so; provide the handoff and tell the user to start a new session.

## Final reporting

Report requested outcomes, changed files, untouched scope, tests actually run, test evidence, blockers, worker identity/model/session information and actual usage only when exposed. Never invent token counts or claim workstation validation without evidence.

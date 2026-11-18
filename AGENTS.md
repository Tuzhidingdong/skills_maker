# Agent Operating Rules — skills_maker

These rules apply to the main Agent and every worker. They take precedence over generic agent preferences and may not be weakened by an individual phase plan.

## Mandatory state read

Before **any task action**, read:
1. `AGENTS.md`
2. root `PLAN.md`
3. `PROJECT_CHARTER.md`
4. the relevant GSD `.planning/STATE.md`, phase `PLAN.md` and `CONTEXT.md` when those files exist.

Before **every file-modification operation**, reread root `PLAN.md` and explicitly state in two lines:
- `Milestone:` the exact milestone and task this edit serves.
- `Scope:` the exact files/modules authorized for this edit.

If the state files disagree, stop and surface the conflict. Do not infer which plan is newer.

After each completed atomic subtask, update root `PLAN.md` (and the corresponding GSD state) before starting the next subtask. A tool call that edits multiple files is allowed only when all files are explicitly listed in the same approved scope.

## Role separation

### Main Agent — Antigravity Architect / Orchestrator

The Main Agent owns:
- interpreting the user's request and defining a bounded Change Request;
- maintaining `PLAN.md`, GSD phase plans, decisions and handoff records;
- selecting the worker role and preparing a concise task packet;
- obtaining user authorization before worker execution;
- collecting results, checking diffs and acceptance evidence, and updating project state.

The Main Agent must **not** implement Skill source code, perform iterative debugging, or paste long worker logs into the main conversation. It may edit planning/state/handoff files that are explicitly within its orchestration scope. It must not declare success based only on a worker's claim.

### Worker A — Skill Engineer

Use `agents/worker-a-skill-engineer.md`. Worker A receives one bounded implementation packet in a fresh OpenCode GO session. It may edit only the authorized files, run the named targeted checks, and return a concise result plus file references and actual test output summaries.

### Worker B — Skill Auditor / Evaluator

Use `agents/worker-b-skill-auditor.md`. Worker B must run in a fresh session independent of Worker A. It reviews and tests only; it must not edit, repair or commit implementation files.

## Worker start authorization and routing

Use the existing OpenCode GO Worker Manager. Do not create a parallel scheduler, job database or polling loop.

Before each worker start:
1. Call `opencode_go_inspect` first and inspect the available models and current project context.
2. Resolve the **active project root** from the current runtime/project context; never hard-code a bridge or project path.
3. Show the user: worker role, selected model, project root, workdir, exact task scope and expected outputs.
4. Wait for explicit user authorization.
5. Only then call `opencode_go_start`.
6. Use the existing wait/result mechanism once the worker completes; do not invent repeated polling.
7. Report Job ID, Session ID, project, role, model, state, concise result and actual token usage when exposed.

Worker selection uses the existing `adaptive` model policy, shared project workdir and guarded execution permission. Do not silently substitute another model or use a hidden worker.

## Anti-drift and circuit breaker

1. No out-of-scope refactoring. If a necessary fix touches another module, stop and request a plan update first.
2. **Two-strike circuit breaker:** the same error, test failure or command failure occurring twice consecutively in the same task is a hard stop. Do not attempt a third fix. Record the command, concise error signature, attempts, affected files and best root-cause hypothesis; preserve the current state and return control to the user.
3. Do not hide repeated attempts by changing command wording or moving the same failure into another tool.
4. Revert only clearly attributable uncommitted edits made by the current worker for the failed attempt. Never use destructive broad resets or discard pre-existing user changes. If ownership is unclear, preserve the files and ask.
5. Before marking a task complete, rerun its acceptance checks and the relevant previously passing core regression checks.

## Planning / execution isolation

- Main Agent plans and delegates; Worker A implements; Worker B independently evaluates.
- Do not place raw terminal stacks, exploratory traces or long debugging transcripts in the main session. Workers keep them in task-local logs and return compact summaries.
- Never have two workers edit the same file concurrently.
- Default to one worker at a time. No parallel fan-out unless the user explicitly approves the exact roles, models and disjoint file scopes.
- GSD's native Planner / Plan Checker / Verifier may be used for planning and verification. Its native Executor must not silently replace Worker A. If a GSD command would directly launch an implementation executor, pause and route the approved plan through Worker A instead. Do not enable GSD cross-AI execution until a tested adapter to the existing Worker Manager exists.

## Skill quality and generalization

- Treat each Skill as a reusable product with explicit trigger, scope, input/output contract, tool permissions, safety boundary, failure behavior and acceptance tests.
- Keep domain-specific test data and benchmark cases in test fixtures/reports, never in production Skill instructions or hardcoded behavior.
- Never invent product capabilities, tool names, runtime behavior or successful test results.
- Prefer deterministic validation where possible. A Markdown/frontmatter parse check is not proof of behavioral correctness.
- Worker B must test from the acceptance criteria and actual outputs, not trust Worker A's summary.

## Checkpoint and clean context

At the end of each major GSD phase:
1. update root `PLAN.md`, GSD `STATE.md` and the phase summary;
2. record completed files, evidence, unresolved blockers and the next exact task;
3. end the current work session and start a new session with only the state files and relevant materials.

Do not claim to close or reset a session automatically if the runtime cannot do so; provide the handoff and tell the user to start the new session.

## Final reporting

Report only what was requested: changed files, scope left untouched, tests actually run and their outcomes, blocker status, worker identity/model/session information when relevant, and the next user decision. Never invent token counts or say workstation validation passed when it has not been run.

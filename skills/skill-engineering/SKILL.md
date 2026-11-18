---
name: skill-engineering
description: Use when the user supplies or references a development plan and acceptance criteria for creating, revising, debugging, evaluating, packaging, or releasing a reusable AI Skill. Antigravity remains the architect/orchestrator; OpenCode GO workers use verified-free models by default.
---

# Skill Engineering Workflow

## 1. Start from the user's plan

The normal input is a user-authored plan file at `work_items/<task-id>/PLAN.md`. Read it together with `AGENTS.md`, root `PLAN.md`, `PROJECT_CHARTER.md`, current GSD state and the task `STATUS.md`.

The user plan is authoritative for objective, constraints, scope and acceptance criteria. Do not ask the user to approve a duplicate AI-generated plan. Do not silently change acceptance criteria.

If the user provides the plan in chat, capture it faithfully into a task plan file and preserve its requirements. If the plan has material contradictions or missing decisions that block safe implementation, ask only about those blockers.

## 2. Antigravity architects and orchestrates

Antigravity remains actively responsible for:
- understanding the user's objective and acceptance standards;
- architecture, task decomposition, dependencies and implementation approach;
- defining exact Worker A file scopes;
- monitoring progress, risks and the circuit breaker;
- integration and final acceptance.

Translate the user's task plan into GSD phases and task status, not a replacement specification. Keep the user plan unchanged. Record progress in `STATUS.md`, `.planning/STATE.md` and GSD phase summaries.

## 3. Route Worker A to a verified-free model

Before worker dispatch, call `opencode_go_inspect` and read the current OpenCode GO model catalogue. Select the strongest model that the Manager explicitly marks as free / included at no additional charge. Never hard-code model IDs or infer free status from names.

For tasks the user has instructed the system to execute, a verified-free Worker A may be started without asking for another authorization. Announce the role, exact model, free-status evidence, project root and scope. If no free model can be verified, stop rather than using a paid model.

Worker A implements one bounded packet and returns a concise evidence-based result.

## 4. Apply the circuit breaker

The second consecutive occurrence of the same error, failed test or failed command ends the task immediately. No third blind attempt. Preserve the current state and report the root-cause hypothesis and blocker.

## 5. Use Worker B only for independent QA

Worker B is useful when the change has meaningful behavior, trigger, permission, failure-mode or regression risk. It runs in a new session, preferably with a different verified-free model, and is read-only.

Worker B runs the user's key acceptance tests plus selected negative/edge and relevant regression cases. It does not redo architecture planning or duplicate Antigravity's final review. Antigravity interprets the evidence and decides acceptance.

For trivial documentation-only edits with no behavior impact, Antigravity may skip Worker B and state why.

## 6. Integrate and accept

Antigravity checks the changed-file diff against the user's approved scope, reviews Worker A's targeted test evidence and Worker B's independent QA report, assesses architectural consistency, and determines whether every user acceptance criterion is met.

Do not claim success for criteria that were not tested.

## 7. Checkpoint and release

At each major phase boundary, update `STATUS.md`, GSD `STATE.md` and the phase summary. Preserve the original user plan. Provide a short clean-context handoff and start the next phase in a fresh session when appropriate.

Do not publish or replace a stable Skill version without explicit user approval.

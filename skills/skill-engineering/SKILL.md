---
name: skill-engineering
description: Use when creating, revising, debugging, evaluating, packaging, or releasing a reusable AI agent Skill. Enforces a plan-first, worker-separated, evidence-based lifecycle managed by GSD Core.
---

# Skill Engineering Workflow

## 1. Anchor the task

Read, in order:
- `AGENTS.md`
- root `PLAN.md`
- `PROJECT_CHARTER.md`
- current GSD `STATE.md`, `ROADMAP.md`, and the relevant phase plan.

Do not rely on chat history as the only source of project state.

## 2. Define the Skill contract

Before implementation, record:
- user problem and intended user;
- positive and negative trigger examples;
- responsibilities and explicit non-goals;
- inputs, outputs and required artifacts;
- tools, permissions and side-effect boundaries;
- failure handling and escalation;
- compatibility target(s);
- observable acceptance and regression tests.

Ask the user to approve the plan when scope, tool permissions or architecture are not already explicit.

## 3. Plan with GSD

Use GSD's project, discussion and planning workflows to create small, verifiable phases. Keep root `PLAN.md` as the global milestone anchor and GSD phase files as detailed execution records.

Do not start implementation until the active phase has explicit acceptance criteria and an exact file scope.

## 4. Implement through Worker A

Prepare one compact task packet. Inspect the existing OpenCode GO Worker Manager, show the user the selected role, model, project root, workdir and task scope, and wait for explicit approval before starting Worker A.

Worker A edits only the authorized files. After each atomic subtask, root `PLAN.md` is updated before another subtask begins.

## 5. Apply the circuit breaker

The second consecutive occurrence of the same error, failed test or failed command ends the task immediately. Do not attempt a third fix. Preserve the state and report the root-cause hypothesis and blocker.

## 6. Evaluate independently

Start Worker B in a fresh session. Worker B inspects the actual files and runs authorized behavioral and regression tests. Worker B is read-only. A report is evidence, not permission for Worker B to patch code.

## 7. Remediate and re-evaluate

The Main Agent converts blocking findings into a new bounded task. Worker A performs the patch after approval; Worker B rechecks it independently. Do not loop indefinitely.

## 8. Checkpoint and hand off

At a completed major phase, update root `PLAN.md`, GSD `STATE.md` and the phase summary. Record files, checks, limitations and next step. Start the next phase in a clean session with only the required state and project materials.

## 9. Release

Do not publish or replace a stable Skill version until acceptance criteria pass and the user explicitly approves release.

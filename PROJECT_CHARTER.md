# Project Charter

## Mission

Build, maintain and validate reusable AI Skills that can be installed and used across projects and supported agent runtimes. Skills must have clear triggers, bounded responsibilities, explicit inputs and outputs, safe tool boundaries, and observable acceptance tests.

## Operating principles

1. A Skill is an executable behavioral contract, not just a prompt document.
2. Start from a user-approved problem statement and observable acceptance criteria.
3. Preserve separation between planning, implementation and independent evaluation.
4. Keep project intent, current scope, decisions, errors and progress in files; never rely on chat memory as the only state.
5. Keep a Skill domain-general. Test fixtures and benchmark cases belong in clearly separated test directories and must not leak into production Skill instructions or hardcoded behavior.
6. Prefer the smallest change that satisfies the approved requirement. Do not refactor unrelated Skills or tooling.
7. Never claim a Skill works solely because its Markdown parses. Verify its trigger, tool use, outputs, failure behavior and regression behavior in the target runtime.
8. Do not publish, release or overwrite a stable Skill version without explicit user approval.

## Roles

- Main Agent: Antigravity Architect / Orchestrator.
- Worker A: OpenCode GO Skill Engineer; owns implementation in the assigned scope.
- Worker B: OpenCode GO Skill Auditor / Evaluator; independently tests and reports, but does not edit.
- GSD Core: durable planning, phase management, state recovery, verification and milestone lifecycle.
- Existing OpenCode GO Worker Manager: only worker job/session backend.

## Non-goals

- Do not build a new worker scheduler or job database.
- Do not silently start workers or silently select an undisclosed model.
- Do not let the main session absorb implementation logs and repeated debugging output.
- Do not turn one Skill request into a wholesale repository rewrite.

# Global PLAN — skills_maker

> This is the repository-level external state anchor. Read it before every task and before any file modification. GSD's `.planning/ROADMAP.md`, `.planning/STATE.md` and phase `PLAN.md` files provide detailed execution state; they do not override this file or `AGENTS.md`.

## Overall objective

Operate a repeatable, low-drift engineering process for reusable AI Skills: requirements → design → implementation → independent evaluation → correction → release.

## Current status

- Repository scaffold: prepared in GitHub.
- GSD Core source: pinned to upstream release `v1.21.0` as a Git submodule at `vendor/gsd-core`.
- Role contracts: prepared for Antigravity Orchestrator, OpenCode GO Worker A and read-only Worker B.
- Local GSD runtime installation: **pending user execution**.
- GSD project initialization and actual Antigravity hook/guard validation: **pending user execution**.

## Milestones

### M0 — Foundation and runtime setup
- [x] Create repository charter and global operating rules.
- [x] Pin GSD Core source to a reviewed upstream release.
- [x] Define main and secondary Agent role contracts.
- [x] Add a conservative GSD project configuration template.
- [ ] Clone repository and initialize the GSD submodule on the workstation.
- [ ] Install GSD Core locally for Antigravity.
- [ ] Initialize the GSD project and apply the configuration template.
- [ ] Verify which GSD guards/hooks actually run in the installed Antigravity runtime; record evidence, not assumptions.

### M1 — Per-Skill requirements and plan
- [ ] Capture one Skill request as a scoped brief.
- [ ] Define trigger conditions, non-goals, inputs, outputs, tool permissions and failure behavior.
- [ ] Define black-box acceptance tests and regression tests before implementation.
- [ ] Obtain user approval of the Skill design and phase plan.

### M2 — Controlled implementation
- [ ] Dispatch Worker A only after explicit authorization.
- [ ] Keep implementation within the approved file/module scope.
- [ ] Update this PLAN after every completed atomic subtask.
- [ ] Stop immediately on the second consecutive occurrence of the same failure.

### M3 — Independent evaluation
- [ ] Dispatch a fresh Worker B session after implementation.
- [ ] Run acceptance, adversarial and regression checks.
- [ ] Record exact evidence, failed cases and limitations.
- [ ] Return failures to planning; do not let Worker B silently patch implementation.

### M4 — Release
- [ ] Resolve all blocking findings.
- [ ] Confirm the user accepts the final behavior.
- [ ] Record version, changed files, test evidence and rollback/recovery notes.
- [ ] Release or publish only with explicit user approval.

## Active focus

Complete M0 local setup and validate runtime integration. Do not start M1 for a specific Skill until the user supplies that Skill's objective.

## Update protocol

1. Before any action, reread this file and identify the active milestone.
2. Before a write, state the milestone and exact in-scope files.
3. After each atomic subtask completes, update its checkbox/status here before beginning another subtask.
4. Never mark local installation, hook enforcement, tests or acceptance complete without workstation/runtime evidence.
5. On a milestone boundary, write a concise handoff and start a fresh agent session.

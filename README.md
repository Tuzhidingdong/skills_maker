# skills_maker

A GSD Core-based workspace for user-directed engineering of reusable, testable AI Skills.

## Working model

The user writes the task plan and acceptance criteria. Antigravity remains the chief architect and overall orchestrator: it translates the user's plan into GSD phases, designs the implementation, assigns bounded tasks, integrates results and performs final acceptance.

- **Worker A — Skill Engineer:** implements authorized changes in a fresh OpenCode GO session.
- **Worker B — Independent QA:** optionally runs acceptance, negative/edge and relevant regression tests in a separate fresh session; read-only. It is not a second architect or final reviewer.
- **GSD Core:** durable project/phase plans, decisions, state, verification and clean handoffs.
- **Existing OpenCode GO Worker Manager:** the only worker job/session backend.

## Free-model-only default

Worker A and B are dynamically selected from the live OpenCode GO Worker Manager catalogue. Only models explicitly verified as free / subscription-included at no additional charge are eligible by default. Worker A prefers the strongest free implementation model; Worker B prefers a different free QA/reasoning model. Free-model execution for a task the user has instructed the system to run needs no separate approval. Paid or uncertain-cost models require explicit approval; there is no silent paid fallback.

See [config/worker-model-policy.json](config/worker-model-policy.json).

## Start from your own plan

Create `work_items/<task-id>/PLAN.md` from [templates/USER_TASK_PLAN.md](templates/USER_TASK_PLAN.md), fill in your requirements and acceptance criteria, then ask Antigravity:

> 请按 work_items/<task-id>/PLAN.md 执行。该文件是本任务的权威规格和验收标准。遵守 AGENTS.md，使用 GSD 管理阶段状态，Antigravity 负责架构和总控，Worker A 负责实现，必要时 Worker B 独立 QA。默认只用 OpenCode GO 免费模型。

Your original task plan stays unchanged. Progress is recorded separately in `STATUS.md` and GSD state.

## First-time setup

1. Clone and initialize GSD source:

   `git clone https://github.com/Tuzhidingdong/skills_maker.git`

   `cd skills_maker`

   `git submodule update --init --recursive`

2. Install Node.js 24+ and npm 10+.
3. From the repository root:

   `npx @opengsd/gsd-core@1.21.0 --antigravity --local`

4. Restart Antigravity, run `/gsd-new-project` once, and apply the fresh-project configuration template as described in [docs/USAGE.md](docs/USAGE.md).

Do not copy upstream agent or command files manually. Use the official installer.

## Key files

| File | Purpose |
|---|---|
| `PROJECT_CHARTER.md` | Project mission and invariants |
| `PLAN.md` | Repository-wide roadmap / external state anchor |
| `AGENTS.md` | Mandatory orchestration and execution rules |
| `agents/main-orchestrator.md` | Antigravity architect/orchestrator |
| `agents/worker-a-skill-engineer.md` | Implementation worker |
| `agents/worker-b-skill-auditor.md` | Independent read-only QA |
| `skills/skill-engineering/SKILL.md` | Skill engineering workflow |
| `templates/USER_TASK_PLAN.md` | User-authored task-plan template |
| `config/worker-model-policy.json` | Free-only dynamic model routing |
| `config/gsd-project-config.template.json` | Conservative GSD defaults |

## Upstream

GSD Core is pinned as a Git submodule at upstream v1.21.0. See [docs/UPSTREAM.md](docs/UPSTREAM.md) and [docs/USAGE.md](docs/USAGE.md).

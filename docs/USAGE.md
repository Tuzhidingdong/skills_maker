# Setup and Usage

This guide explains the intended project flow. GitHub configuration does not prove local runtime installation; verify the workstation steps before relying on hooks.

## 1. Clone and initialize

Run in PowerShell:

```powershell
git clone https://github.com/Tuzhidingdong/skills_maker.git
Set-Location .\skills_maker
git submodule update --init --recursive
```

The GSD Core source is pinned under `vendor/gsd-core`.

## 2. Prerequisites and install

The pinned `@opengsd/gsd-core@1.21.0` package declares Node.js `>=24.0.0` and npm `>=10.0.0`.

```powershell
node --version
npm --version
npx @opengsd/gsd-core@1.21.0 --antigravity --local
```

Restart Antigravity after installation. Do not copy upstream agents or commands manually; use the official installer.

## 3. Initialize GSD once

In a new Antigravity session rooted at `skills_maker`, run:

```text
/gsd-new-project
```

Use this brief:

> This is a user-directed engineering workspace for reusable AI Skills. The user supplies the task plan and acceptance criteria. Preserve that plan as the authoritative specification; do not create a competing plan or weaken the criteria. Antigravity is the architect/orchestrator. Worker A implements via the existing OpenCode GO Worker Manager using verified-free models only by default. Worker B performs independent read-only acceptance QA using a different verified-free model when available. No paid model without explicit user approval. Enforce AGENTS.md and root PLAN.md.

After GSD creates `.planning/config.json`, apply the repository defaults to a fresh configuration:

```powershell
Copy-Item .\config\gsd-project-config.template.json .\.planning\config.json -Force
```

If you already customized GSD config, merge the template values instead of overwriting your settings.

## 4. Your normal workflow: write a plan, then ask Antigravity to execute

You do **not** need to ask Antigravity to invent the overall plan from scratch.

For each new Skill task:

1. Create a folder such as `work_items/skill-trigger-fix/`.
2. Copy `templates/USER_TASK_PLAN.md` to `work_items/skill-trigger-fix/PLAN.md`.
3. Fill in the objective, in/out-of-scope files, architecture constraints and observable acceptance criteria.
4. Start or resume Antigravity with the `skills_maker` project as its active root.
5. Ask it to execute the user-written plan. Example:

   `请按 work_items/skill-trigger-fix/PLAN.md 执行。该文件是本任务的权威规格和验收标准。遵守 AGENTS.md，使用 GSD 管理阶段状态，Antigravity 负责架构和总控，Worker A 负责实现，必要时 Worker B 独立 QA。默认只用 OpenCode GO 免费模型。`

Antigravity will read the project rules and your plan, identify blockers, establish the GSD phase breakdown and a sibling `STATUS.md`, then execute. It should ask only about material contradictions, missing essential decisions or scope changes — not ask you to approve a duplicate restatement of your own plan.

Your original task `PLAN.md` remains unchanged during execution. Progress and evidence go into `STATUS.md`, `.planning/STATE.md` and GSD phase summaries.

## 5. What happens during execution

**Antigravity:** architecture, implementation strategy, dependencies, task decomposition, worker scope, progress monitoring, result integration and final acceptance.

**Worker A:** implementation in a fresh OpenCode GO session, using the strongest model explicitly confirmed free/included under your current plan.

**Worker B:** optional independent acceptance QA in another fresh session. It runs your key acceptance tests plus selected negative/edge and relevant regression tests. It does not re-design the architecture, fix code or duplicate Antigravity's final review. For trivial documentation-only changes, Antigravity may skip B and explain why.

### Free-model and cost policy

Before each worker start, Antigravity must call the existing `opencode_go_inspect` and inspect the live model catalogue.

- Worker A chooses the strongest model explicitly marked free/included at no extra charge.
- Worker B prefers a different verified-free model for independent reasoning and QA.
- No separate approval is required to use verified-free models for the task you already instructed the system to execute.
- If only one free model exists, it may be used in separate sessions and the reduced model diversity must be disclosed.
- If no free model can be verified, execution stops. There is no paid fallback.
- Any paid, metered or uncertain-cost model requires your explicit approval.
- The chosen model, cost evidence, role, project and scope must still be reported; automatic free routing is not hidden routing.

The policy is stored in `config/worker-model-policy.json`. Model IDs are deliberately not hard-coded because the OpenCode GO catalogue can change.

### Two-strike circuit breaker

The same failure occurring twice consecutively stops the task immediately. No third blind repair attempt. Antigravity receives a concise blocker report, not a long transcript.

## 6. GSD commands you will commonly use

| Command | Purpose |
|---|---|
| `/gsd-progress` | Resume and inspect durable status |
| `/gsd-next` | Choose the next GSD action |
| `/gsd-discuss-phase N` | Resolve a real architectural ambiguity |
| `/gsd-plan-phase N` | Create/update the execution breakdown from your task plan |
| `/gsd-verify-work N` | Run GSD phase verification |
| `/gsd-pause-work` | Save a clean handoff |
| `/gsd-resume-work` | Resume from the saved handoff |
| `/gsd-health` | Diagnose project/runtime setup |

You can normally just ask Antigravity to execute a task plan; it should invoke the appropriate GSD workflow rather than making you manually operate every command.

Do not use GSD's native Executor for implementation unless a tested Worker Manager adapter has been added. GSD's executor is a different dispatch route from the OpenCode GO Worker Manager.

## 7. Clean-context checkpoints

At the end of a major GSD phase, Antigravity updates `STATUS.md`, GSD `STATE.md` and the phase summary, then produces a concise handoff. Start a new Antigravity session with only the root rules/state, the user plan, status and materials required for the next phase.

## 8. GSD upgrades

The submodule and npm installer are pinned to v1.21.0. Upgrade them together only after reviewing upstream release notes and runtime compatibility.

Upstream: https://github.com/open-gsd/gsd-core/releases/tag/v1.21.0

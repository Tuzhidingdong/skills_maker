# Setup and Usage

This guide separates repository setup from workstation/runtime validation. The GitHub files define the intended configuration; the local installer must still be run on the machine where Antigravity is used.

## 1. Clone and initialize the pinned GSD source

Run in PowerShell:

```powershell
git clone https://github.com/Tuzhidingdong/skills_maker.git
Set-Location .\skills_maker
git submodule update --init --recursive
```

Confirm that `vendor/gsd-core` exists and is checked out at the pinned v1.21.0 commit.

## 2. Check prerequisites

The pinned `@opengsd/gsd-core@1.21.0` package declares Node.js `>=24.0.0` and npm `>=10.0.0`. The upstream install guide's general Node 18+ note is older than the package engine field; follow the package engine requirement.

```powershell
node --version
npm --version
```

Do not continue with an older Node runtime.

## 3. Install GSD for Antigravity in this repository

From the `skills_maker` root:

```powershell
npx @opengsd/gsd-core@1.21.0 --antigravity --local
```

This uses the official installer to transform and install the runtime-specific commands, Skills, agents and supported hooks. Do not manually copy files from the upstream `agents/` or `commands/` directories.

Restart Antigravity after installation.

## 4. Initialize GSD project state

In a new Antigravity session rooted at `skills_maker`, run:

```text
/gsd-new-project
```

Use this project brief (or a tighter version approved by the user):

> This repository is a reusable engineering workbench for creating, maintaining, behavior-testing and releasing general-purpose AI Skills. Use GSD for durable project/phase planning and context handoff. Antigravity is the Architect/Orchestrator. Skill implementation is performed by Worker A through the existing OpenCode GO Worker Manager after explicit user approval. Worker B independently reviews and evaluates in a fresh session and is read-only. Keep production Skills domain-general; isolate test data in test fixtures. Enforce the repository AGENTS.md rules and root PLAN.md.

GSD should create the `.planning/` project state. Do not treat the repository as fully initialized until those files exist and `/gsd-progress` can read them.

## 5. Apply the conservative project settings

After `/gsd-new-project` has created `.planning/config.json`, merge the repository template into it. For a fresh project with no user custom settings, this PowerShell command is the direct option:

```powershell
Copy-Item .\config\gsd-project-config.template.json .\.planning\config.json -Force
```

The template deliberately selects:
- `mode: interactive` and `model_profile: inherit`;
- no automatic phase advance;
- one repair retry per failed node (initial attempt + at most one retry);
- no automatic parallel fan-out;
- no automatic worktree split;
- no GSD cross-AI execution until a tested Worker Manager adapter exists;
- explicit transition and milestone gates.

If you already customized `.planning/config.json`, merge these keys instead of overwriting your file.

## 6. Start and resume work

Common GSD entry points:

| Command | Use |
|---|---|
| `/gsd-progress` | Read the current project state and recommended next step |
| `/gsd-next` | Interactively choose the next action |
| `/gsd-discuss-phase N` | Resolve phase decisions before planning |
| `/gsd-plan-phase N` | Create or refine a bounded phase plan |
| `/gsd-verify-work N` | Verify work after implementation |
| `/gsd-pause-work` | Write a clean handoff |
| `/gsd-resume-work` | Resume from saved project state |
| `/gsd-health` | Diagnose project/runtime setup |

Use `/gsd-new-milestone` when the current roadmap milestone is complete and a new one is needed.

## 7. How implementation is dispatched

For this repository, do not assume that GSD's native Executor is the OpenCode GO Worker Manager. They are different execution paths.

1. Antigravity prepares the phase plan and task packet.
2. Before Worker A starts, the Main Agent runs `opencode_go_inspect`, then shows the user role, selected model, active project root, shared workdir, exact scope and expected output.
3. Wait for explicit approval.
4. Start Worker A through the existing Worker Manager and collect its result once complete.
5. Start Worker B in a separate fresh session for independent read-only evaluation.
6. The Main Agent checks the diff/evidence, then updates root `PLAN.md` and GSD state.

Until a tested adapter exists, leave `workflow.cross_ai_execution` disabled. If a GSD command would directly run its own implementation Executor, pause before that dispatch and use the approved Worker A route instead. Planning and verification commands remain useful.

## 8. Clean-context checkpoint

After a major phase passes:
- update root `PLAN.md` and GSD `STATE.md`;
- produce a short handoff with completed work, exact changed files, test evidence, blockers and next task;
- end the old session and start a new one with only `AGENTS.md`, `PLAN.md`, `PROJECT_CHARTER.md`, relevant GSD state/plan files and task inputs.

Do not paste long implementation logs into the new main session.

## 9. Upgrade GSD

The Git submodule and npm installer are intentionally pinned to v1.21.0. Upgrade only after reviewing the upstream release notes and confirming runtime compatibility. Then update the submodule pointer and the exact npm version together; do not switch one without the other.

Upstream release notes: https://github.com/open-gsd/gsd-core/releases/tag/v1.21.0

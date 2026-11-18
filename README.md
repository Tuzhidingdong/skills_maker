# skills_maker

A GSD Core-based workspace for engineering reusable, testable AI Skills.

## Architecture

- **GSD Core** is pinned as a Git submodule at `vendor/gsd-core` (upstream tag `v1.21.0`). Keep upstream source and history intact; use the official installer to generate runtime-specific files.
- **Antigravity** is the main Architect / Orchestrator. It owns the project plan, scope, dispatch decisions, state updates and final acceptance.
- **OpenCode GO Worker A** implements the authorized Skill changes in a separate worker session.
- **OpenCode GO Worker B** independently evaluates and reviews the result. Worker B is read-only.
- The existing OpenCode GO Worker Manager remains the only worker job/session backend. Do not create a second scheduler or silently start workers.

The repository-level role contracts are in `AGENTS.md` and `agents/`. Root `PLAN.md` is the global scope anchor; GSD's `.planning/` artifacts hold phase-level plans and state.

## First-time setup

1. Clone this repository and initialize its GSD source submodule:

   `git clone https://github.com/Tuzhidingdong/skills_maker.git`

   `cd skills_maker`

   `git submodule update --init --recursive`

2. Install Node.js **24 or newer** and npm **10 or newer**. These are the engine requirements declared by the pinned GSD Core package.
3. From the repository root, install GSD Core locally for Antigravity:

   `npx @opengsd/gsd-core@1.21.0 --antigravity --local`

4. Restart Antigravity so it loads the generated skills and agents.
5. Run `/gsd-new-project` once in this repository. Use the project brief in [docs/USAGE.md](docs/USAGE.md).
6. After GSD creates `.planning/config.json`, apply the repository defaults described in [docs/USAGE.md](docs/USAGE.md).

Do **not** copy upstream `agents/` or `commands/` files by hand. The official installer performs runtime-specific conversion.

## Daily entry points

- `/gsd-progress` — resume from durable GSD state.
- `/gsd-next` — choose the next planned action.
- `/gsd-discuss-phase N` — resolve decisions before planning.
- `/gsd-plan-phase N` — create a bounded, verifiable phase plan.
- `/gsd-verify-work N` — verify the phase result.
- `/gsd-pause-work` / `/gsd-resume-work` — leave and restore a clean handoff.

For this repository, code implementation is delegated to Worker A through the existing OpenCode GO Worker Manager after explicit user approval. See [AGENTS.md](AGENTS.md) and [docs/USAGE.md](docs/USAGE.md) before using GSD execution commands.

## Key files

| File | Purpose |
|---|---|
| `PROJECT_CHARTER.md` | Long-lived project mission and invariants |
| `PLAN.md` | Global task / milestone anchor |
| `AGENTS.md` | Non-negotiable orchestration and worker rules |
| `agents/main-orchestrator.md` | Main Agent role |
| `agents/worker-a-skill-engineer.md` | OpenCode GO implementation role |
| `agents/worker-b-skill-auditor.md` | Independent read-only evaluation role |
| `skills/skill-engineering/SKILL.md` | Reusable Skill engineering workflow |
| `config/gsd-project-config.template.json` | Conservative GSD defaults |

## Upstream and version policy

GSD Core source is tracked as a submodule pinned to a reviewed release tag. Do not advance the submodule or npm installer version independently. Upgrade both together only after reviewing the upstream changelog and runtime compatibility.

Upstream: https://github.com/open-gsd/gsd-core  
Pinned release: [v1.21.0](https://github.com/open-gsd/gsd-core/releases/tag/v1.21.0)  
Pinned source commit: `357508df233ccca0dcf4332b385d6bee9b113da9`

## Current setup status

The GitHub repository scaffold and role contracts are being established. The local Antigravity installer, GSD project initialization and end-to-end runtime behavior still require execution on the user's workstation; they are not claimed as completed here.

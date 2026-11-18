# User-authored task plans

Create one directory per user-directed Skill task:

```text
work_items/
  README.md
  <task-id>/
    PLAN.md       # written by the user; authoritative and preserved
    STATUS.md     # maintained by Antigravity
    artifacts/    # optional evidence, test outputs and handoff notes
```

Start by copying `templates/USER_TASK_PLAN.md` to `work_items/<task-id>/PLAN.md` and filling in the requirements and acceptance criteria. Create `STATUS.md` from `templates/STATUS.md`.

The Main Agent must not edit the user's PLAN.md during execution. If a material plan change is necessary, it proposes the exact delta and waits for the user's decision. Routine progress, test evidence, worker IDs and blockers belong in STATUS.md and GSD state.

Use a short, stable task ID (for example, `skill-trigger-fix`). Do not put private secrets or credentials in a task plan.

---
name: "Work Ledger"
description: "Use when substantial multi-run project work needs durable scope, plan, state, decisions, and validation in .work-ledger/."
---
# Work Ledger

Track substantial work across runs and handoffs in `.work-ledger/`. Open or resume an entry when work spans runs, touches several subsystems, needs a handoff, or already has an entry. Skip obvious single-run edits, read-only investigation, conversation, and memory-only updates. If small work grows, open an entry then.

## Select the entry

Read `.work-ledger/index.md`, creating it if absent. Resume the entry matching the user's request or create the next `NNN-short-slug`; never start an unrelated backlog item. Keep only one entry `in-progress`. Before switching, mark the prior entry `open` and record where it stopped.

Place each entry in `.work-ledger/<id>/`. Each entry has `scope.md`, `plan.md`, and `state.md`. Add `decisions.md` when a choice matters and `validation.md` when checking completion. Keep an index row with id, title, state, and updated date.

- `scope.md`: goal, boundary, and explicit exclusions.
- `plan.md`: small steps with a check for each.
- `state.md`: dated progress, remaining work, and blockers; update it and the index row at the end of each run.
- `decisions.md`: one short record per consequential choice or pivot: decision, reason, alternative considered, evidence pointer, and result. Add a new record when a later finding changes the decision.
- `validation.md`: commands or observations, their results, and review evidence. Link to proof artifacts when useful.

Use `open` for queued or paused work, `in-progress` for active work, `completed` only after validation is recorded, and `closed` for abandoned or superseded work with a reason. Keep work on the current entry; record unrelated work as a new `open` entry.

If the ledger is outside write permissions, report the blocker. Put reusable lessons in memory; the ledger records the work itself.

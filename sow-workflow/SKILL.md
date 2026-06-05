---
name: "SOW Workflow"
description: "Use when project work needs durable Statements of Work in .clai/sow/ with scope, plan, state, decisions, and validation."
---
# SOW Workflow

Use this workflow when project work needs an auditable Statement of Work across runs or handoffs.

## Trigger

Run this workflow for any task that will modify project files.

It does not run for:

- pure conversation, explanation, or design discussion,
- read-only investigation with no file edits,
- memory-only updates inside `.clai/memory/`.

If it is unclear whether the user request will produce edits, ask once. Do not open a SOW for clarification rounds.

## On every triggering run

1. Read `.sow/index.md`, creating it if missing. Note which SOWs are `open`, `in-progress`, or `completed`.
2. If a SOW is `in-progress`, resume it. Only one SOW may be in progress at a time.
3. If none is in progress, pick the highest-priority `open` SOW and transition it to `in-progress`, timestamping the transition in its `state.md`.
4. Modify project files only in service of the current SOW.
5. If newly discovered work does not fit the current SOW, do not silently expand scope. Draft a new SOW in `open`, then return to the current one.
6. End each run by updating the current SOW's `state.md` with what changed, what remains, and any blockers.

## States

- `open`: defined but not started.
- `in-progress`: active; only one at a time.
- `completed`: implementation done and validation recorded.
- `closed`: archived or superseded.

## Layout

```text
.sow/
├── index.md
└── <sow-id>/
    ├── scope.md       # what and why
    ├── plan.md        # how
    ├── state.md       # current status, updated every run
    ├── decisions.md   # noteworthy choices
    └── validation.md  # evidence of completion
```

## Completion gate

Mark a SOW `completed` only when `validation.md` lists evidence, such as commands run, tests passed, manual checks completed, or review outcomes.

## Blocked states

If `.sow/` is unavailable or outside the current write permissions, report the blocker instead of silently skipping SOW updates.

## Boundary with memory

Memory is what was learned. SOWs are what work exists. Put plans, status, decisions, and validation in SOW files; put reusable learned heuristics in memory.

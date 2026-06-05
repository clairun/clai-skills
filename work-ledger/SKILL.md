---
name: "Work Ledger"
description: "Use when substantial multi-run project work needs a durable ledger entry in .work-ledger/ with scope, plan, state, decisions, and validation."
---
# Work Ledger

Use this workflow to keep an auditable record of substantial work across runs and handoffs.

The ledger is a set of living work records. Write terse operational notes: what is in scope, what the plan is, what happened, what remains.

## Trigger

Open or resume a ledger entry when the task meets any of:

- it will plausibly span more than one run,
- it touches multiple subsystems or many files,
- the user asks for a plan, tracking, or a handoff,
- it continues work that already has a ledger entry.

Do not open an entry for:

- single-run edits with an obvious scope, such as typo fixes, one-file changes, or mechanical renames,
- pure conversation, explanation, or design discussion,
- read-only investigation with no file edits,
- memory-only updates inside `.clai/memory/`.

If a small task grows past the threshold mid-run, open the entry at that point. For borderline tasks, skip the ledger: a missing entry costs less than ceremony on every edit.

## Selecting the entry

The user's request drives selection, never the backlog:

1. Read `.work-ledger/index.md`, creating it from the template below if missing.
2. If the request continues an existing entry, resume it.
3. If the request is new work, create a new entry and set it `in-progress`.
4. If a different entry is `in-progress`, pause it first: set it back to `open` and record where it stopped and why in its `state.md`. Only one entry is `in-progress` at a time.
5. Do not start open entries the user did not ask about. Mention the open backlog when relevant; never work it unprompted.

## On every run with an active entry

1. Modify project files only in service of the current entry.
2. If newly discovered work does not fit, do not silently expand scope: add a new `open` entry for it, then return to the current one.
3. End each run by appending to the current entry's `state.md`: what changed, what remains, any blockers. Update its row in `index.md`.

## States

- `open`: defined but not started, or paused.
- `in-progress`: active; only one at a time.
- `completed`: implementation done and validation recorded.
- `closed`: abandoned or superseded; record the reason in `state.md`.

Transitions: `open → in-progress`, `in-progress → open` (paused), `in-progress → completed`, `in-progress → closed`, `open → closed`.

## Identifiers and layout

Entry ids are `NNN-short-slug` with a zero-padded sequence number from `index.md`, such as `004-fix-auth-redirect`.

```text
.work-ledger/
├── index.md
└── <entry-id>/
    ├── scope.md       # created at open
    ├── plan.md        # created at open
    ├── state.md       # created at open, appended every run
    ├── decisions.md   # created at first noteworthy decision
    └── validation.md  # created at completion
```

## File contents

- `index.md`: one table row per entry.

  ```markdown
  # Work Ledger

  | id | title | state | updated |
  |----|-------|-------|---------|
  | 001-example | Short title | in-progress | 2026-06-05 |
  ```

- `scope.md`: what and why in a few sentences, plus an explicit out-of-scope list.
- `plan.md`: ordered steps, each independently verifiable.
- `state.md`: dated entries, newest first; each entry covers what changed, what remains, and blockers.
- `decisions.md`: one entry per noteworthy choice: the decision, alternatives considered, and the reason.
- `validation.md`: evidence of completion: commands run with results, tests passed, manual checks, review outcomes.

## Completion gate

Mark an entry `completed` only when `validation.md` lists concrete evidence. A plan finished without recorded validation stays `in-progress` or `open`.

## Blocked states

If `.work-ledger/` is unavailable or outside the current write permissions, report the blocker instead of silently skipping ledger updates.

## Boundary with memory

Memory is what was learned. The ledger is what work exists. Put plans, status, decisions, and validation in ledger files; put reusable learned heuristics in memory.

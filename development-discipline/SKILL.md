---
name: "Development Discipline"
description: "Use when implementing code changes with focused scope, local consistency, concise comments, and practical validation."
---
# Development Discipline

Use this skill when editing or creating code. The goal is to ship a small, readable change that fits the existing codebase.

## Work style

- Read the surrounding code before editing. Follow local patterns, naming, error handling, and test style unless they are actively harmful.
- Keep the diff scoped to the requested behavior. Do not mix unrelated refactors, formatting churn, or speculative cleanup into the change.
- Prefer clear code over explanatory scaffolding: better names, simpler control flow, and smaller helpers should carry most of the meaning.
- Add tests in proportion to risk. Cover the behavior that matters to users or callers, not incidental implementation details.
- Before handing off, inspect the final diff as a human reviewer would: remove churn, stale notes, dead paths, and accidental complexity.

## Comment Policy

- Default to no comment. Write one only for intent, an invariant, or an external constraint that cannot be recovered from the code, in the shortest form that carries it.
- Do not add comments that restate names, types, branches, loops, assignments, or obvious control flow.
- Do not narrate every step of new code. If a comment is needed to make simple code understandable, first try to make the code clearer.
- Keep comments short and tied to the code they explain. Avoid historical essays, future plans, and implementation diaries in source files.
- State a rule once. Copies drift, and a long explanatory block is a signal to simplify the rule instead of documenting it harder.
- Never assert in a comment what you have not verified, and never cite scratch paths or another repo's line numbers.
- When editing nearby code, delete or update stale comments. Leave useful existing comments intact.

## Validation

- Run the narrowest meaningful checks that prove the change works.
- If a check is unavailable, too expensive, or delegated to CI, say that directly with the residual risk.

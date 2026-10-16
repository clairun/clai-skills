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
- Add tests in proportion to risk. Test observable behavior: a wrong result or missing effect should fail, while an internal refactor should pass.
- Before handing off, inspect the final diff as a human reviewer would: remove churn, stale notes, dead paths, and accidental complexity.

## Design

- Validate external input at its boundary and pass typed values inward. Avoid repeated guards for states internal types already exclude.
- Model states and ownership explicitly when branches or synchronized booleans grow. Keep simpler local code when it already expresses the rule.
- For operations that can retry or resume, check what happens after a duplicate call or partial prior run.

## Comments

Default to none. A comment earns its place when it carries intent, an invariant, or an external constraint that the code can't express; keep it short and next to the code it explains. If simple code seems to need a comment, try clearer code first. State a rule once: copies drift, and a long explanatory block usually means the rule should be simpler. Don't assert what you haven't verified, and don't cite scratch paths or another repo's line numbers. When editing nearby code, update or delete stale comments and leave useful ones alone.

## Validation

- Run the narrowest meaningful checks that prove the change works.
- If a check is unavailable, too expensive, or delegated to CI, say that directly with the residual risk.

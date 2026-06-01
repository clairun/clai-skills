---
name: "Delegation"
description: "Use when work can be split across independent collaborators, tools, or review passes, then integrated into one coherent result."
---
# Delegation

Use delegation when independent work can happen in parallel or when a separate perspective materially improves quality. Delegation may mean assigning work to another person, another agent, a specialized tool, or a separate review pass.

## Delegate

Delegate tasks that are separable and easy to verify:

- read-only investigation with a clear scope,
- independent hypothesis checks,
- code or design review,
- summaries of separate subsystems,
- data gathering that does not block the next local step.

## Keep local

Keep work local when the next step depends on the result immediately, when the task is tightly coupled, or when the final decision requires weighing tradeoffs across multiple inputs.

## Dispatch shape

Every delegated task should include:

- a concrete scope, such as `audit error handling in src-tauri/src/assistant/`, not `look at the code`,
- the expected output shape, such as findings, verdict, summary, or file list,
- constraints the assignee needs, such as read-only mode, no network, or no edits outside a module.

Prefer structured tools or interfaces when available because they produce outputs that are easier to validate. Dispatch independent tasks in parallel. Use sequential delegation only when one result feeds the next task.

## Integrate, don't paste

Verify important delegated claims against evidence before relying on them. Reconcile conflicts and produce one coherent result for the user, not a stitched transcript of delegated replies.

For reviews, a review verdict is input to judgment, not a substitute for it. Validate material findings and decide what to accept, reject, or fix.

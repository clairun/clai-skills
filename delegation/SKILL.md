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

## Lifecycle

After delegating a task to an agent, you own it until it reaches a terminal status.

- Do not send a final answer or end the main run while any delegated task is still `queued` or `running`.
- Poll delegated task ids with the available task-result tool until each is `completed`, `failed`, or `blocked`.
- If a running task's progress is unclear, inspect its session history or tool-call transcript when the platform exposes the task `sessionId` or `runId`; use that evidence to decide whether it is progressing, stalled, hung, or blocked.
- If a delegated task fails, blocks, or appears stalled, report that state from evidence instead of inventing a result.

## Integrate, don't paste

Verify important delegated claims against evidence before relying on them. Reconcile conflicts and produce one coherent result for the user, not a stitched transcript of delegated replies.

For reviews, a review verdict is input to judgment, not a substitute for it. Validate material findings and decide what to accept, reject, or fix.

## Self-reports about the delegate's own run

A delegate's account of what *it* did is a claim, not an observation, and its errors run
self-flattering: it will assert it verified something it never measured.

This matters for the claims you are about to act on or repeat to the user as fact, such as "the tests
pass", "I stayed inside that module", or "I checked every call site". Settle those with the cheapest
evidence available — the diff, the files, or rerunning the check yourself. Reading the delegate's full
transcript usually costs more than redoing the check, so go to `sessionId`/`runId` only when the claim
is about process and left no artifact behind. If a claim would not change your next action, spend
nothing on it.

When writing the task, prefer constraints that leave a checkable artifact.

## Failed delegations

When a delegated task times out, errors, or returns output that does not match the requested shape:

- retry once with a sharper scope and explicit output shape,
- if it fails again, do the work locally or report it as not done; never fill the gap with guesses,
- never integrate unverifiable claims because a delegate asserted them confidently.

If two delegates contradict each other on a material point, resolve it with direct evidence rather than majority or confidence.

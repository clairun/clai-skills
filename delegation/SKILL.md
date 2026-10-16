---
name: "Delegation"
description: "Use when work can be split across independent collaborators, tools, or review passes, then integrated into one coherent result."
---
# Delegation

Delegate when independent work can run in parallel or when a separate perspective improves quality: scoped investigations, hypothesis checks, reviews, summaries of separate subsystems, data gathering. Keep work local when the next step needs the result right away, when it is tightly coupled, or when the decision means weighing several inputs together.

## How it works in CLAI

Agents talk to each other through tasks. You assign a task to a workspace agent and it runs in its own session, in parallel with yours. You don't need to wait: when the task finishes, fails, or blocks, its result arrives in your session as an event and wakes you. You can carry on with other work or end your turn in the meantime. You can send messages to a running task, and a task can report back more than once. The task-result tool is there for a closer look at a task whose progress seems unclear: it exposes the session and run ids you can use to read its transcript.

## A good task

A good dispatch has a concrete scope (`audit error handling in src-tauri/src/assistant/`, not `look at the code`), the shape of output you expect (findings, verdict, summary, file list), and whatever constraints the worker needs (read-only, no network, stay inside one module). The worker doesn't see your conversation, so include the context it needs. Independent tasks can go out together; sequence them only when one result feeds the next.

## Integrating results

Delegated claims are input, not conclusions. Verify the ones that matter, reconcile conflicts with direct evidence rather than confidence or majority, and produce one coherent result rather than a stitched transcript of replies. A review verdict is the same: validate the findings and decide what to accept, reject, or fix.

When a task fails, times out, or returns something other than what you asked for, retry once with a sharper scope. If that fails too, do the work yourself or report it as not done. Report failures and stalls from evidence; don't fill gaps with guesses.

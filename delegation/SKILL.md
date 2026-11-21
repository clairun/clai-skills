---
name: "Delegation"
description: "Use when another agent's role covers the work, or when work can be split across independent collaborators, tools, or review passes, then integrated into one coherent result."
---
# Delegation

When the workspace has an agent whose role covers a kind of work, that work is theirs by default; if your own role covers it, it is yours. When you route the work, assign it to them, including small changes and fixes to what they delivered. If two roles cover it, the author of the work comes first, except for reviewing that work, which goes to someone other than its author. Do another role's work yourself only when the round trip would cost more than the work is worth. Then say so, and ask that agent to review what you changed, not to redo it.

Inside a task assigned to you, the work your instructions give you is yours: do it, and do not hand it on unless they ask. Say in your reply whose role covers any part that belongs to someone else, so they can review it.

Beyond that, delegate when independent work can run in parallel or when a separate perspective improves quality: scoped investigations, hypothesis checks, reviews, summaries of separate subsystems, data gathering. Keep work that belongs to no one's role local when the next step needs the result right away, when it is tightly coupled, or when the decision means weighing several inputs together.

## How it works in CLAI

Agents talk to each other through tasks. You assign a task to a workspace agent and it runs in its own session, in parallel with yours. You don't need to wait: when the task finishes, fails, or blocks, its result arrives in your session as an event and wakes you. You can carry on with other work or end your turn in the meantime. You can send messages to a running task, and a task can report back more than once. The task-result tool is there for a closer look at a task whose progress seems unclear: it exposes the session and run ids you can use to read its transcript.

## A good task

A good dispatch has a concrete scope (`audit error handling in src-tauri/src/assistant/`, not `look at the code`), the shape of output you expect (findings, verdict, summary, file list), and whatever constraints the worker needs (read-only, no network, stay inside one module). The worker doesn't see your conversation, so include the context it needs. Independent tasks can go out together; sequence them only when one result feeds the next.

## Integrating results

Delegated claims are input, not conclusions. Verify the ones that matter, reconcile conflicts with direct evidence rather than confidence or majority, and produce one coherent result rather than a stitched transcript of replies. A review verdict is the same: validate the findings and decide what to accept, reject, or fix.

When work an agent delivered later fails a check because of that work (a review finding, an automated check, a user correction), send the failure back to the task that produced it. The message reopens that task with its history, so the author fixes its own mistake and can record what it learnt. If the task cannot take a message, assign the same agent a new task with the failure. Inside a task assigned to you, put a failure in work you did not assign in your final reply instead. A flaky check is not the author's failure, and a changed requirement is new work. If the work fails the same check again after going back, stop and tell the user (inside a task, in your final reply) instead of fixing it yourself.

When a task fails to run, times out, or returns something other than what you asked for, retry once with a sharper scope. If that fails too, report it as not done. Do the work yourself only as the exception at the top allows, with the same review request. Report failures and stalls from evidence; don't fill gaps with guesses.

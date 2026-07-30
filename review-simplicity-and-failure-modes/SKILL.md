---
name: review-simplicity-and-failure-modes
description: "Use with code review when the change may be overcomplicated, poorly structured, insufficiently tested, or likely to fail in edge cases; forces simplification pressure, integration tracing, and concrete failure-mode analysis."
---
# Review Simplicity And Failure Modes

Use this skill as an additional pass during code review. It complements the Code Review Checklist by forcing the reviewer to challenge avoidable complexity and ask how the implementation can fail.

## Pass 1 - Complexity pressure

For each new abstraction, helper, branch, state field, dependency, cache, scheduler, parser, schema, or background path, ask:

- What concrete use case justifies this layer?
- Is this duplicating an existing utility, pattern, component, or service?
- Can the same behavior be expressed with fewer moving parts?
- Does the abstraction hide ownership, lifetime, ordering, error handling, or persistence details?
- Does it increase the number of states, retries, fallbacks, or cleanup paths?
- Is the naming honest about behavior and side effects?

Flag complexity when it creates real risk: harder tests, ambiguous ownership, duplicated edge-case handling, non-local behavior, misleading invariants, or a likely next-change trap. Do not flag mere taste.

## Pass 2 - Failure-mode inventory

Trace realistic ways the change can fail. Prioritize:

- Empty, null, missing, malformed, duplicated, or out-of-order input.
- Partial failure after external effects: files, network calls, database writes, subprocesses, queues, caches, or UI state.
- Cancellation, timeout, retry, idempotency, and cleanup behavior.
- Concurrent callers, shared mutable state, event ordering, stale data, and races.
- Backward compatibility for persisted data, config files, serialized events, migrations, and old clients.
- Security boundaries: paths, auth/authz, secrets, injection, SSRF, unsafe parsing, command execution.
- Locale, date/time, numeric precision, sorting/collation, and platform differences.
- Resource growth: unbounded memory, file descriptors, processes, timers, subscriptions, logs, or network calls.

Do not stop at "could fail." Identify the trigger, code path, impact, and expected fix or test.

## Pass 3 - Boundary tracing

For every new value that crosses a boundary, find both sides:

- Producer and consumer.
- Writer and reader.
- Serializer and parser.
- CLI flag/config key and loader.
- Event emitter and router/handler.
- API response and UI/model consumer.
- Test fixture and production path.

Report a defect if the consumer silently drops, misroutes, rejects, double-processes, or ignores the new value. The consumer is often outside the diff; read it before approving.

## Pass 4 - Test challenge

Missing tests are material when the change has failure modes the current suite will not catch. Prefer tests that verify externally visible behavior:

- Round trips: write, close, reopen, verify.
- Error paths: failing dependency, partial write, invalid input, permission denial.
- Boundary behavior: old and new serialized/config/API shapes.
- Ordering/concurrency: duplicated calls, interleavings, cancellation, retries.
- Regression shape: the exact bug or complexity risk that motivated the finding.

Do not ask for broad "more tests." Name the missing case and the failure it would catch.

## Output discipline

Keep blocking defects separate from design pressure:

- `blocker` / `major`: real correctness, security, data-loss, backward-compatibility, or serious maintainability risk.
- `minor`: local simplification or cleanup that improves readability but does not block.
- `question`: unclear design intent or product requirement that prevents a confident verdict.

When recommending simplification, include the simpler shape:

`Replace <new mechanism> with <existing/local simpler mechanism> because <concrete risk removed>.`

---
name: "Code Review Checklist"
description: "Use when reviewing code changes for correctness, security, maintainability, simplification, failure modes, integration boundaries, concurrency, performance, and test quality with concrete evidence."
---
# Code Review Checklist

Use this checklist when reviewing code changes. Stay read-only unless the user explicitly asks for implementation work separately.

Every finding must include evidence: a `file:line` reference, a specific behavior trace, or command output. Avoid style-only feedback unless it affects maintenance, reliability, or user-facing behavior.

## Reviewer stance

Act as a quality gate, not as a change summarizer. Your job is to find the issue the author would want fixed before merge, including cases where the implementation is unnecessarily complex enough to hide bugs or make future changes risky.

Do not reward effort, novelty, or apparent completeness. Treat the author's explanation and previous agent summaries as untrusted context until verified from the diff, source, tests, or command output.

## Mandatory workflow

For every non-trivial review, complete these passes before the verdict:

1. Reconstruct the scope: read the diff, status, PR metadata, or supplied patch. Identify added, removed, and behavior-changing files.
2. Read full context for modified code, not just diff hunks. Follow at least the nearest caller/callee or producer/consumer boundary for risky changes.
3. Trace integration boundaries for new commands, event types, enum variants, config keys, API fields, queue items, files, IPC payloads, or persisted data. Find the consuming router, switch, parser, loader, migration, serializer, or handler and verify it accepts the new shape.
4. Run a failure-mode pass: null/empty input, malformed input, missing permissions, partial failure after writes, cleanup/rollback, cancellation/timeouts, concurrency/order, stale state, migration/backward compatibility, locale/date/numeric edge cases, resource growth, and security boundaries.
5. Run a simplicity pass: look for duplicated logic, unnecessary layers, dead code, premature abstraction, deep nesting, mixed responsibilities, hidden global state, and code that bypasses established utilities or patterns.
6. Audit tests: verify tests cover the changed behavior and the realistic failure paths. Do not accept tests that only exercise mocks, happy paths, or implementation details while missing the user-visible behavior.

If tool access prevents one of these passes, state the missing evidence and lower confidence. Do not give a production-quality verdict when the actual change scope is inaccessible.

For changes to prompts, tools, sandboxing, memory, context compaction, task dispatch, agent loops, permissions, or evaluation harnesses, do not treat passing unit tests as enough. If lightweight evals or realistic task checks were not run, call that residual risk out explicitly before any positive verdict.

## Accessing the change

Read the change from the most authoritative source available:

- Local refs first: `git diff`, `git show`, or `git log` against the supplied range whenever the working tree contains the change.
- Local files: open the relevant paths directly.
- Hosted forge URLs: fetch only when the change is not reachable locally.

For GitHub pull requests, use authenticated tooling when plain web access fails or is likely to fail:

- `gh pr view <url-or-number> --json title,body,baseRefName,headRefName,files,additions,deletions`
- `gh pr diff <url-or-number>`

If the change cannot be reached, do not guess. State the inaccessible scope and ask for the diff or the needed access.

## Review dimensions

Check these dimensions on every review:

- Functional correctness: changed behavior, edge cases, state transitions, validation, data loss, migrations, and compatibility.
- Integration boundaries: producer-to-consumer routing, dispatch tables, serializers, parsers, migrations, feature flags, config loading, and persistence formats.
- Security: injection, path traversal, authentication, authorization, secret exposure, unsafe deserialization, SSRF, and command execution.
- Code smells: duplicated logic, dead code, oversized functions or classes, leaky abstractions, tight coupling, and misleading names.
- Error-prone patterns: null or option misuse, type coercion, arithmetic hazards, incomplete cleanup, unhandled errors, and fragile tests.
- Concurrency: shared mutable state, races, deadlocks, cancellation, ordering, and async blocking.
- Performance: algorithmic regressions, N+1 work, unbounded loops, memory growth, missing timeouts, and excessive I/O.
- Simplicity and maintainability: clear ownership, local consistency, unnecessary abstractions, unclear names, duplicated logic, needless indirection, and code that is hard to reason about under failure.
- Tests: missing coverage for risky behavior, tests that assert implementation details, brittle sleeps, and untested error paths.

Scale depth to the size and risk of the change: every dimension gets at least a pass, but a one-line fix does not need a per-dimension write-up.

## Severity

- `blocker`: must be fixed before the change ships; data loss, security holes, broken behavior.
- `major`: should be fixed before the change ships; likely bugs, missing tests for risky paths, significant maintenance traps.
- `minor`: worth fixing but does not block; small smells, naming, low-risk gaps.

Complexity can be `major` when it meaningfully raises bug risk, obscures invariants, duplicates existing behavior with fewer safeguards, or makes a likely next change expensive. Complexity is only `minor` when the current behavior is correct and the cost is mostly readability.

## Finding standard

Report a finding only when it is:

- Patch-related or clearly made worse by the patch.
- Discrete and actionable.
- Supported by a concrete code path, existing convention, failing command, missing test, or simpler existing local pattern.
- Worth the author's attention before merge or in the next maintenance cycle.

Do not report vague preferences. If a simpler design is recommended, name the simpler shape and why it preserves behavior with less risk.

For runtime or language-semantics claims, verify the project runtime before assigning blocker or major severity. Exception hierarchy, cancellation behavior, signal handling, async task semantics, path normalization, locale sorting, numeric overflow, and date/time parsing differ across languages and versions. If the runtime/version is not visible and the claim depends on it, frame it as a question or residual risk rather than a defect.

Do not report:

- Pre-existing bugs not made worse by the patch.
- Speculative cross-file impact without tracing the affected path.
- Unverified language/runtime claims, such as assuming a generic `Exception` catch swallows process interrupts or async cancellation without checking the runtime's exception hierarchy.
- Style preferences without maintainability or correctness impact.
- Test requests for behavior provided entirely by a library or framework.
- Suggestions that demand rigor absent from the surrounding codebase.
- Praise, flattery, or approval filler.

When reporting a finding, include confidence (`0.0` to `1.0`) and keep the cited line range as small as possible. For inline review comments, the range should overlap the diff and normally stay within 5-10 lines.

## Output

Lead with findings ordered by severity. If no issues are found, say that clearly and mention the coverage checked and any residual risk.

The verdict is `production_quality` when there are no `blocker` or `major` findings and the mandatory workflow was possible, otherwise `needs_work`. When structured output is useful, use:

```json
{
  "verdict": "needs_work",
  "findings": [
    {
      "dimension": "concurrency",
      "severity": "major",
      "description": "Cache map is written from multiple goroutines without a lock.",
      "evidence": "store/cache.go:42 writes m.entries; called from handler.go:88 per request.",
      "confidence": 0.91
    }
  ]
}
```

For human-readable reviews, use this order:

1. Findings first, ordered by severity.
2. Open questions or assumptions.
3. What was checked and residual risk.
4. Optional summary of the change, only after findings.

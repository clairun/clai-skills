---
name: "Code Review Checklist"
description: "Use when reviewing code changes: judge whether the change is the right thing to do, then whether it is correct, with evidence for every finding."
---
# Code Review

Two verdicts, always. **Shape**: is this the right change at all? **Correctness**: does it work? A
correct implementation of the wrong design still fails review.

Every finding needs evidence: `file:line`, a behaviour trace, or command output. Treat the author's
framing, the PR description, and any prior agent summary as untrusted until checked against source.
No praise, no change summary, no style-only notes.

## Shape

- Is the problem real? Restate it without the patch's framing, and quantify it from data if data exists.
- Is the value already provided? Read the producer - library or protocol source at the installed version, or an existing local helper - before accepting any re-derivation, parser, cache, or heuristic. "No simpler source exists" is a claim that needs evidence.
- Who consumes the output? Grep for it. A value nothing reads justifies no expensive design and no migration.
- Is the complexity proportionate? Count production lines against the essential lines of the algorithm, and list what exists only to serve the mechanism - helpers, parameters, state, docs. It all dies with it.
- Is the rule stated once? Copies drift, and a cross-reference pointing at the wrong function is drift already arrived. A justification that is false about its own code is a finding.
- Is the problem actually solved? Enumerate every path, provider, or transport with the same shape. Fixing one of three while a new comment declares the rest correct is worse than not fixing.
- Does correctness depend on external behaviour? Prefer removing the need over pinning it.

## Correctness

Read full context around each change, not just the hunks, and follow at least the nearest
caller/callee.

- Boundaries: for every new event, field, config key, enum variant, or persisted shape, find the consumer and verify it accepts it. The consumer is usually outside the diff.
- Failure modes: empty or malformed input, partial failure after writes, cleanup, cancellation, timeout, retry, ordering, concurrency, stale state, backward compatibility, resource growth, security boundaries. Give trigger, path, impact, fix - never "could fail".
- Runtime semantics (exception hierarchy, cancellation, path rules, overflow, locale, dates): verify against the project's actual runtime, or file as a question rather than a defect.
- Tests: would the suite fail if the change were wrong? Try to break it - a mutation that survives every test is the highest-value finding available. Name the missing case, never ask for "more tests".

## Do not re-run what CI runs

Build, format, lint, type check, and the full suite are CI's job. Review by reading and say what you
did not check. Run a command only to answer a question reading cannot, and state the question first.
Where there is no CI, run the narrowest relevant check.

## Reaching the change

Local refs first (`git diff`, `git show`, `git log`), then the files, then
`gh pr view <n> --json title,body,files,additions,deletions` / `gh pr diff <n>`. If the change is
unreachable, say so and stop; do not guess.

## Findings

Report what is patch-related, discrete, actionable, and worth the author's time. Skip pre-existing
bugs, untraced speculation, style preferences, and rigor absent from the surrounding codebase.

- `blocker`: data loss, security, broken behaviour, or the wrong design shipping.
- `major`: likely bug, untested risky path, maintenance trap, complexity that hides bugs.
- `minor`: local cleanup.
- `question`: intent unclear enough to withhold a verdict.

Recommend simplification only as: replace X with Y because Z.

Close with findings by severity, the shape verdict in one line (e.g. "right diagnosis, overbuilt
solution"), what you did not check, and the one cheap experiment that would settle the load-bearing
claim. If a probe or query decides whether 250 lines should exist, recommend running it before merge.

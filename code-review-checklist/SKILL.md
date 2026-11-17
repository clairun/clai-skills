---
name: "Code Review Checklist"
description: "Use when reviewing a change, validating someone's theory, or confirming work is done: judge whether it is the right thing to do, then whether it is correct, verifying independently with evidence for every finding."
---
# Code Review

Two verdicts: **shape** (is this the right change at all?) and **correctness** (does it work?). A
correct implementation of the wrong design still fails review.

Every finding comes with evidence: `file:line`, a behaviour trace, or command output. Praise, change
summaries, and style-only notes don't help the author.

## Verify independently

The author's framing, the PR description, a prior agent's summary, or a theory handed to you are
context, not facts. Verification means re-deriving the claim, not re-reading it. Re-run the specific
command whose output they cite (that one, not the surrounding suite) and compare it with the claim.
Reproduce the behaviour or bug before accepting that it exists or is fixed. Test the negation: look
for the case that would make the theory false. Check what the explanation doesn't mention: adjacent
paths, error branches, callers outside the described scope. Stay read-only unless asked to fix things.
If you wrote, simplified, or wrote the tests for the change, your review is a self-check, not an
independent one.

## Shape

- Is the problem real? Restate it without the patch's framing, and quantify it from data if data exists.
- Is the value already provided? Read the producer - library or protocol source at the installed version, or an existing local helper - before accepting any re-derivation, parser, cache, or heuristic. "No simpler source exists" is a claim that needs evidence.
- Who consumes the output? Grep for it. A value nothing reads justifies no expensive design and no migration.
- Is the complexity proportionate? Count production lines against the essential lines of the algorithm, and list what exists only to serve the mechanism - helpers, parameters, state, docs. It all dies with it.
- Is the rule stated once? Copies drift, and a cross-reference pointing at the wrong function is drift already arrived. A justification that is false about its own code is a finding.
- Is the problem actually solved? Enumerate every path, provider, or transport with the same shape. Fixing one of three while a new comment declares the rest correct is worse than not fixing.
- Does correctness depend on external behaviour? Prefer removing the need over pinning it.
- Does it follow precedent? Read the most recent changes of the same kind in the repo. A safeguard they use and this one lacks needs a reason.

## Correctness

Read full context around each change, not just the hunks, and follow at least the nearest
caller/callee.

- Boundaries: for every new event, field, config key, enum variant, or persisted shape, find the consumer and verify it accepts it. The consumer is usually outside the diff.
- Blast radius: follow callers and serialized or external consumers beyond symbol search. Identify the one fact the change depends on for safety; prove it with source, a focused check, or a live run, or mark it unproven.
- Failure modes: empty or malformed input, partial failure after writes, cleanup, cancellation, timeout, retry, stale state, backward compatibility, resource growth, security boundaries. Give trigger, path, impact, fix - never "could fail".
- Ordering and concurrency: for each lock, row, file, or other shared resource the change takes, write down the order it acquires them and compare it with every other path touching the same resources while it runs, including implicit ones symbol search misses: cascades, triggers, hooks, handlers. Acquiring a stronger hold on something you already hold is an ordering problem too.
- Reverse paths: rollback, down migrations, undo, and feature-flag-off paths get the same review as the forward path.
- Runtime semantics (exception hierarchy, cancellation, path rules, overflow, locale, dates): verify against the project's actual runtime, or file as a question rather than a defect.
- Tests: would the suite fail if the change were wrong? Try to break it - a mutation that survives every test is the highest-value finding available. Name the missing case, never ask for "more tests". A test built to confirm your model can't find what the model misses: for concurrency, run the real conflicting operation alongside the change rather than a stand-in.

Go through every item for this change before concluding; most won't apply, and deciding that is
quick. Each that applies needs evidence even when it comes up clean: an item read as a heading is
where the next reviewer finds the bug. Don't stop at the first plausible findings.

## CI's job and yours

Build, format, lint, type check, and the full suite are CI's job. Review by reading and say what you
did not check. A command is worth running when it answers a question reading cannot; state the
question first. Where there is no CI, run the narrowest relevant check.

## Reaching the change

Local refs first (`git diff`, `git show`, `git log`), then the files, then
`gh pr view <n> --json title,body,files,additions,deletions` / `gh pr diff <n>`. If the change is
unreachable, say so rather than guessing.

## Findings

Report what is patch-related, discrete, actionable, and worth the author's time. Pre-existing bugs,
untraced speculation, style preferences, and rigor absent from the surrounding codebase are not.

- `blocker`: data loss, security, broken behaviour, or the wrong design shipping.
- `major`: likely bug, untested risky path, maintenance trap, complexity that hides bugs.
- `minor`: local cleanup.
- `question`: intent unclear enough to withhold a verdict.

Recommend simplification only as: replace X with Y because Z.

Close with findings by severity, the shape verdict in one line (e.g. "right diagnosis, overbuilt
solution"), what you did not check, including applicable items you couldn't verify, and the one cheap
experiment that would settle the load-bearing claim. If a probe or query decides whether 250 lines
should exist, recommend running it before merge.

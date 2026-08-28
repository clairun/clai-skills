---
name: "Iterative Review"
description: "Use before publishing, releasing, deploying, or otherwise shipping code; require independent review, fix validated findings, and repeat until clear."
---
# Iterative Review

Use this loop before publishing, releasing, deploying, or otherwise shipping code.

## Trigger

Run the loop before any of these actions:

- `git push`, `gh pr create`, `gh pr ready`, `gh pr merge`, or branch publication,
- deploy, release, or publish commands such as `make release`, `cargo publish`, or `npm publish`,
- marking implementation work as shipped or production-ready.

The loop does not trigger for read-only investigation, planning, explanation, or local commits that are not being published.

## Review loop

1. Identify the best available independent review mechanism: a human reviewer, code-review tool, CI review, review agent, or equivalent.
2. Prepare the full change for review. Prefer a git range such as `main..HEAD` or a complete file list. Do not narrow the scope to only the files edited most recently.
3. Request review over the full change. Ask for a verdict plus concrete findings with evidence. If the repo has CI, say so in the request and tell the reviewer to run nothing CI already covers — no builds, formatters, linters, or test suites — and to review the code itself. CI reports on its own schedule; do not hold the review for it. For high-risk changes, use at least two independent perspectives when available, asked separately: one for correctness bugs, one for whether this is the right change at all.
4. For each finding, verify the cited evidence. Reject false positives with a concise reason and fix validated issues.
5. Re-run the same review mechanism over the original full scope after fixes. Do not ask reviewers to check only the fixes. Carry forward the list of rejected false positives so they are not re-litigated each round.
6. Repeat until the final verdict is production-quality or no material issues remain.
7. Only then perform the triggering publish, release, deploy, or ship action.

For reviewing code changes, the Code Review Checklist skill defines the dimensions, severities, and verdict vocabulary.

## Termination

The loop is bounded. If new material findings keep appearing after three full rounds, stop looping: that signals a problem deeper than review can fix. Summarize validated findings, applied fixes, and unresolved issues, then ask the user how to proceed instead of shipping or looping further.

## Blocked states

Do not ship if review is required but unavailable, times out, or returns malformed output. Report the specific blocker and the safest next step.

If the user asks to skip the loop while this skill is selected, state that the skill requires review before shipping and ask them to remove or replace the skill if they want a different policy.

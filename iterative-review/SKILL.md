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
3. Request review over the full change. Ask for a verdict plus concrete findings with evidence.
4. For each finding, verify the cited evidence. Reject false positives with a concise reason and fix validated issues.
5. Re-run the same review mechanism over the original full scope after fixes. Do not ask reviewers to check only the fixes.
6. Repeat until the final verdict is production-quality or no material issues remain.
7. Only then perform the triggering publish, release, deploy, or ship action.

## Blocked states

Do not ship if review is required but unavailable, times out, or returns malformed output. Report the specific blocker and the safest next step.

If the user asks to skip the loop while this skill is selected, state that the skill requires review before shipping and ask them to remove or replace the skill if they want a different policy.

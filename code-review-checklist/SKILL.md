---
name: "Code Review Checklist"
description: "Use when reviewing code changes for correctness, security, maintainability, concurrency, performance, and test quality with concrete evidence."
---
# Code Review Checklist

Use this checklist when reviewing code changes. Stay read-only unless the user explicitly asks for implementation work separately.

Every finding must include evidence: a `file:line` reference, a specific behavior trace, or command output. Avoid style-only feedback unless it affects maintenance, reliability, or user-facing behavior.

## Accessing the change

Read the change from the most authoritative source available:

- Local refs first: `git diff`, `git show`, or `git log` against the supplied range whenever the working tree contains the change.
- Local files: open the relevant paths directly.
- Hosted forge URLs: fetch only when the change is not reachable locally.

For GitHub pull requests, use authenticated tooling when plain web access fails or is likely to fail:

- `gh pr view <url-or-number> --json title,body,baseRefName,headRefName,files,additions,deletions`
- `gh pr diff <url-or-number>`
- `gh api repos/{owner}/{repo}/pulls/{number}/files`
- `gh auth status`

If the change cannot be reached, do not guess. State the inaccessible scope and ask for the diff or the needed access.

## Review dimensions

Check these dimensions on every review:

- Functional correctness: changed behavior, edge cases, state transitions, validation, data loss, migrations, and compatibility.
- Security: injection, path traversal, authentication, authorization, secret exposure, unsafe deserialization, SSRF, and command execution.
- Code smells: bloaters, object-orientation abusers, change preventers, dispensables, couplers, and obfuscators.
- Error-prone patterns: null or option misuse, type coercion, arithmetic hazards, incomplete cleanup, unhandled errors, and fragile tests.
- Concurrency: shared mutable state, races, deadlocks, cancellation, ordering, and async blocking.
- Performance: algorithmic regressions, N+1 work, unbounded loops, memory growth, missing timeouts, and excessive I/O.
- Maintainability: clear ownership, local consistency, unnecessary abstractions, unclear names, and duplicated logic.
- Tests: missing coverage for risky behavior, tests that assert implementation details, brittle sleeps, and untested error paths.

## Output

Lead with findings ordered by severity. If no issues are found, say that clearly and mention the coverage checked and any residual risk.

When structured output is useful, use:

```json
{
  "verdict": "production_quality",
  "findings": []
}
```

Use `needs_work` when there is any blocker or major issue. Findings should include `dimension`, `severity`, `description`, and `evidence`.

---
name: "Codebase Quality Audit"
description: "Use when auditing a whole repository or subsystem for concrete, high-value refactoring and quality improvements, producing a prioritized report and refactoring sequence without changing code."
---
# Codebase Quality Audit

Use this skill to audit an existing codebase for concrete, high-value improvements and produce a prioritized report. This is a whole-repo (or whole-subsystem) analysis, not a review of a single change: for reviewing a diff or pull request, use the Code Review Checklist skill instead.

The goal is to identify improvements worth making, not to rewrite the codebase or impose personal style. When performing this audit, do not modify code; produce the report only. Implementation is a separate, explicitly-requested task and is out of scope for this pass. Every finding must carry evidence: a `file:line` reference and quoted code.

## Phase 1 — Understand before critiquing

Do this before any critique, and summarize it in 5–10 lines:

- Map the repo: entry points, main modules, dependency direction, build and test setup.
- Identify the existing architectural intent and conventions. Existing conventions win over your preferences unless they are actively harmful.
- Assess test coverage: it determines how safe refactoring is.

If something is ambiguous, for example dead code versus code used by an external consumer, flag it as a question rather than assuming.

## Phase 2 — Analyze

Examine the code across the dimensions defined in the Code Review Checklist skill (correctness, security, code smells, error-prone patterns, concurrency, performance, maintainability, tests), applied to the whole codebase rather than a change. Prioritize:

- Correctness risks: error-handling gaps, races, resource leaks, invariants that can be violated.
- Code smells: duplicated logic, long functions doing multiple things, god modules, deep nesting, primitive obsession, shotgun surgery, feature envy, inappropriate intimacy between modules, dead code.
- Design issues: wrong dependency direction, leaky abstractions, missing seams for testing, mixed levels of abstraction, business logic tangled with I/O or framework code.
- Pattern opportunities: only where a pattern reduces complexity for a problem the code actually has, such as strategy to replace a growing switch. Never introduce a pattern speculatively.
- Consistency: naming, module layout, and error-handling style diverging within the repo.

For every finding provide:

- Location: file path and line range.
- Evidence: the specific code demonstrating the issue, quoted.
- Impact: what concretely goes wrong or gets harder — bugs, change cost, testability.
- Proposed fix: specific enough that another engineer could implement it.
- Effort and risk: S/M/L effort, and whether tests currently protect the change.
- Severity: `high` / `medium` / `low`.

Severity here rates improvement value, not ship-blocking, so it uses `high`/`medium`/`low` rather than the Code Review Checklist's change-gating vocabulary.

## Phase 3 — Report

Produce a report with:

- Executive summary: the top 5 issues by value-to-effort ratio.
- Findings grouped by category, ordered by severity.
- A suggested refactoring sequence: order changes so each step is small, independently verifiable, and keeps the build green. Mark which steps are blocked on adding tests first.
- A short "leave it alone" list: things that look smelly but are not worth touching, and why.

## Rules

- This pass is analysis only: report findings, do not edit code. If the user wants fixes, that is a separate task run without this skill's read-only constraint.
- Prefer deleting code over adding code. Prefer simple over clever.
- Do not recommend new dependencies, frameworks, or layers of abstraction unless a specific, existing pain point demonstrates the payoff.
- A long but linear, readable function is not automatically a smell. Flag it only if it mixes concerns or resists testing.
- Calibrate to the repo's size and maturity: a script collection and a production service warrant different standards.
- If you find fewer real issues than expected, say so. Do not pad the report.

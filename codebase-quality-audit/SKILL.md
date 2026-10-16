---
name: "Codebase Quality Audit"
description: "Use when auditing a whole repository or subsystem for concrete, high-value refactoring and quality improvements, producing a prioritized report and refactoring sequence without changing code."
---
# Codebase Quality Audit

A whole-repo (or whole-subsystem) analysis that produces a prioritized report of improvements worth making. It is read-only: implementation is a separate task. For a single change or PR, the Code Review Checklist skill is the right tool.

## Understand first

Before critiquing, map the repo in 5–10 lines: entry points, main modules, dependency direction, build and test setup, the architectural intent and conventions, and how much test coverage protects a refactor. Existing conventions win over your preferences unless they are actively harmful. When something is ambiguous (dead code, or code used by an external consumer?), flag it as a question rather than assuming.

## Analyze

Apply the Code Review Checklist dimensions to the whole codebase, looking for:

- correctness risks: error-handling gaps, races, resource leaks, invariants that can be violated;
- code smells: duplicated logic, functions doing several things, god modules, deep nesting, primitive obsession, shotgun surgery, feature envy, dead code;
- design issues: wrong dependency direction, leaky abstractions, missing seams for testing, business logic tangled with I/O or framework code;
- pattern opportunities, only where a pattern reduces complexity for a problem the code actually has;
- consistency: naming, layout, and error-handling style diverging within the repo.

Each finding carries a `file:line` location with the code quoted, the concrete impact (bugs, change cost, testability), a fix specific enough for someone else to implement, an S/M/L effort estimate with whether tests protect the change, and a `high`/`medium`/`low` value rating.

## Report

Lead with the top five issues by value-to-effort. Group the rest by category, ordered by severity. Suggest a refactoring sequence of small, independently verifiable steps that keep the build green, marking the ones that need tests first. Close with a short "leave it alone" list: things that look smelly but aren't worth touching, and why.

Prefer deleting code over adding it, simple over clever. Don't recommend new dependencies or layers of abstraction without a specific existing pain point that pays for them. A long but linear, readable function isn't a smell by itself. Calibrate to the repo's size and maturity, and if you find fewer real issues than expected, say so rather than padding.

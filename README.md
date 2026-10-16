# CLAI Skills

Curated, read-only skills for CLAI users.

Each skill lives in its own directory and exposes a `SKILL.md` file with
frontmatter metadata plus concise task guidance. CLAI discovers these folders
directly from the repository checkout.

Skills give direction, not mandates: they describe task knowledge and
workflow discipline, and explain how CLAI works where that matters (such as
how delegated tasks report back) so agents can use their own judgement.

## Catalog

| Skill | Use when |
|-------|----------|
| [code-review-checklist](code-review-checklist/SKILL.md) | Reviewing a code change: whether it is the right change at all, then whether it is correct. |
| [codebase-quality-audit](codebase-quality-audit/SKILL.md) | Auditing a whole repository or subsystem for high-value refactoring and quality improvements, producing a prioritized report. |
| [delegation](delegation/SKILL.md) | Splitting separable work across collaborators, tools, or review passes. |
| [development-discipline](development-discipline/SKILL.md) | Implementing code changes with focused scope, clear boundaries, and behavior-focused validation. |
| [iterative-review](iterative-review/SKILL.md) | Shipping code: require independent review, fix validated findings, repeat until clear. |
| [performance-measurement](performance-measurement/SKILL.md) | Measuring and reporting performance changes without mistaking errors, skipped work, or noise for gains. |
| [project-verification](project-verification/SKILL.md) | Creating or maintaining a project-local guide to exercise real user behavior. |
| [root-cause-debugging](root-cause-debugging/SKILL.md) | Reproducing a failure, tracing its first wrong state, and verifying the fix. |
| [self-reflection](self-reflection/SKILL.md) | Finishing non-trivial work: THINK, WRITE, REFLECT, REVISE before handing off. |
| [unbiased-second-opinion](unbiased-second-opinion/SKILL.md) | Reviewing or validating someone else's theory: treat prior framing as untrusted. |
| [work-ledger](work-ledger/SKILL.md) | Substantial multi-run work that needs durable scope, plan, decisions, state, and validation records. |

The review skills compose: `code-review-checklist` is the review itself,
`unbiased-second-opinion` governs reviewing work framed by someone else, and
`iterative-review` wraps the review in a fix-and-re-review loop before
shipping. `codebase-quality-audit` reuses the same finding dimensions but
targets a whole codebase for proactive improvement rather than gating a
specific change.

Ideas for the debugging, measurement, verification, and review guidance were adapted from [pstack](https://github.com/cursor/plugins/tree/main/pstack/skills) (MIT License, copyright 2026 Lauren Tan).

# CLAI Skills

Curated, read-only skills for CLAI users.

Each skill lives in its own directory and exposes a `SKILL.md` file with
frontmatter metadata plus concise task guidance. CLAI discovers these folders
directly from the repository checkout.

Skills should stay architecture agnostic: they describe task knowledge and
workflow discipline, not a specific runtime topology.

## Catalog

| Skill | Use when |
|-------|----------|
| [code-review-checklist](code-review-checklist/SKILL.md) | Reviewing code changes across correctness, security, concurrency, performance, and tests. |
| [delegation](delegation/SKILL.md) | Splitting separable work across collaborators, tools, or review passes. |
| [iterative-review](iterative-review/SKILL.md) | Shipping code: require independent review, fix validated findings, repeat until clear. |
| [self-reflection](self-reflection/SKILL.md) | Finishing non-trivial work: THINK, WRITE, REFLECT, REVISE before handing off. |
| [unbiased-second-opinion](unbiased-second-opinion/SKILL.md) | Reviewing or validating someone else's theory: treat prior framing as untrusted. |
| [work-ledger](work-ledger/SKILL.md) | Substantial multi-run work that needs durable scope, plan, state, and validation records. |

The review skills compose: `code-review-checklist` defines what a single
review pass checks, `unbiased-second-opinion` governs reviewing work framed
by someone else, and `iterative-review` wraps either in a fix-and-re-review
loop before shipping.

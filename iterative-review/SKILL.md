---
name: "Iterative Review"
description: "Use before publishing, releasing, deploying, or otherwise shipping code: get an independent review, fix validated findings, and repeat until clear."
---
# Iterative Review

Before code goes out (a push, a PR, a release, a deploy, or calling work shipped), get an independent review and act on it. Local commits, investigation, and planning don't need this.

## The loop

Find the best independent reviewer available: a person, a review agent, a review tool. Give them the whole change (a git range such as `main..HEAD`, or the complete file list), not just the files you touched last. Ask for a verdict and concrete findings with evidence. If the repo has CI, say so and ask the reviewer to read the code rather than rerun builds, linters, and test suites; CI reports on its own schedule. For risky changes, two separate perspectives help: one on correctness, one on whether this is the right change at all.

Check each finding against its evidence. Fix the real ones; set aside false positives with a short reason. Then review the full scope again, carrying the rejected list forward so it isn't re-litigated. Ship when the verdict is production quality or nothing material remains.

The Code Review Checklist skill defines the dimensions, severities, and verdict vocabulary.

## When it doesn't converge

If material findings keep appearing after about three rounds, the problem is deeper than review can fix. Stop, summarize what was found, fixed, and left open, and ask the user how to proceed. If review is unavailable or returns nothing usable, say so and suggest the safest next step rather than shipping unreviewed.

---
name: "Iterative Review"
description: "Use before publishing, releasing, deploying, sending, or otherwise shipping work, code or not: get an independent review, fix validated findings, and repeat until clear."
---
# Iterative Review

Before work goes out (a push, a PR, a release, a deploy, a report or document delivered to the user or outside the workspace, or calling work shipped), get an independent review and act on it. A task's reply to the agent that assigned it is not going out: that agent has it reviewed before it goes further. Local commits, drafts, investigation, and planning don't need this.

## The loop

Find the best independent reviewer available: a person, a review agent, a review tool. Independent means the reviewer starts without the context that wrote, simplified, or tested the change: a session reviewing its own work confirms its own model. A new task has none of that context, even when it goes to the agent that made the change; prefer an agent whose role is reviewing when there is one. A new task with only the change and what it was built from (the repo, or a document's sources and data), asked to find why it breaks or misleads, is a good default. Give them the whole change (a git range such as `main..HEAD`, the complete file list, or the whole document), not just the part you touched last. Ask for a verdict and concrete findings with evidence. If the repo has CI, say so and ask the reviewer to read the code rather than rerun builds, linters, and test suites; CI reports on its own schedule. For risky changes, two separate perspectives help: one on correctness, one on whether this is the right change at all.

Check each finding against its evidence. Fix the real ones, or when another task did the work, send them back to it. Set aside false positives with a short reason. Then review the full scope again, carrying the rejected list forward so it isn't re-litigated. Every revision counts as a new change, including simplifications and changes the user asked for: restructuring code can introduce failure modes the previous round never saw. Ship when no blocker, major or question is open. Ask the user what to do about the minor ones.

The Code Review Checklist skill defines the dimensions, severities, and verdict vocabulary. For work other than code, read *broken behaviour* as a wrong figure, claim or conclusion, and *likely bug* as a likely error or misleading statement, and ask the reviewer to rate each finding that way.

## When it doesn't converge

If material findings keep appearing after about three rounds, the problem is deeper than review can fix. Stop, summarize what was found, fixed, and left open, and ask the user how to proceed. If review is unavailable or returns nothing usable, say so and suggest the safest next step rather than shipping unreviewed.

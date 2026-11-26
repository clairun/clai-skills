---
name: "Project Planning"
description: "Use when turning a goal into a plan before implementation starts: split the work into small deliverables and their dependencies, build them in order, and settle what only the user can decide."
---
# Project Planning

Plan before implementation when the goal holds several deliverables that can each be reviewed separately, or needs more than one worker. Skip it for a change that can be built and checked as one unit. The output is a written plan the user has approved.

## Deliverables and dependencies

A unit is one change reviewed by itself. Plan the work as units of two kinds.

- **Deliverables.** Each is one behaviour someone can see working, small enough to review in one sitting and to release without the ones after it.
- **Dependencies.** What must exist before a deliverable can be built: a stored field, an interface, a change to a shared component. A dependency has no visible result of its own; if it has one, it is a deliverable. Its review asks whether existing behaviour is safe and whether it gives its deliverable what it needs.

List the deliverables in build order and, under each, the dependencies it is the first to need. That one list is the build sequence: each dependency lands immediately before the first deliverable that needs it, and a dependency no deliverable names is not built. A dependency built next to its consumer is shaped by a real need; one built early is shaped by a guess.

Exercise each deliverable for real before starting the next; a passing test is not that. Every step then leaves a working system, and what you learn can still change the rest.

Give a substantial algorithm (a parser, a state machine) its own unit. Settle what the user will see before a unit starts: a change of mind after review reopens the review.

## Settle during planning

Do not open with a questionnaire. A user can rarely answer in the abstract, and answers given before seeing a design do not hold. Draft the plan with your best answer to each of these, taken from the request and the existing system, mark it as an assumption, and let the user correct it:

- the first release: the fewest deliverables that are useful without the rest;
- what must never happen, especially what data leaves the system and to whom;
- who merges, releases, and publishes;
- where each deliverable can be exercised for real.

Ask the user to confirm the data assumption explicitly; a wrong guess there is the expensive one. If an answer changes later, that is a plan change: update the plan and every unit it affects.

## The plan

One document: the goal and first release; hard constraints, each as a rule a reviewer can check; the design, with claims about the existing system checked against its source; decisions and who made them; the deliverables with their dependencies, in build order; open questions and who decides each; what is not verified.

When assigning a unit, include the hard constraints that apply to it: a worker sees its task, not the plan. The Delegation skill covers writing those tasks, Iterative Review the review loop, Project Verification real-world checks, and Work Ledger keeping state once work begins.

## Ready to implement

Start when the user has approved the plan and confirmed the data assumption, and no open question blocks the first unit. Otherwise say what is missing and ask.

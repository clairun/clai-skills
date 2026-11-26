---
name: "Project Planning"
description: "Use when turning a goal into a plan before implementation starts: split the work into small deliverables and their dependencies, build them in order, and settle what only the user can decide."
---
# Project Planning

Plan before the first implementation task when the work needs more than one change or more than one worker. The output is a written plan the user has approved.

## Ask first

- What is the smallest release you would use, and what comes after it?
- What must never happen? Ask what data may leave the system, and to whom.
- Who may merge, release, and publish text other people will read?
- Where can this be exercised for real?

## Deliverables and dependencies

Plan the work as two lists.

- **Deliverables.** Each is one behaviour someone can see working, small enough to review in one sitting and to release without the ones after it.
- **Dependencies.** What must exist before a deliverable can be built: a stored field, an interface, a change to a shared component. Each changes nothing a user sees and is its own unit, so its review asks only whether existing behaviour is safe.

Record which deliverable needs which dependency. A dependency no deliverable names is not built.

Build in deliverable order, and land each dependency immediately before the first deliverable that needs it. A dependency built next to its consumer is shaped by a real need; one built early is shaped by a guess.

Exercise each deliverable for real before starting the next. Every step then leaves a working system, and what you learn can still change the rest.

Give a self-contained algorithm (a parser, a state machine) its own unit. Settle what the user will see before a unit starts: a change of mind after review reopens the review.

## The plan

One document: the goal and first release; hard constraints, each as a rule a reviewer can check; the design, with claims about the existing system checked against its source; decisions and who made them; the two lists in build order; open questions and who decides each; what is not verified.

Repeat each hard constraint in every unit's instructions: a worker sees its task, not the plan. The Delegation skill covers writing those tasks, Iterative Review the review loop, Project Verification real-world checks, and Work Ledger keeping state once work begins.

## Ready to implement

Start when the user has approved the first release and the constraints, and no open question blocks the first unit. Otherwise say what is missing and ask.

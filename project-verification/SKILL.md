---
name: "Project Verification"
description: "Use when a project needs a reusable way to launch and exercise its real UI, CLI, service, or app behavior, or when that verification guide has drifted."
---
# Project Verification

Create or maintain a project-local verification guide. Read the repo's run instructions and existing tests first; use its harness when possible. Write concrete commands and selectors from this project, not placeholders.

The guide must answer:

- **Launch:** How to start an isolated usable instance and tell when it is ready; include teardown.
- **Doctor:** How to check the instance, version, dependencies, and access before driving it.
- **Drive:** How to perform a user-visible action with stable commands, routes, prompts, or selectors.
- **Evidence:** What proves the action and its result, including visible state and external effects; where the proof is saved.
- **Cleanup:** How to stop only what this run started and preserve the proof.

Keep a short feature map: for each important user path, record how to reach it, the action, and the observable success state. Check source for drift, then exercise the path in a live instance. If a path is unreachable, record the exact prerequisite. Treat a product regression as a product issue, not a reason to weaken the guide.

Run the guide's launch, doctor, one feature, evidence capture, and cleanup once before calling a new guide ready. For maintenance, check every mapped feature against source and exercise each live; report what was verified and what could not run.

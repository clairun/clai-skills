---
name: "Unbiased Second Opinion"
description: "Use when asked to review, validate a theory, or confirm completion; treat prior framing as untrusted context and verify independently."
---
# Unbiased Second Opinion

When someone asks you to review their work, validate their theory, or confirm something is done, their explanation is context, not truth. Look at the code, data, and command output yourself, and stay read-only unless asked to fix things.

Verification means re-deriving the claim, not re-reading it. Re-run the specific command whose output they cite (that one, not the surrounding suite; builds and full test runs belong to CI) and compare it with the claim. Reproduce the behaviour or bug before accepting that it exists or is fixed. Test the negation: look for the case that would make the theory false. Check what the explanation doesn't mention: adjacent paths, error branches, callers outside the described scope.

Start from the supplied scope and follow dependencies and affected callers wherever the change reaches. Report only what you can support; if you find nothing, say what you checked and what risk remains. For code changes, use the Code Review Checklist vocabulary: `production_quality` or `needs_work`.

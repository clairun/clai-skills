---
name: "Unbiased Second Opinion"
description: "Use when asked to review, validate a theory, or confirm completion; treat prior framing as untrusted context and verify independently."
---
# Unbiased Second Opinion

Use this skill when asked to review, validate a theory, or confirm that work is done.

## Ground rules

- Treat the requester's explanation as context, not truth.
- Inspect the code, data, and command output yourself.
- Stay read-only unless explicitly asked to implement fixes.
- Prefer direct evidence over summaries.
- If the prompt embeds a theory, verify it or reject it with reasoning.

## Verifying independently

Verification means re-deriving the claim, not re-reading it:

- Re-run the specific command whose output the requester cites, and compare it against the claim. Re-run that command only, not the surrounding suite: build, lint, and full-suite verification belong to CI.
- Reproduce the claimed behavior or bug yourself before accepting that it exists or that it is fixed.
- Test the negation: look for the case that would make the theory false, not only examples that fit it.
- Check what the explanation does not mention: adjacent code paths, error branches, and callers outside the described scope.

## Review behavior

Start from the supplied scope, then follow dependencies and affected callers as needed. Do not limit the review to highlighted files if the change has broader effects.

Report only issues you can support. If no issue is found, say what was checked and any residual risk. For code changes, use the verdict and severity vocabulary from the Code Review Checklist skill: `production_quality` or `needs_work`.

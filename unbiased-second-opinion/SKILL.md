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

## Review behavior

Start from the supplied scope, then follow dependencies and affected callers as needed. Do not limit the review to highlighted files if the change has broader effects.

Report only issues you can support. If no issue is found, say what was checked and return a production-quality verdict or equivalent clear conclusion.

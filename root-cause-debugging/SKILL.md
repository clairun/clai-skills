---
name: "Root Cause Debugging"
description: "Use when diagnosing a bug or recurring failure: reproduce it, trace the first wrong state, fix the cause, and verify the real behavior."
---
# Root Cause Debugging

1. State the expected behavior, the observed failure, and a small way to reproduce it. Read the actual error and state before forming a theory.
2. Trace backward from the symptom through producers and boundaries until you find the first wrong decision or missing handling of a legitimate state. Instrument the path when evidence is missing.
3. Decide whether the failing state is valid. If valid, handle it where the contract requires. If invalid, fix its producer. A guard, retry, or swallowed error at the symptom is not a root-cause fix.
4. Search for the same fault pattern in sibling paths, including persistence and restart paths when relevant.
5. Verify the reproduction fails before the fix and succeeds after it when practical. Otherwise use the closest real check and state what remains unverified.

If two fixes based on the same assumption fail, write down that assumption and test it before another edit. Keep the final report to the cause, evidence, fix, and verification.

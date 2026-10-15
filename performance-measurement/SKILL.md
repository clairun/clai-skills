---
name: "Performance Measurement"
description: "Use when measuring or reporting a speedup, regression, throughput, or latency change, especially to choose between implementations."
---
# Performance Measurement

Before measuring, write the claim and inspect what the harness times. Use realistic data and the production build and settings for each side. Capture a baseline before the change.

Check the result before trusting it:

- Did the intended work run inside the timed region? Check the output or side effect, not only elapsed time.
- Did any request or operation fail, retry, time out, or return a partial result? Count those separately.
- What limits throughput: CPU, lock, disk, network, dependency, or load generator? Measure the limiter during a separate profiling run.
- Is the claimed gain possible given the changed step's share of total time? Measure the user-visible end-to-end path too.
- Does the difference reproduce? Alternate sides under similar load, report the run count and spread, and call overlapping results inconclusive.

One run can support a labeled ballpark estimate, never a choice between options. Report the metric and unit, workload, versions and settings, run count, spread, limiter, error count, and whether the end-to-end result changed. If a side was untuned or the work was not confirmed, do not declare a winner.

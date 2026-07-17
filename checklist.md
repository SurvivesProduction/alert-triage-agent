# Production Error-Handling Checklist

The checklist I run against any automation before it touches production. If you can't check a box, that's where your 3am page is coming from.

## Failure handling
- [ ] Every external call (API, LLM, webhook) has retry logic with a delay/backoff — a hiccup is not an outage
- [ ] Retry counts are bounded — unlimited retries on an LLM call is a money printer for your provider
- [ ] A step that fails after all retries routes to an error path — never a dead end
- [ ] The error path notifies a human in a channel someone actually reads
- [ ] Failure notifications include: workflow name, failed step, input that caused it, timestamp

## Visibility
- [ ] The workflow logs each execution somewhere queryable (table, sheet, DB) — not just the tool's execution history
- [ ] A heartbeat/scheduled check alerts when the workflow has been silent longer than expected
- [ ] You can answer "did it run last night?" in under 60 seconds

## Blast radius
- [ ] Test/simulated data cannot reach production channels (separate Slack channels, separate credentials)
- [ ] The workflow can be disabled in one click without breaking anything downstream
- [ ] Restart after a failure is deliberate (human re-enables) — automatic recovery hides root causes

## Cost
- [ ] You know roughly what one execution costs, and what 1,000 executions cost
- [ ] Anything loop-shaped has a circuit breaker (max iterations / max spend)

---
*From the Survives Production channel — https://www.youtube.com/@survivesproduction*

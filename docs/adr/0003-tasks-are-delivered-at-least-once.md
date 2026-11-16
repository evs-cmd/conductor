---
status: accepted
date: 2026-11-16
---

# 3. Tasks are delivered at least once, and workers must be idempotent

## Context

Workers poll for tasks over the network and can crash, hang or lose their connection at any point: before any work,
after a side effect, or before reporting the result. The engine can't tell a slow worker from a dead one, or a lost
update from a task that never ran.

Source: [Durable Execution Semantics](../architecture/durable-execution.md), "Task delivery guarantees" and "Failure
matrix".

## Decision

Every task is delivered at least once, never exactly once.

- A scheduled task sits in a persistent queue until a worker polls it.
- A task whose worker sends no update within `responseTimeoutSeconds` is put back in the queue and delivered again.
- A task reported `FAILED` is retried according to `retryCount`, `retryLogic` and `retryDelaySeconds`. One reported
  `FAILED_WITH_TERMINAL_ERROR` isn't retried.
- Each state change is persisted before the next action.

## Consequences

- No task is silently lost, and workers need no retry logic of their own.
- A task can run more than once, for example after a worker crashes between a side effect and its update. Workers
  must be idempotent, or detect a redelivery, for instance from the task's `updateTime`.
- Timeouts and retries are configured per task definition, so each task's owner chooses its trade-off between
  redelivery delay and duplicate work.
- An engine-side exactly-once guarantee is ruled out. A future change toward one would need a new ADR superseding
  this one.

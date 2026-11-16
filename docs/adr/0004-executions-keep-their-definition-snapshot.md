---
status: accepted
date: 2026-11-16
---

# 4. A running execution keeps the definition snapshot taken when it started

## Context

Workflow definitions change while executions are running, and some executions wait for days on WAIT or HUMAN tasks
across several deploys. If an execution read its definition from the metadata store at each step, an update or a
deleted version would change or break executions already under way.

Source: [Durable Execution Semantics](../architecture/durable-execution.md), "What persists", "Failure matrix" and
"Workflow-level durability".

## Decision

When an execution starts, the engine persists a snapshot of the workflow definition with it, and the execution uses
only that snapshot from then on. It doesn't read the metadata store's current definition again.

- Updating a definition affects new executions only.
- Deleting a version doesn't affect executions already started from it.
- Several versions can run at the same time.
- A restart may opt in to the latest definition.

## Consequences

- Definitions can change with no downtime and no migration of running executions, and canary rollouts are possible.
- Every execution's record shows exactly which definition it ran, which supports replay and audit.
- A bug fixed in a definition doesn't reach executions already running. They have to be restarted with the latest
  definition, or finished as they are.
- Each execution stores a copy of its definition, which costs storage proportional to the number of executions.

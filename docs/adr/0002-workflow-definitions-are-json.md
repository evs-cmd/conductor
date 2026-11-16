---
status: accepted
date: 2026-11-16
---

# 2. A workflow definition is JSON, and JSON is its only runtime form

## Context

Workflows are written in several ways: JSON files, the SDKs in several languages, the UI and the API. If each kept
its own form, the server would need a compiled or language-specific representation, and a definition read back from
the API could differ from the one written.

Workflows also need to be replayable months later and generated at runtime, for example by a planner agent.

Source: [JSON + Code Native Workflow Orchestration](../architecture/json-native.md).

## Decision

A workflow definition is a JSON document, whatever wrote it. The SDKs convert code to that JSON and register it. The
server stores, versions and executes only the JSON, with no compiled intermediate form. The JSON written in a file is
the JSON the API accepts, the UI shows and the SDKs return.

A definition holds no executable code. Every side effect runs in a worker or a system task.

## Consequences

- Given the same inputs, the engine schedules the same tasks in the same order, which is what makes replay
  unconditional.
- A definition can be built at runtime and passed inline to `StartWorkflowRequest`, with no registration, compile or
  deploy.
- Definitions are diffable and belong in source control, and versions can be rolled back by registering an earlier
  document.
- Logic that must run during orchestration has to be a worker or a system task. Features that would put code in a
  definition are ruled out.
- Branch lists and plans generated at runtime, for example by an LLM, need validation and limits before they run,
  since the engine executes whatever valid JSON it is given.

# Architecture decision records

Each file here records one architecture decision Conductor made: what was decided, why, and what it costs. The pages
in [`docs/architecture/`](../architecture/) stay the full explanation; an ADR states the decision and links there.

| ADR | Decision | Status |
|---|---|---|
| [0001](0001-record-architecture-decisions.md) | Record architecture decisions | Accepted |
| [0002](0002-workflow-definitions-are-json.md) | A workflow definition is JSON, and JSON is its only runtime form | Accepted |
| [0003](0003-tasks-are-delivered-at-least-once.md) | Tasks are delivered at least once, and workers must be idempotent | Accepted |
| [0004](0004-executions-keep-their-definition-snapshot.md) | A running execution keeps the definition snapshot taken when it started | Accepted |

## Adding one

1. Copy the shape of an existing ADR into `NNNN-short-title.md`, with the next number.
2. Keep the front matter: `status` (`proposed`, `accepted`, `superseded` or `deprecated`) and `date` (`YYYY-MM-DD`).
3. Write three sections: **Context** (the forces, and the page or issue it comes from), **Decision** (what is
   decided, in the present tense) and **Consequences** (what it allows, what it rules out, and what it costs).
4. Add a row to the table above.

A decision that changes gets a new ADR. The old one stays, with `status: superseded` and a line naming the ADR that
replaces it.

---
status: accepted
date: 2026-11-16
---

# 1. Record architecture decisions

## Context

Conductor's design choices are explained across `docs/architecture/`, the developer guide and the reference. A reader
learns what the engine does, but not which choices were decisions, what they rule out, or what a change to one would
break. Those choices shape every contribution: a pull request that adds embedded code to a workflow definition, or
assumes a task runs exactly once, works against them without knowing it.

## Decision

Record each architecture decision as a short Markdown file in `docs/adr/`, numbered in order, in the MADR format:
front matter with `status` and `date`, then Context, Decision and Consequences.

The first decisions recorded (0002 to 0004) are ones Conductor already made and documents in `docs/architecture/`.
Each names the page it comes from, which stays the full explanation.

## Consequences

- A contributor can find the constraints a change must keep in one folder.
- A decision that changes gets a new ADR that supersedes the old one, and the old one stays, marked superseded.
- The pages in `docs/architecture/` remain the user-facing explanation. An ADR states the decision and its cost, and
  links there.

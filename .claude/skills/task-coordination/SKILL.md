---
name: task-coordination
description: Route task inputs to the appropriate agent, monitor execution state across
  multi-agent sequences, detect delegation cycles, and aggregate results. Coordinator
  only; does not author, review, or approve fachliche artifacts.
---
Canonical skill: SKL-010
Claude Code skill: task-coordination
Bound agents: AGT-001
Knowledge assets: KNW-004
Source: blueprint/catalogs/skills.yaml

# Task Coordination — Procedure

Use this skill to route a task to the correct agent under the phase model.

1. Determine the current phase from the `Approved-by-user:` markers in
   `.blueprint/design/requirements.md`, `ui-design.md`, and `architecture.md`.
2. Route the request to the agent responsible for the current phase; refuse shortcuts
   into later phases and explain why.
3. Surface conflicts (stale approvals, contradicting artifacts, missing preconditions)
   to the user as explicit decisions.
4. Record the routing decision and its rationale.

Coordination only — no authoring, reviewing, or approving.

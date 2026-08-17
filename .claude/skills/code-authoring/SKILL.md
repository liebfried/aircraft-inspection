---
name: code-authoring
description: Generate or modify source code within an explicitly defined write scope,
  following activated language profile rules. Hand off immediately to code review.
---
Canonical skill: SKL-003
Claude Code skill: code-authoring
Bound agents: AGT-004
Knowledge assets: KNW-009
Source: blueprint/catalogs/skills.yaml

# Code Authoring — Procedure

Use this skill to implement product code within the approved scope.

Precondition: `.blueprint/design/requirements.md`, `.blueprint/design/ui-design.md`,
and `.blueprint/design/architecture.md` all exist with `Approved-by-user:` markers.
Otherwise stop — no product code before all three approvals.

1. Implement using the approved stack, realising the accepted screens and flows, and
   satisfying the referenced requirement IDs.
2. Work in reviewable increments; after each increment produce a short implementation
   summary referencing the covered requirement IDs.
3. Hand off each increment to code review before continuing.
4. If the approved artifacts do not settle a materially visible decision, ask the user.
5. If implementation requires deviating from an approved artifact, stop and name the
   conflict; the affected phase needs renewed approval first.

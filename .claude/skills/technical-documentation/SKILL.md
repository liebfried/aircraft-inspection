---
name: technical-documentation
description: Draft or update technical documentation for an assigned artifact or system
  component. Documentation must reference canonical IDs without copying rule text.
  Requires human review before publication.
---
Canonical skill: SKL-007
Claude Code skill: technical-documentation
Bound agents: AGT-008
Knowledge assets: KNW-012
Source: blueprint/catalogs/skills.yaml

# Technical Documentation — Procedure

Use this skill to draft or update technical documentation.

1. Source content from the approved artifacts under `.blueprint/design/` and the
   current implementation; reference decisions instead of restating them.
2. Keep README/setup instructions, architecture overview, and API documentation in
   sync with the implementation increment that changed them.
3. Present substantial documentation changes to the user for review before treating
   them as final.

Documentation must not invent decisions that were never approved.

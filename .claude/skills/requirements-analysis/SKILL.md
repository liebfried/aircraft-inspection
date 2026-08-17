---
name: requirements-analysis
description: Analyse requirements inputs for completeness, detect ambiguity and contradictions,
  and trace each requirement to the applicable canonical rules and activated profiles.
  Produces a structured requirements analysis artifact.
---
Canonical skill: SKL-001
Claude Code skill: requirements-analysis
Bound agents: AGT-001
Knowledge assets: KNW-001, KNW-002, KNW-003
Source: blueprint/catalogs/skills.yaml

# Requirements Analysis — Procedure

Use this skill to produce the requirements baseline for the project.

1. Interrogate the user in focused batches until purpose, users, features, data,
   integrations, platforms, non-functional expectations, constraints, and out-of-scope
   items are all answered or explicitly recorded as open questions.
2. Write `.blueprint/design/requirements.md`:
   - one numbered, testable requirement per line item (REQ-001, REQ-002, ...)
   - acceptance criteria per feature requirement
   - open questions with owners
   - explicit out-of-scope list
3. Present the baseline and ask for the user's explicit approval.
4. Only after explicit approval, append the marker line `Approved-by-user: <date>`.

Never fill a gap with an assumption, and never approve the baseline yourself.

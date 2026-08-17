---
name: artifact-validation
description: Validate YAML artifacts against JSON-Schema contracts, check referential
  integrity, detect ID duplicates, and detect rule violations. Produce a validation
  report.
---
Canonical skill: SKL-008
Claude Code skill: artifact-validation
Bound agents: AGT-005
Knowledge assets: KNW-001, KNW-004, KNW-005
Source: blueprint/catalogs/skills.yaml

# Artifact Validation — Procedure

Use this skill to validate project artifacts structurally.

1. Validate YAML/JSON artifacts against their schemas where schemas exist.
2. Check `.blueprint/design/` artifacts for required structure: numbered requirement
   IDs, approval markers, ADR sections, screen-to-requirement references.
3. Check referential integrity: every referenced requirement ID exists; no duplicates.
4. Produce a validation report with per-finding location, expected, and found.

Never modify the artifacts under validation.

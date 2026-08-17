---
name: code-review
description: Systematically review code for correctness, rule compliance, and test
  coverage adequacy. The reviewing agent must not be the author of the code under
  review. Produce a finding record. Cannot approve or merge.
---
Canonical skill: SKL-004
Claude Code skill: code-review
Bound agents: AGT-005
Knowledge assets: KNW-001, KNW-009
Source: blueprint/catalogs/skills.yaml

# Code Review — Procedure

Use this skill to review an implementation increment independently of its author.

1. Confirm you did not author the code under review; if you did, stop and request an
   independent review pass.
2. Check requirement coverage (REQ-xxx references), architectural conformance,
   correctness, boundary error handling, security basics, readability, and test
   coverage adequacy.
3. Produce a finding record: severity (BLOCKER/MAJOR/MINOR), location, description,
   concrete suggestion — and an overall pass/blocked verdict.
4. Flag security-relevant findings for the security-analysis skill.

You do not approve or merge; the verdict informs the user's decision.

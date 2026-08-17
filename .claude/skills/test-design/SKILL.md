---
name: test-design
description: Design test cases and test strategies from requirements and implementation
  scope, aligned with PROF-018 (Testing) rules. Assess coverage adequacy. Produce
  a test specification and coverage assessment report.
---
Canonical skill: SKL-006
Claude Code skill: test-design
Bound agents: AGT-007
Knowledge assets: KNW-001, KNW-010
Source: blueprint/catalogs/skills.yaml

# Test Design — Procedure

Use this skill to derive test cases and a coverage assessment from the requirements.

1. Map every requirement ID to at least one test case; acceptance criteria become test
   cases one-to-one.
2. Cover happy paths, edge cases, error handling, and testable non-functional
   expectations.
3. Produce a test specification (name, preconditions, steps, expected result) that can
   be implemented directly.
4. Produce a coverage assessment: covered requirements, gaps, prioritised proposals.

Coverage adequacy is confirmed by the user.

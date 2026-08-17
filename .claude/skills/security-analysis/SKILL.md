---
name: security-analysis
description: Analyse code, configuration, or architecture for violations of activated
  security profile rules. Classify findings by severity. Produce a security finding
  record. Cannot close, downgrade, or approve its own findings.
---
Canonical skill: SKL-005
Claude Code skill: security-analysis
Bound agents: AGT-006
Knowledge assets: KNW-001, KNW-007
Source: blueprint/catalogs/skills.yaml

# Security Analysis — Procedure

Use this skill for a security-focused pass over code, configuration, or architecture.

1. Check input validation, injection risks, authentication and session handling,
   authorisation on every data access, secrets handling, dependency risks, personal
   data storage and retention, and transport security.
2. Classify each finding (BLOCKER/MAJOR/MINOR) with location, exploit scenario, and a
   concrete remediation proposal.
3. State explicitly that BLOCKER findings halt implementation and release until the
   user decides.

You cannot close, downgrade, or accept your own findings.

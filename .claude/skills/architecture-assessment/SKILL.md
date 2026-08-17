---
name: architecture-assessment
description: Assess an architecture proposal or ADR for compliance with activated
  architecture and security rules. Identify architectural risks. Produce a structured
  assessment artifact. Final architectural decisions remain with the human architect
  role.
---
Canonical skill: SKL-002
Claude Code skill: architecture-assessment
Bound agents: AGT-003
Knowledge assets: KNW-001, KNW-002, KNW-003, KNW-008
Source: blueprint/catalogs/skills.yaml

# Architecture Assessment — Procedure

Use this skill to produce the user-approved architecture for the project.

Precondition: `.blueprint/design/requirements.md` and `.blueprint/design/ui-design.md`
exist with `Approved-by-user:` markers. Otherwise stop and complete the earlier phases.

1. For each decision — language/framework, database (needed? which?), database hosting,
   application hosting and deployment, API style, authentication, third-party services —
   present 2-3 options with trade-offs and a recommendation, and let the user decide.
2. Check every decision against the approved requirements (especially non-functional)
   and the accepted UI design; surface conflicts instead of resolving them silently.
3. Record each decision ADR-style (context, options, decision, consequences) in
   `.blueprint/design/architecture.md`, referencing the requirement IDs it serves.
4. List architectural risks with severity and mitigations.
5. After the user's explicit approval of the complete architecture, append
   `Approved-by-user: <date>`.

Architectural decisions are made by the user; you assess and recommend.

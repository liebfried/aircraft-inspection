---
name: ui-design-and-mockups
description: Translate an approved requirements analysis into user interface concepts,
  wireframes, and reviewable mockup artifacts. Iterate on design variants based on
  explicit human feedback until the human accepts or rejects the design. Design acceptance
  and sign-off remain with the human role holder.
---
Canonical skill: SKL-011
Claude Code skill: ui-design-and-mockups
Bound agents: AGT-011
Knowledge assets: KNW-001
Source: blueprint/catalogs/skills.yaml

# UI Design and Mockups — Procedure

Use this skill to produce a user-accepted UI design from the approved requirements.

Precondition: `.blueprint/design/requirements.md` exists with an `Approved-by-user:`
marker. Otherwise stop and complete the requirements phase first.

1. Derive a screen inventory and navigation flow; map every screen to the requirement
   IDs it realises.
2. Create mockups under `.blueprint/design/mockups/` (self-contained HTML, SVG, or
   Markdown wireframes — openable without a build step).
3. Present the mockups, explain the layout rationale (including accessibility), and ask
   explicitly whether the design fits. Offer variants for key screens where reasonable.
4. Iterate on every piece of explicit user feedback and present the result again.
5. Write `.blueprint/design/ui-design.md`: screen inventory, flows, rationale, mockup
   links, open questions. After the user's explicit acceptance, append
   `Approved-by-user: <date>`.

Design acceptance is always the user's decision.

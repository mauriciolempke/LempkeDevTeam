---
name: arch-review-cr
description: Review a change request for architectural impact
---

Activate the **Architect** role. Read the agent definition from `agents/architect.md`.

1. Read `.devAgents/architect-notes.md` for current architecture context
2. List all CRs in `.devAgents/changes/` and identify unprocessed ones
3. For each unprocessed CR:
   a. Read the CR document
   b. Assess architectural impact: **No Impact** / **Minor Adjustment** / **Significant Change**
   c. If impact detected, explain what needs to change and why
   d. If significant change, recommend recreating the codebase from scratch
4. Update `architect-notes.md` with the assessment
5. If architecture changes are needed, update: architecture doc, rules files, guides as appropriate
6. Inform the user of the assessment and recommended actions

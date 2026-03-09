---
name: tl-review-cr
description: Check for new change requests and re-plan affected implementation tasks
---

Activate the **Tech Lead** role. Read the agent definition from `agents/tech-lead.md`.

1. Read `.devAgents/tech-lead-notes.md` for context
2. List all CRs in `.devAgents/changes/` and identify unprocessed ones
3. For each unprocessed CR:
   - Assess implementation impact
   - Identify affected milestones and tasks
   - Re-plan task board as needed
4. Update `.devAgents/tasks/task-board.md` with changes
5. Update `tech-lead-notes.md`
6. Report to user: what changed, what milestones are affected, new timeline

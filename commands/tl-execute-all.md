---
name: tl-execute-all
description: Execute all remaining milestones sequentially without pausing between them
---

Activate the **Tech Lead** role. Read the agent definition from `agents/tech-lead.md`.

Execute ALL remaining milestones sequentially, following the same process as `/tl-execute` for each one, but WITHOUT pausing between milestones for user approval.

1. Read task board and identify all remaining milestones
2. For each milestone in order:
   - Execute the full milestone flow (spawn subagents, review, test, commit)
   - If a milestone fails after one retry, STOP and report to user
3. After all milestones complete, present a full summary:
   - Milestones completed
   - Tasks completed per milestone
   - Any issues encountered and how they were resolved
4. Update task board and `tech-lead-notes.md`

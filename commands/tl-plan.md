---
name: tl-plan
description: Read all upstream artifacts and produce an implementation plan with milestones and task breakdown
---

Activate the **Tech Lead** role. Read the agent definition from `agents/tech-lead.md`.

1. Read `.devAgents/tech-lead-notes.md` to resume context (if exists)
2. Check `.devAgents/changes/` for unprocessed CRs — notify user if found
3. Read ALL upstream artifacts:
   - `.devAgents/prd.md`, `.devAgents/roadmap.md`, `.devAgents/backlog.md`
   - `.devAgents/architecture.md`, `.devAgents/tech-stack.md`
   - `.devAgents/ux/` — all UX specs
   - `.devAgents/analytics/tracking-plan.md`
   - `.devAgents/rules/` — all rules files
   - All briefings in `.devAgents/briefs/`
4. Break down work into **milestones** with concrete tasks
5. For each task: identify the subagent type, dependencies, files to touch, priority
6. Identify tasks that can run in parallel within each milestone
7. Write the plan to `.devAgents/tasks/task-board.md`
8. Update `tech-lead-notes.md`
9. Present the plan to the user for approval
10. Suggest: "Plan is ready. Use `/tl-execute` to execute the next milestone."

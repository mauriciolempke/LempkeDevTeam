---
name: tl-execute
description: Execute the next milestone by spawning developer subagents in parallel
---

Activate the **Tech Lead** role. Read the agent definition from `agents/tech-lead.md`.

1. Read `.devAgents/tech-lead-notes.md` and `.devAgents/tasks/task-board.md`
2. Identify the next milestone to execute
3. For all tasks in this milestone:
   a. Identify which can run in parallel
   b. Spawn subagents (frontend-developer, backend-developer, data-engineer) in parallel using the Agent tool
   c. Each subagent gets: task description, scope boundaries, relevant rules/specs, files to touch/not touch
   d. Collect results from all subagents
4. Once dev tasks complete:
   a. Spawn **code-reviewer** to review all code produced
   b. If issues found: send fixes back to the SAME dev subagent (preserving session)
   c. Re-verify with code-reviewer
5. Once code review passes:
   a. Spawn **qa-engineer** to run E2E and integration tests
   b. If failures found: send fixes back to appropriate dev subagent
6. If ALL tasks pass review and testing:
   - Commit the code (you are the ONLY agent that commits)
   - Update task board statuses to DONE
   - Report results to user
7. If a milestone CANNOT be fully completed:
   - Do NOT commit any code
   - Report the blocking issue to the user
   - Wait for guidance
8. After retry failure (one retry allowed): escalate to user
9. Update `tech-lead-notes.md`
10. Suggest: "Milestone complete. Use `/tl-execute` for the next milestone or `/tl-status` to review."

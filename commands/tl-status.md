---
name: tl-status
description: Show task board status — backlog, in-progress, done, and blocked tasks grouped by idea
---

Activate the **Tech Lead** role. Read the agent definition from `agents/tech-lead.md`.

1. Read `.devAgents/tech-lead-notes.md` and `.devAgents/tasks/task-board.md`.
2. Display the board grouped by idea (and CR for any local-only entries):
   - Per idea: idea name, feature name, current Solus status (`planned` / `ready_to_implement` / `in_progress` / `done`).
   - Per task: T#, name, subagent type, priority, dependencies, status (`backlog` / `in_progress` / `done` / `failed` / `blocked`).
3. Roll up totals: ideas by status, tasks by status.
4. Highlight any in-progress idea, currently running subagents (if any), and any blocked or failed items with the reason.
5. Check `.devAgents/changes/` for unprocessed CRs and surface them.
6. Suggest next action — `/tl-plan-ideas`, `/tl-execute`, `/tl-execute-all`, `/tl-review-cr`, or `/tl-get-defects` — based on the current state.

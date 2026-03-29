---
name: tl-review-cr
description: Check for new change requests and re-plan affected implementation tasks
---

Activate the **Tech Lead** role. Read the agent definition from `agents/tech-lead.md`.

1. Read `.devAgents/tech-lead-notes.md` for context

> **Scope:** The Tech Lead only plans and executes work that comes from Change Requests in `.devAgents/changes/`. Ideas and defects from Solus are **not** sourced here — defects are handled by `/tl-defects`, and ideas are the PM's responsibility.

## Step 1 — Find unprocessed CRs

List all files in `.devAgents/changes/` and identify any that are **not yet processed** (status ≠ Closed).

- If no unprocessed CRs exist, inform the user:
  ```
  No unprocessed change requests found in .devAgents/changes/.
  All CRs are closed. Use /tl-execute or /tl-execute-all to run pending milestones, or /tl-defects to work on accepted defects.
  ```
  Then stop.

## Step 2 — Process each unprocessed CR

For each unprocessed CR:
- Read the full CR document
- Assess implementation impact
- Decompose into milestones and tasks with clear descriptions, types, priorities, and dependencies

## Step 3 — Persist the plan

Update `.devAgents/tasks/task-board.md` with the full milestone and task breakdown.

**Also create the milestones and tasks in the Solus database** using the MCP server, under the idea referenced in the CR (look for `idea_id` in the CR document):

**For each milestone:**
```
create_milestone(
  idea_id: <cr_idea_id>,
  name: "M# — <milestone name>",
  description: <milestone description>,
  status: "not_started",
  target_date: <estimated date if known>
)
```

**For each task within that milestone** (use the returned milestone `id`):
```
create_task(
  milestone_id: <milestone_id>,
  project_id: DEFAULT_PROJECT_ID,
  name: "T#.# — <task name>",
  description: <task description>,
  type: "task",
  priority: "high",
  status: "backlog",
  position: <sequential integer>
)
```

Record all Solus IDs in the task board entry. If the CR has no `idea_id`, skip the Solus DB writes and only update the task board.

## Step 4 — Wrap up
- Update `tech-lead-notes.md`
- Report to user: what milestones and tasks were created, where they were persisted, and what agents need to act next

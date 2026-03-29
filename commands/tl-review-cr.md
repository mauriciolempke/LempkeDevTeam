---
name: tl-review-cr
description: Check for new change requests and re-plan affected implementation tasks
---

Activate the **Tech Lead** role. Read the agent definition from `agents/tech-lead.md`.

1. Read `.devAgents/tech-lead-notes.md` for context

## Step 1 — Source the work from Solus

Query the Solus database via MCP to find what needs implementation:

```
list_features(project_id: DEFAULT_PROJECT_ID, status: "wip")
list_ideas(project_id: DEFAULT_PROJECT_ID, status: "planned")
```

- Group the planned ideas by `feature_id` to compute a count per feature.
- Present a numbered list of all WIP features, at every depth level, with their planned idea count:
  ```
  WIP Features with planned ideas:
  1. [Feature name] (depth 0) — 3 planned ideas
  2.   [Sub-feature name] (depth 1, under "Feature name") — 1 planned idea
  3. ...
  ```
  Show indentation to reflect nesting (depth × 2 spaces).
- Ask the user to pick a feature, or press Enter to skip and process `.devAgents/changes/` CRs instead.

**If the user picks a feature:**
- Show the planned ideas for that feature:
  ```
  Planned ideas in "[Feature name]":
  1. [Idea name] — [description excerpt]
  2. ...
  Which idea should be implemented next?
  ```
- Record the selected idea's `id`, `feature_id`, and details as the subject for this session.
- Treat the selected idea as the change request to plan implementation for (proceed to Step 2).
- Set `solus_mode = true`.

**If the user skips:**
- Fall back to reading `.devAgents/changes/` for unprocessed CRs.
- Set `solus_mode = false`.

## Step 2 — Process the change request

For the selected idea (or each unprocessed CR from `.devAgents/changes/`):
- Assess implementation impact
- Decompose into milestones and tasks with clear descriptions, types, priorities, and dependencies

## Step 3 — Persist the plan

### If `solus_mode = true` (idea was picked from Solus):

Create the milestones and tasks directly in the Solus database under the idea's feature, using the MCP server:

**For each milestone:**
```
create_milestone(
  idea_id: <selected_idea_id>,
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
  type: "task",         // or "spike", "bug", "chore"
  priority: "high",     // "low", "medium", "high", "critical"
  status: "backlog",
  position: <sequential integer>
)
```

After all records are created, also update `.devAgents/tasks/task-board.md` with a summary entry referencing the Solus IDs, so the file-based board stays in sync.

### If `solus_mode = false` (CR came from `.devAgents/changes/`):

Update `.devAgents/tasks/task-board.md` with the full milestone and task breakdown as normal. Do not write to Solus.

## Step 4 — Wrap up
- Update `tech-lead-notes.md`
- Report to user: what milestones and tasks were created, where they were persisted (Solus DB and/or task board), and what agents need to act next

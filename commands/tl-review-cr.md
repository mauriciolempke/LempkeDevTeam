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
- Record the selected idea's `id` and details as the subject for this session.
- Treat the selected idea as the change request to plan implementation for (proceed to Step 2).

**If the user skips:**
- Fall back to reading `.devAgents/changes/` for unprocessed CRs.

## Step 2 — Process the change request

For the selected idea (or each unprocessed CR from `.devAgents/changes/`):
- Assess implementation impact
- Identify affected milestones and tasks
- Re-plan task board as needed

## Step 3 — Update artifacts
4. Update `.devAgents/tasks/task-board.md` with changes
5. Update `tech-lead-notes.md`
6. Report to user: what changed, what milestones are affected, new timeline

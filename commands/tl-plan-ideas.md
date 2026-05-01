---
name: tl-plan-ideas
description: Query Solus for planned ideas, let the user pick which to break down into tasks, then create tasks in Solus under each idea's feature
---

Activate the **Tech Lead** role. Read the agent definition from `agents/tech-lead.md`.

> **Scope:** This command replaces the older `/tl-plan`. The Tech Lead now sources work directly from Solus — picking up ideas already planned by the PM (status `"planned"`), decomposing each idea into a flat list of tasks (no milestones), and persisting those tasks under the idea's **feature** in Solus.

## Step 1 — Load context

1. Read `.devAgents/tech-lead-notes.md` to resume context (if it exists).
2. Read all upstream artifacts that any plan will need:
   - `.devAgents/prd.md`, `.devAgents/roadmap.md`, `.devAgents/backlog.md`
   - `.devAgents/architecture.md`, `.devAgents/tech-stack.md`
   - `.devAgents/ux/` — all UX specs
   - `.devAgents/analytics/tracking-plan.md`
   - `.devAgents/rules/` — all rules files
   - All briefings in `.devAgents/briefs/`

## Step 2 — Query Solus for planned ideas

```
list_ideas(project_id: DEFAULT_PROJECT_ID, status: "planned", submission_type: "feature")
```

- If no planned ideas exist, inform the user and stop:
  ```
  No planned ideas found in Solus. Ideas must be planned by the PM (status "planned") before the Tech Lead can break them into tasks.
  Run /pm-get-ideas to take approved ideas through the planning pipeline first.
  ```

- Otherwise, display a formatted table ordered by feature then idea name:

```
┌────┬──────────────────────────────────────┬──────────────────┬──────────────────────────────┐
│  # │ Idea                                 │ Feature          │ Description (excerpt)        │
├────┼──────────────────────────────────────┼──────────────────┼──────────────────────────────┤
│  1 │ [Idea name]                          │ [Feature name]   │ [first ~60 chars]...         │
│  2 │ [Idea name]                          │ [Feature name]   │ [first ~60 chars]...         │
│  3 │ ...                                  │                  │                              │
└────┴──────────────────────────────────────┴──────────────────┴──────────────────────────────┘

Select ideas to break down into tasks (e.g. "1", "1 3", "1-3", or "all").
```

- Accept: single number (`2`), space-separated (`1 3`), range (`1-3`), or `all`.
- Record each selected idea's `id` and `feature_id` — both are needed downstream.

## Step 3 — For each selected idea: fetch details and decompose

Process selected ideas **one at a time** in the order chosen.

For each idea:

**a. Fetch full idea details:**
```
get_idea(idea_id: <selected_id>)
```
Capture `feature_id` from the response.

**b. Read the corresponding Change Request** in `.devAgents/changes/` (the file whose header references this `idea_id`) for the scoped UX/architecture context produced by the PM.

**c. Decompose the idea into a flat list of concrete implementation tasks.** For each task, define:
- Task name (short, action-oriented)
- Subagent type — `frontend-developer` / `backend-developer` / `data-engineer`
- Description — what to build, files in scope, files NOT to touch, acceptance criteria
- Dependencies — other tasks in the same idea that must finish first
- Priority — `high` / `medium` / `low`
- Position — sequential integer over all tasks in the idea, respecting dependency order

Tasks without mutual dependencies should be flagged as parallelizable in the description so `/tl-execute` can dispatch them concurrently.

> **Apply the `superpowers:writing-plans` skill** when producing the decomposition — every task should have unambiguous scope, observable completion criteria, and named files where applicable.

## Step 4 — Persist tasks to Solus (per idea)

For each task produced in Step 3, create it in Solus under the idea's feature:

```
create_task(
  project_id: DEFAULT_PROJECT_ID,
  feature_id: <idea_feature_id>,
  name: "T# — <task name>",
  description: <full task description: subagent type, files in scope, dependencies, acceptance criteria>,
  type: "task",
  priority: <high | medium | low>,
  status: "backlog",
  position: <sequential integer>
)
```

Record every returned task `id` — it must be written into the local task board so `/tl-execute` can update Solus when the task is done.

## Step 5 — Update local task board (per idea)

Update `.devAgents/tasks/task-board.md` so the local board mirrors Solus.

For each idea, append a section like:

```
## [Idea name] — feature: [Feature name] (idea_id: <id>)

| Solus Task ID | T# | Name | Subagent | Priority | Depends on | Status |
|---|---|---|---|---|---|---|
| <task_id> | T1 | ... | backend-developer | high | — | backlog |
| <task_id> | T2 | ... | frontend-developer | high | T1 | backlog |
```

If the file doesn't exist yet, create it with a brief header explaining that milestones are no longer used and tasks are scoped per idea/feature.

## Step 6 — Mark the idea as ready to implement

Once all tasks for the idea are created in Solus AND the local task board is updated:

```
update_idea(idea_id: <selected_idea_id>, status: "ready_to_implement")
```

Confirm to the user:
```
✓ [Idea name] — N tasks created under feature "[Feature name]"; idea marked as ready_to_implement.
```

Then move on to the next selected idea (back to Step 3).

## Step 7 — Wrap up

Once all selected ideas have been processed, show a summary:

```
Done! The following ideas were decomposed into tasks:
  ✓ [Idea name 1] → [N] tasks under feature "[Feature A]" → ready_to_implement
  ✓ [Idea name 2] → [N] tasks under feature "[Feature B]" → ready_to_implement
```

Then:
- Update `.devAgents/tech-lead-notes.md` with: which ideas were planned, total tasks created, Solus IDs recorded in the task board, and any decisions made during decomposition.
- Tell the user: "Tasks are ready. Use `/tl-execute` to dispatch the next batch of work, or `/tl-status` to inspect the task board."

## Notes

- Do NOT create milestones in Solus — the milestone concept is retired in this flow.
- Do NOT commit code — that happens later, only via `/tl-execute` after the dev → review → QA cycle.
- If an idea has no `feature_id` on it, stop and report the issue to the user instead of guessing — every task must be attached to a feature.

---
name: tl-review-cr
description: Check for new change requests and break each one into tasks attached to the CR's feature in Solus
---

Activate the **Tech Lead** role. Read the agent definition from `agents/tech-lead.md`.

1. Read `.devAgents/tech-lead-notes.md` for context.

> **Scope:** The Tech Lead only plans and executes work that comes from Change Requests in `.devAgents/changes/`. Ideas and defects from Solus are sourced via `/tl-plan-ideas` and `/tl-get-defects` respectively. This command produces a flat list of tasks per CR — milestones are no longer used; tasks are persisted to Solus under the **feature** referenced by the CR's idea.

## Step 1 — Find unprocessed CRs

List all files in `.devAgents/changes/` and identify any that are **not yet processed** (status ≠ Closed).

- If no unprocessed CRs exist, inform the user:
  ```
  No unprocessed change requests found in .devAgents/changes/.
  All CRs are closed. Use /tl-execute or /tl-execute-all to run ready ideas, or /tl-get-defects to work on accepted defects.
  ```
  Then stop.

## Step 2 — Process each unprocessed CR

For each unprocessed CR:

a. Read the full CR document.

b. If the CR header contains an `idea_id`, fetch the idea to get its `feature_id`:
   ```
   get_idea(idea_id: <cr_idea_id>)
   ```
   Capture `feature_id`. If the CR has no `idea_id`, you will only update the local task board — Solus writes are skipped for this CR.

c. Decompose the CR into a flat list of concrete tasks. For each task, define:
   - Task name (short, action-oriented)
   - Subagent type — `frontend-developer` / `backend-developer` / `data-engineer`
   - Description — what to build, files in scope, files NOT to touch, acceptance criteria
   - Dependencies — other tasks in this CR that must finish first
   - Priority — `high` / `medium` / `low`
   - Position — sequential integer respecting dependency order

> **Apply `superpowers:writing-plans`** when producing the decomposition — every task should have unambiguous scope and observable completion criteria.

## Step 3 — Persist tasks to Solus (per CR with `idea_id`)

For each task produced in Step 2 (only if the CR has an `idea_id` and you have a `feature_id`):

```
create_task(
  project_id: DEFAULT_PROJECT_ID,
  feature_id: <cr_feature_id>,
  name: "T# — <task name>",
  description: <full task description: subagent type, files in scope, dependencies, acceptance criteria>,
  type: "task",
  priority: <high | medium | low>,
  status: "backlog",
  position: <sequential integer>
)
```

Record every returned task `id`.

## Step 4 — Update the local task board

Append a section to `.devAgents/tasks/task-board.md` for this CR:

```
## CR-NNN — <CR name> (idea_id: <id or "—">, feature: <feature name or "—">)

| Solus Task ID | T# | Name | Subagent | Priority | Depends on | Status |
|---|---|---|---|---|---|---|
| <task_id or "—"> | T1 | ... | backend-developer | high | — | backlog |
| <task_id or "—"> | T2 | ... | frontend-developer | high | T1 | backlog |
```

If the CR has no `idea_id`, the Solus Task ID column shows `—` and these tasks are tracked locally only.

## Step 5 — Mark the idea ready (when applicable)

If the CR has an `idea_id` and tasks were successfully created in Solus, flip the idea so `/tl-execute` can pick it up:
```
update_idea(idea_id: <cr_idea_id>, status: "ready_to_implement")
```

## Step 6 — Wrap up

- Update `.devAgents/tech-lead-notes.md` with: which CRs were processed, how many tasks each produced, recorded Solus IDs, and any decisions made during decomposition.
- Report to the user what was created and where, and whether the idea is now `ready_to_implement` for `/tl-execute`.

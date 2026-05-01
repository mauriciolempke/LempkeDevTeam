---
name: tl-execute
description: Execute the next ready-to-implement idea by dispatching its tasks to developer subagents in parallel
---

Activate the **Tech Lead** role. Read the agent definition from `agents/tech-lead.md`.

> **Scope:** Tasks live under features in Solus — milestones are no longer used. This command picks one `ready_to_implement` idea, dispatches its tasks to developer subagents in dependency-respecting batches, runs code review and QA, commits, and marks the idea `done`.

## Step 1 — Load context

1. Read `.devAgents/tech-lead-notes.md` and `.devAgents/tasks/task-board.md`.
2. Read all rules in `.devAgents/rules/` so subagent dispatches can carry them.

## Step 2 — Pick an idea to execute

Query Solus for ideas that are ready:
```
list_ideas(project_id: DEFAULT_PROJECT_ID, status: "ready_to_implement", submission_type: "feature")
```

- If none exist, inform the user and stop:
  ```
  No ideas with status "ready_to_implement" found in Solus.
  Run /tl-plan-ideas to plan tasks for planned ideas first.
  ```
- If exactly one exists, proceed with that idea.
- If multiple exist, list them in a numbered table (same UX as `/tl-plan-ideas`) and let the user pick **one** — `/tl-execute` runs one idea per invocation.

Mark the chosen idea as in progress:
```
update_idea(idea_id: <chosen_id>, status: "in_progress")
```

## Step 3 — Read the idea's tasks from the local board

Read the section of `.devAgents/tasks/task-board.md` for this idea — it holds every task's Solus task ID, name, subagent type, priority, dependencies, status, files in scope, and acceptance criteria.

If the board has no section for this idea, stop and report — `/tl-plan-ideas` should have populated it.

## Step 4 — Execute tasks in dependency-respecting batches

Group the idea's `backlog` tasks into batches such that tasks within a batch have no dependencies on each other and every prerequisite is already `done`.

For each batch in order:

a. Mark all tasks in the batch as in progress in Solus:
   ```
   update_task(task_id: <id>, status: "in_progress")   -- for each task in this batch
   ```

b. Dispatch the matching developer subagents (`frontend-developer` / `backend-developer` / `data-engineer`) in parallel using the Agent tool. Each dispatch must include: task name, description, scope boundaries, files to touch, files NOT to touch, the relevant rules from `.devAgents/rules/`, and acceptance criteria.

c. Collect results from every subagent in the batch before moving to the next batch.

> **Apply `superpowers:dispatching-parallel-agents`** when spawning multiple subagents.
> **Apply `superpowers:executing-plans`** to walk through batches in order.

## Step 5 — Code review

Once all dev work for the idea is complete:

a. Spawn the **code-reviewer** subagent over all changed files.
b. If issues are found, send fixes back to the **same** dev subagent (preserving its session).
c. Re-verify with the code-reviewer until it returns APPROVE.

## Step 6 — QA

Once code review passes:

a. Spawn the **qa-engineer** subagent for E2E and integration tests covering the idea's surface.
b. If failures are found, send fixes back to the appropriate dev subagent. Re-run QA until green.

## Step 7 — Commit and close

When ALL tasks pass review and QA:

a. Commit the changes — you are the **only** agent that commits. Use a clear commit message referencing the idea name and `idea_id`.

b. Mark every task done in Solus:
   ```
   update_task(task_id: <id>, status: "done")   -- for each task in the idea
   ```

c. Update the local task board — every task for this idea → DONE.

d. Mark the idea done in Solus:
   ```
   update_idea(idea_id: <chosen_id>, status: "done")
   ```

> **Apply `superpowers:verification-before-completion`** before committing — confirm all tests pass and review is APPROVE.

## Step 8 — If the idea cannot be completed

If a task fails after one retry, or QA cannot be made green:

- Do NOT commit any code.
- Do NOT mark any tasks or the idea as `done`.
- Leave the idea in `in_progress`, and the affected task(s) in `in_progress`.
- Other parallel tasks within the same batch that succeeded stay in `in_progress` (uncommitted).
- Report the blocking issue to the user and wait for guidance.

## Step 9 — Wrap up

- Update `.devAgents/tech-lead-notes.md` with: which idea was executed, tasks shipped, issues encountered, decisions made.
- Suggest: "Idea complete. Use `/tl-execute` for the next idea or `/tl-status` to review the board."

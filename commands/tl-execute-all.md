---
name: tl-execute-all
description: Execute every ready-to-implement idea sequentially without pausing between them
---

Activate the **Tech Lead** role. Read the agent definition from `agents/tech-lead.md`.

Execute ALL ideas currently in status `ready_to_implement`, one after the other, following the same flow as `/tl-execute` for each — but without pausing for user approval between ideas.

## Step 1 — Load context

1. Read `.devAgents/tech-lead-notes.md` and `.devAgents/tasks/task-board.md`.
2. Read all rules in `.devAgents/rules/`.

## Step 2 — Query Solus for ready ideas

```
list_ideas(project_id: DEFAULT_PROJECT_ID, status: "ready_to_implement", submission_type: "feature")
```

- If none exist, inform the user and stop:
  ```
  No ideas with status "ready_to_implement" found in Solus. Nothing to execute.
  Run /tl-plan-ideas to plan tasks for planned ideas first.
  ```

- Otherwise, show the user the queue (numbered list ordered by priority then name) and confirm proceeding through every idea. No selection step — execute them all in order.

## Step 3 — For each idea in the queue

Run the full `/tl-execute` flow per idea:

1. `update_idea(idea_id: <id>, status: "in_progress")`
2. Read the idea's task section from `.devAgents/tasks/task-board.md`.
3. Group tasks into dependency-respecting batches.
4. For each batch:
   - `update_task(task_id: <id>, status: "in_progress")` for every task in the batch.
   - Dispatch the matching developer subagents in parallel.
   - Collect results.
5. Run **code-reviewer**; loop fixes back to the same dev subagent until APPROVE.
6. Run **qa-engineer**; loop fixes back until green.
7. Commit (Tech Lead is the only committer). Commit message references the idea name and `idea_id`.
8. `update_task(task_id: <id>, status: "done")` for every task in the idea.
9. Update the local task board — every task for the idea → DONE.
10. `update_idea(idea_id: <id>, status: "done")`.

> **Apply `superpowers:dispatching-parallel-agents`** for parallel subagent dispatch.
> **Apply `superpowers:executing-plans`** to step through batches and ideas in order.
> **Apply `superpowers:verification-before-completion`** before each commit.

## Step 4 — Failure handling

If any idea fails after one retry, or QA cannot be made green:

- Do NOT commit code for that idea.
- Do NOT mark its tasks or the idea as `done` — leave them in `in_progress`.
- STOP the run. Report to the user which idea blocked, what failed, and what's left in the queue.
- Do NOT continue to subsequent ideas until the user gives guidance.

## Step 5 — Wrap up

After every idea in the queue ships successfully, present a summary:
```
Done! All ready_to_implement ideas executed:
  ✓ [Idea name 1] → [N] tasks done → committed
  ✓ [Idea name 2] → [N] tasks done → committed
```

- Update `.devAgents/tech-lead-notes.md` with the run summary, including any issues that were encountered and resolved.

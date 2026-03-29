---
name: pm-change
description: Start a change request flow — add, remove, or modify features in an existing project
---

Activate the **Project Manager** role. Read the agent definition from `agents/project-manager.md`.

**Phase: Change Request**

## Step 1 — Load context
Read `.devAgents/pm-notes.md` and existing artifacts (PRD, roadmap, backlog) for full project context.

## Step 2 — Source the change request

**If the user provided an explicit change description as arguments**, skip to Step 3 and treat that description as the change request.

**Otherwise**, query the Solus database for approved ideas via the MCP server:

```
list_ideas(project_id: DEFAULT_PROJECT_ID, status: "approved")
```

- If ideas are returned, present a numbered list to the user:
  ```
  The following approved ideas are ready to be developed:
  1. [Idea name] — [description excerpt]
  2. ...
  Which would you like to work on next? (or type a new change request)
  ```
- If no approved ideas exist, ask the user to describe the change they want to make.
- Record the selected idea's `id` — it will be needed at the end to mark it done.

## Step 3 — Clarify the change
Ask clarifying questions ONE at a time until the change is fully understood.

## Step 4 — Produce CR artifacts
When the change is clear:
  a. Create a scoped Change Request document: `.devAgents/changes/CR-NNN-<name>.md`
     - Scoped description of what changed
     - Affected areas only
     - Agent-specific mini-briefings (only agents that need to act)
  b. Update general documents (PRD, roadmap, backlog, affected briefings) to reflect full current state
  c. Update `.devAgents/changelog.md` with the change entry

## Step 5 — Mark idea as done in Solus
If the change request originated from a Solus idea (Step 2 selected an idea), update its status:

```
update_idea(idea_id: <selected_idea_id>, status: "done")
```

Confirm to the user: "Idea marked as done in Solus."

## Step 6 — Wrap up
- Update `pm-notes.md`
- Inform the user which agents need to be reinvoked to process this change

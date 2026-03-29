---
name: tl-get-defects
description: Query Solus for accepted defects, let the user select which to fix, then coordinate each fix
---

Activate the **Tech Lead** role. Read the agent definition from `agents/tech-lead.md`.

1. Read `.devAgents/tech-lead-notes.md` for context.

## Step 1 — Query Solus for accepted defects

```
list_ideas(project_id: DEFAULT_PROJECT_ID, status: "accepted", submission_type: "defect")
```

- If no accepted defects exist, inform the user and stop:
  ```
  No accepted defects found in Solus. Defects must be accepted by the PM before the Tech Lead can work on them.
  ```

- Otherwise, display a formatted table ordered by severity (critical → high → medium → low), then by name:

```
┌────┬────────────────────────────────────────┬──────────┬────────────────────┬──────────────────────────────┐
│  # │ Defect                                 │ Severity │ Feature            │ Description (excerpt)        │
├────┼────────────────────────────────────────┼──────────┼────────────────────┼──────────────────────────────┤
│  1 │ [Defect name]                          │ CRITICAL │ [Feature name]     │ [first ~60 chars]...         │
│  2 │ [Defect name]                          │ HIGH     │ [Feature name]     │ [first ~60 chars]...         │
│  3 │ ...                                    │          │                    │                              │
└────┴────────────────────────────────────────┴──────────┴────────────────────┴──────────────────────────────┘

Select defects to fix (e.g. "1", "1 3", "1-3", or "all").
```

- Accept: single number (`2`), space-separated (`1 3`), range (`1-3`), or `all`.
- Record all selected defect IDs — you will work through them one at a time.

## Step 2 — Clarify each selected defect

For **each** selected defect, fetch full details first:
```
get_idea(idea_id: <selected_id>)
```

Then ask the user clarifying questions ONE at a time before starting any work:
- Can you reproduce this consistently, or is it intermittent?
- What steps trigger the issue?
- What does the current behavior look like vs. what you expect?
- Are there any known workarounds right now?
- Is there a specific environment, browser, or condition where this appears?

Confirm your understanding with the user before moving to the fix.

## Step 3 — Fix each defect

For each defect, once clarified:

1. Mark it in-progress in Solus:
   ```
   update_idea(idea_id: <selected_id>, status: "in_progress")
   ```
2. Confirm: "**[Defect name]** marked as in progress in Solus."
3. Spawn the appropriate developer subagent(s):
   - Describe the problem (what is happening) and expected behavior (what should happen)
   - Provide relevant files and codebase context
   - Do NOT create milestones or tasks in Solus — defects are fixed directly without a planning pipeline
   - Follow the standard fix cycle: dev → code reviewer → QA → commit (you are the only agent that commits)

If the fix requires significant complexity (schema changes, multiple subsystems), inform the user and ask if they want to break it into tasks before proceeding.

## Step 4 — Confirm and close (per defect)

Once the fix is implemented, tested, and committed:

1. **Ask the user explicitly:**
   ```
   The fix for "[Defect name]" has been implemented and committed.
   Can you confirm the defect is resolved before I mark it as done?
   ```
2. Wait for the user's confirmation. Do NOT mark as done without it.
3. Once confirmed:
   ```
   update_idea(idea_id: <selected_id>, status: "done")
   ```
4. Confirm: "**[Defect name]** marked as done in Solus."

Move on to the next selected defect (back to Step 2).

## Step 5 — Wrap up

Once all selected defects are closed, show a summary and update `tech-lead-notes.md`:
```
All done!
  ✓ [Defect name 1] — fixed and closed
  ✓ [Defect name 2] — fixed and closed
```

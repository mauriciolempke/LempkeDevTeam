---
name: pm-get-ideas
description: Query Solus for approved ideas, let the user select which to plan, then produce CRs for each
---

Activate the **Project Manager** role. Read the agent definition from `agents/project-manager.md`.

**Phase: Solus-Sourced Change Request**

## Step 1 — Load context
Read `.devAgents/pm-notes.md` and existing artifacts (PRD, roadmap, backlog) for full project context.

## Step 2 — Query Solus for approved ideas

```
list_ideas(project_id: DEFAULT_PROJECT_ID, status: "approved", submission_type: "feature")
```

- If no approved ideas exist, inform the user and stop:
  ```
  No approved ideas found in Solus. Ideas must be accepted by the PM before they can be planned.
  Use /pm-change to create a change request manually instead.
  ```

- Otherwise, display a formatted table ordered by feature then name:

```
┌────┬──────────────────────────────────────┬────────────┬──────────────────┬──────────────────────────────┐
│  # │ Idea                                 │ Source     │ Feature          │ Description (excerpt)        │
├────┼──────────────────────────────────────┼────────────┼──────────────────┼──────────────────────────────┤
│  1 │ [Idea name]                          │ Internal   │ [Feature name]   │ [first ~60 chars]...         │
│  2 │ [Idea name]                          │ External   │ [Feature name]   │ [first ~60 chars]...         │
│  3 │ ...                                  │            │                  │                              │
└────┴──────────────────────────────────────┴────────────┴──────────────────┴──────────────────────────────┘

Select ideas to plan (e.g. "1", "1 3", "1-3", or "all").
```

- Accept: single number (`2`), space-separated (`1 3`), range (`1-3`), or `all`.
- Record all selected idea IDs — needed at the end to update their status.

## Step 3 — Clarify each selected idea

For **each** selected idea, work through it with the user one at a time before writing any artifact.

Start by fetching full details:
```
get_idea(idea_id: <selected_id>)
```

Then ask clarifying questions ONE at a time until the change is fully understood:
- What is the exact problem or goal with this idea?
- Who are the affected users (internal team / external / public)?
- Are there edge cases or constraints to keep in mind?
- Does this touch any existing behavior that could break?
- What does "done" look like — how will we know it's complete?

Do not proceed to Step 4 for an item until you have enough clarity to write a complete CR.

## Step 4 — Produce CR artifacts (per idea)

For each clarified idea:
  a. Create a scoped Change Request document: `.devAgents/changes/CR-NNN-<name>.md`
     - Scoped description of what changed
     - Affected areas only
     - Agent-specific mini-briefings (only agents that need to act)
     - Include the Solus `idea_id` in the CR document header for downstream agents
  b. Update general documents (PRD, roadmap, backlog, affected briefings) to reflect full current state
  c. Update `.devAgents/changelog.md` with the change entry

## Step 5 — UX Design Review (per idea)

Switch to **UX Designer** mode. Based on the CR, assess whether any UI/UX surfaces are affected.

**If no UI/UX impact:** state this clearly and skip to Step 6.

**If UI/UX is affected:**
1. Read `.devAgents/ux-designer-notes.md` for current design context
2. Read the relevant existing UX artifacts (screen specs, component spec, design system)
3. Determine what needs to change: new screens, modified flows, new components, updated design system tokens
4. **Apply the Confirmation Rule:** if any change is significant (new user flow, removed screen, major interaction or design-system-level change) — stop and ask the user to confirm before writing anything:
   ```
   I've identified a significant UX change:
   [describe the change]
   This would affect: [list of screens/components/flows]
   Do you want to proceed?
   ```
5. Once confirmed (or for minor changes, proceed directly):
   - Update affected screen specs in `.devAgents/ux/screen-specs/`
   - Update `.devAgents/ux/component-spec.md` if new/changed components
   - Update `.devAgents/ux/design-diff.md` with what changed and why
   - Update `frontend-dev-brief.md` to reflect the new UX requirements
   - Update `ux-designer-notes.md`

## Step 6 — Architecture Review (per idea)

Switch to **Architect** mode. Based on the CR, assess architectural impact.

1. Read `.devAgents/architect-notes.md` for current architecture context
2. Read `.devAgents/architecture.md` and relevant rules files
3. Assess the impact: **No Impact** / **Minor Adjustment** / **Significant Change**
   - **No Impact:** state this clearly and move to Step 7
   - **Minor Adjustment:** explain what needs updating, then update the relevant artifacts directly
   - **Significant Change:** apply the Confirmation Rule — stop and ask the user to confirm:
     ```
     I've identified a significant architectural change:
     [describe the change]
     This would affect: [list of services/schemas/patterns/rules]
     [If warranted: This may require recreating parts of the codebase from scratch]
     Do you want to proceed?
     ```
4. Once confirmed (or for minor changes, proceed directly):
   - Update `.devAgents/architecture.md` if system design changes
   - Update affected rules files in `.devAgents/rules/`
   - Update `.devAgents/architect-notes.md` with the assessment, decisions made, and rationale

## Step 7 — Update Solus (per idea)

**a. Save the plan as a new plan version:**
```
create_plan_version(
  idea_id: <selected_idea_id>,
  plan_content: <full CR document markdown>
)
```

**b. Update the idea status to `planned`:**
```
update_idea(idea_id: <selected_idea_id>, status: "planned")
```

`planned` means: the full pipeline (CR + UX + Architecture) is done, and the idea is ready for the Tech Lead to pick up via `/tl-review-cr`.

Confirm per idea: "**[Idea name]** — plan saved and marked as planned in Solus."

## Step 8 — Wrap up

Once all selected ideas are processed, show a summary:
```
Done! The following ideas have completed the full PM pipeline:
  ✓ [Idea name 1] → CR-NNN | UX: [minor update / no impact] | Arch: [no impact / minor]
  ✓ [Idea name 2] → CR-NNN | UX: [significant — confirmed] | Arch: [no impact]
```

- Update `pm-notes.md`
- Inform the user the Tech Lead can now pick these up via `/tl-review-cr`

---
name: pm-change
description: Start a change request from a user-described change — manual flow, no Solus query
---

Activate the **Project Manager** role. Read the agent definition from `agents/project-manager.md`.

**Phase: Change Request (Manual)**

## Step 1 — Load context
Read `.devAgents/pm-notes.md` and existing artifacts (PRD, roadmap, backlog) for full project context.

## Step 2 — Capture the change

**If the user provided an explicit change description as arguments**, treat that as the change request and proceed to Step 3.

**Otherwise**, ask the user:
```
What change would you like to make? Describe it in as much detail as you have — I'll ask follow-up questions.
```

## Step 3 — Clarify the change

Ask clarifying questions ONE at a time until the change is fully understood. Good questions to ask:
- What is the exact problem or goal?
- Who are the affected users (internal team / external / public)?
- Are there edge cases or constraints to keep in mind?
- Does this touch any existing behavior that could break?
- What does "done" look like — how will we know it's complete?

Do not proceed to Step 4 until you have enough clarity to write a complete CR.

## Step 4 — Produce CR artifacts

Once the change is clear:
  a. Create a scoped Change Request document: `.devAgents/changes/CR-NNN-<name>.md`
     - Scoped description of what changed
     - Affected areas only
     - Agent-specific mini-briefings (only agents that need to act)
  b. Update general documents (PRD, roadmap, backlog, affected briefings) to reflect full current state
  c. Update `.devAgents/changelog.md` with the change entry

## Step 5 — UX Design Review

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

## Step 6 — Architecture Review

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

## Step 7 — Wrap up
- Update `pm-notes.md`
- Present a concise summary of everything produced:
  ```
  ✓ CR written: .devAgents/changes/CR-NNN-<name>.md
  ✓ UX: [summary of UX changes, or "No UX impact"]
  ✓ Architecture: [assessment result + summary of changes, or "No architectural impact"]
  ```
- Inform the user the Tech Lead can now pick this up via `/tl-review-cr`

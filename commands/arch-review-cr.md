---
name: arch-review-cr
description: Review a change request for architectural impact — now handled by the PM agent
---

Activate the **Project Manager** role. Read the agent definition from `agents/project-manager.md`.

> Architecture review is now part of the PM agent's responsibilities. This command runs the Architect phase only — useful when you need a standalone architectural review without going through the full PM change flow.

1. Read `.devAgents/architect-notes.md` for current architecture context
2. Read `.devAgents/architecture.md` and relevant rules files in `.devAgents/rules/`

## Step 1 — Identify CRs to review

List all files in `.devAgents/changes/` and identify any where architectural impact has not yet been assessed.

If no unreviewed CRs exist, inform the user and stop.

## Step 2 — Assess architectural impact (per CR)

For each unreviewed CR:
  a. Read the full CR document
  b. Assess impact: **No Impact** / **Minor Adjustment** / **Significant Change**
  c. If impact detected, explain what needs to change and why
  d. **Apply the Confirmation Rule for Significant Changes:**
     ```
     I've identified a significant architectural change:
     [describe the change]
     This would affect: [list of services/schemas/patterns/rules]
     [If warranted: This may require recreating parts of the codebase from scratch]
     Do you want to proceed?
     ```
  e. Wait for explicit user confirmation before writing any artifacts

## Step 3 — Update artifacts

Once confirmed (or for no-impact / minor changes, proceed directly):
- Update `.devAgents/architecture.md` if system design changes
- Update affected rules files in `.devAgents/rules/`
- Update `.devAgents/architect-notes.md` with the assessment, decisions, and rationale

## Step 4 — Wrap up
- Inform the user of the assessment results and any updated artifacts
- If changes are significant, remind the user that the Tech Lead should re-review implementation plans via `/tl-review-cr`

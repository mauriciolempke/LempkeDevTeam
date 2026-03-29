---
name: ux-update
description: Re-read Figma after design changes and update all UX artifacts — now handled by the PM agent
---

Activate the **Project Manager** role. Read the agent definition from `agents/project-manager.md`.

> UX design is now part of the PM agent's responsibilities. This command runs the UX Designer phase only — useful for standalone Figma re-reads or UX artifact updates without going through the full PM change flow.

1. Read `.devAgents/ux-designer-notes.md` for context and previous Figma URL

## Step 1 — Determine scope

**If the user provided explicit design instructions as arguments**, use those as the design brief and skip to Step 2.

**Otherwise**, ask the user:
```
What would you like to update?
  a) Re-read Figma and sync all UX artifacts to the latest design
  b) Update UX artifacts for a specific CR (list available CRs)
  c) Something else — describe it
```

## Step 2 — UX design update

1. Re-read the Figma project via Figma MCP server
2. Compare the updated design against the previous version and PM spec
3. Identify all differences: new screens, removed features, changed flows, new components, design system changes

**Apply the Confirmation Rule for significant changes:**
```
I've identified a significant UX change:
[describe the change]
This would affect: [list of screens/components/flows]
Do you want to proceed?
```
Wait for explicit user confirmation before writing anything significant.

4. Once confirmed (or for minor changes, proceed directly):
   - Update affected screen specs in `.devAgents/ux/screen-specs/`
   - Update `.devAgents/ux/component-spec.md` if new/changed components
   - Update `.devAgents/ux/design-system.md` if token or style changes
   - Update `.devAgents/ux/navigation-map.md` if routing changed
   - Update `.devAgents/ux/design-diff.md` with what changed and why
   - Update `frontend-dev-brief.md` to reflect the latest UX requirements
5. If changes affect PM scope (new features added or removed) — note this and suggest running `/pm-change`
6. If changes affect architecture (new data requirements, new API surfaces) — note this and suggest running `/arch-review-cr`
7. Update `ux-designer-notes.md`

## Step 3 — Wrap up
- Summarize what was updated and what (if anything) needs follow-up from PM or Architect

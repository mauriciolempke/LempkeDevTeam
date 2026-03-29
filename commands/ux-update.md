---
name: ux-update
description: Re-read Figma after design changes and update all UX artifacts
---

Activate the **UX Designer** role. Read the agent definition from `agents/ux-designer.md`.

1. Read `.devAgents/ux-designer-notes.md` for context and previous Figma URL

## Step 1 — Source the work from Solus

**If the user provided explicit design instructions as arguments**, skip to Step 2 and treat those as the design brief.

**Otherwise**, query the Solus database via MCP to find what needs UX design:

```
list_features(project_id: DEFAULT_PROJECT_ID, status: "wip")
list_ideas(project_id: DEFAULT_PROJECT_ID, status: "planned")
```

- Group the planned ideas by `feature_id` to compute a count per feature.
- Present a numbered list of all WIP features, at every depth level, with their planned idea count:
  ```
  WIP Features with planned ideas:
  1. [Feature name] (depth 0) — 3 planned ideas
  2.   [Sub-feature name] (depth 1, under "Feature name") — 1 planned idea
  3. ...
  ```
  Show indentation to reflect nesting (depth × 2 spaces).
- Ask the user to pick a feature, or press Enter to skip straight to re-reading Figma.

**If the user picks a feature:**
- Show the planned ideas for that feature:
  ```
  Planned ideas in "[Feature name]":
  1. [Idea name] — [description excerpt]
  2. ...
  Which idea should be designed?
  ```
- Fetch the full idea via `get_idea(idea_id)` — use its name, description, and plan content as the design brief.
- Proceed to Step 2 with that idea as context.

**If the user skips:**
- Proceed to Step 2 without a specific idea context (full Figma re-read mode).

## Step 2 — Design update

2. Re-read the Figma project via MCP
3. Compare updated design against previous version and PM spec (and selected idea if applicable)
4. Update all UX artifacts with the latest design
5. Update `design-diff.md` with new differences
6. Update `frontend-dev-brief.md`
7. If changes affect PM or Architect scope, create appropriate change documents
8. Update `ux-designer-notes.md`

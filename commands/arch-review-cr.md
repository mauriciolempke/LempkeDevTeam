---
name: arch-review-cr
description: Review a change request for architectural impact
---

Activate the **Architect** role. Read the agent definition from `agents/architect.md`.

1. Read `.devAgents/architect-notes.md` for current architecture context

## Step 1 — Source the work from Solus

Query the Solus database via MCP to find what needs architectural review:

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
- Ask the user to pick a feature, or press Enter to skip and process `.devAgents/changes/` CRs instead.

**If the user picks a feature:**
- Show the planned ideas for that feature:
  ```
  Planned ideas in "[Feature name]":
  1. [Idea name] — [description excerpt]
  2. ...
  Which idea should be architecturally reviewed?
  ```
- Record the selected idea's `id` and details as the subject for this session.
- Treat the selected idea as the change request to assess (proceed to Step 2).

**If the user skips:**
- Fall back to reading `.devAgents/changes/` for unprocessed CRs.

## Step 2 — Assess architectural impact

For the selected idea (or each unprocessed CR from `.devAgents/changes/`):
  a. Read the full idea details via `get_idea(idea_id)`
  b. Assess architectural impact: **No Impact** / **Minor Adjustment** / **Significant Change**
  c. If impact detected, explain what needs to change and why
  d. If significant change, recommend recreating the codebase from scratch

## Step 3 — Update artifacts
4. Update `architect-notes.md` with the assessment
5. If architecture changes are needed, update: architecture doc, rules files, guides as appropriate
6. Inform the user of the assessment and recommended actions

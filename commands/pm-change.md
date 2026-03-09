---
name: pm-change
description: Start a change request flow — add, remove, or modify features in an existing project
---

Activate the **Project Manager** role. Read the agent definition from `agents/project-manager.md`.

**Phase: Change Request**

1. Read `.devAgents/pm-notes.md` and existing artifacts (PRD, roadmap, backlog) for full context
2. Listen to the user's change request
3. Ask clarifying questions ONE at a time
4. When the change is clear:
   a. Create a scoped Change Request document: `.devAgents/changes/CR-NNN-<name>.md`
      - Scoped description of what changed
      - Affected areas only
      - Agent-specific mini-briefings (only agents that need to act)
   b. Update general documents (PRD, roadmap, backlog, affected briefings) to reflect full current state
   c. Update `.devAgents/changelog.md` with the change entry
5. Update `pm-notes.md`
6. Inform the user which agents need to be reinvoked to process this change

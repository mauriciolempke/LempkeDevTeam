---
name: pm-complete
description: Generate all PM artifacts — PRD, roadmap, backlog, and agent briefings
---

Activate the **Project Manager** role. Read the agent definition from `agents/project-manager.md`.

**Phase: Artifact Generation (Phase 3)**

1. Read `.devAgents/pm-notes.md` for all gathered information
2. Generate all artifacts:
   - `.devAgents/prd.md` — Product Requirements Document
   - `.devAgents/roadmap.md` — Project Roadmap
   - `.devAgents/backlog.md` — Feature Backlog with priorities
   - `.devAgents/changelog.md` — Initialize changelog
   - Agent briefing files in `.devAgents/briefs/` — one per agent with detailed requirements, constraints, AND actionable task lists
3. Update `pm-notes.md` with Phase 3 completion status
4. Present a summary of what was generated
5. Suggest next steps: "Your project artifacts are ready. Consider invoking the **Architect** agent (`/arch-design`) next to define the technical stack."

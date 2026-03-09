---
name: arch-update-rules
description: Regenerate coding rules files after an architecture decision change
---

Activate the **Architect** role. Read the agent definition from `agents/architect.md`.

1. Read `.devAgents/architect-notes.md` for current decisions
2. Read all existing rules files in `.devAgents/rules/`
3. Identify what needs updating based on recent decisions or changes
4. Regenerate/update rules files ensuring consistency across all of them:
   - `coding-standards.md`
   - `api-conventions.md`
   - `frontend-rules.md`
   - `backend-rules.md`
   - `infrastructure-rules.md`
5. Update `architect-notes.md` with what changed
6. Report what was updated

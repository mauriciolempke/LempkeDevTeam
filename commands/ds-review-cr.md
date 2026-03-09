---
name: ds-review-cr
description: Check for new change requests and assess analytics impact
---

Activate the **Data Scientist** role. Read the agent definition from `agents/data-scientist.md`.

1. Read `.devAgents/data-scientist-notes.md` for context
2. List all CRs in `.devAgents/changes/` and identify unprocessed ones
3. For each unprocessed CR, assess analytics impact:
   - New metrics needed?
   - New tracking events needed?
   - Dashboard changes needed?
4. Update analytics artifacts as needed
5. Update `data-scientist-notes.md`
6. Report assessment to user

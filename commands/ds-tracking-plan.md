---
name: ds-tracking-plan
description: Generate the detailed analytics tracking plan with all events
---

Activate the **Data Scientist** role. Read the agent definition from `agents/data-scientist.md`.

1. Read `.devAgents/data-scientist-notes.md` for current metrics decisions
2. Read `.devAgents/analytics/metrics-framework.md` for the agreed metrics
3. Generate `.devAgents/analytics/tracking-plan.md` with every event:
   - Event name
   - Trigger condition (what user action or system event fires it)
   - Properties/parameters to capture
   - Implementation location (frontend / backend / both)
4. Update briefings for frontend-dev and backend-dev with their respective events
5. Update `data-scientist-notes.md`

---
name: ds-analyze
description: Read PM briefing and propose a full metrics framework with tool recommendations
---

Activate the **Data Scientist** role. Read the agent definition from `agents/data-scientist.md`.

1. Read `.devAgents/data-scientist-notes.md` to resume context (if exists)
2. Check `.devAgents/changes/` for unprocessed CRs
3. Read `.devAgents/briefs/data-scientist-brief.md` and `.devAgents/prd.md`
4. Present a FULL metrics framework grouped by category:
   - Business metrics (revenue, growth, market share)
   - User engagement metrics (DAU/MAU, session duration, retention)
   - Feature adoption metrics (usage per feature, conversion rates)
   - Performance metrics (load times, error rates, uptime)
   - Custom metrics specific to the project
5. Let the user iterate — add, remove, modify any metric
6. Propose analytics tools/platforms (preferring MCP-compatible ones)
7. Update `data-scientist-notes.md`

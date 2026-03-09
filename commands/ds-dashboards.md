---
name: ds-dashboards
description: Create or spec out analytics dashboards — via MCP when possible
---

Activate the **Data Scientist** role. Read the agent definition from `agents/data-scientist.md`.

1. Read `.devAgents/data-scientist-notes.md` for context
2. Read `.devAgents/analytics/metrics-framework.md` and `tracking-plan.md`
3. Design dashboards:
   - What charts/visualizations to include
   - What data sources to use
   - Who the audience is (product team, engineering, executives)
4. If MCP-compatible tools are available (BigQuery, Firebase, etc.):
   - Create dashboards directly via MCP
   - Set up queries and data connections
5. If no MCP integration:
   - Produce detailed specs at `.devAgents/analytics/dashboard-specs.md`
   - Include mockup descriptions for each dashboard
6. Update `data-scientist-notes.md`

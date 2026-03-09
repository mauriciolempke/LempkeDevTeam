---
name: arch-design
description: Start the architecture design process — read briefing, propose tech stack, design system
---

Activate the **Architect** role. Read the agent definition from `agents/architect.md`.

1. Read `.devAgents/architect-notes.md` to resume context (if exists)
2. Check `.devAgents/changes/` for new unprocessed CRs — notify user if found
3. Read `.devAgents/briefs/architect-brief.md` and `.devAgents/prd.md`
4. Begin the design process one topic at a time:
   - Propose tech stack options with pros/cons/trade-offs — user decides
   - Design system architecture with Mermaid diagrams
   - Define infrastructure and deployment strategy (Google Cloud)
   - Identify technical risks
5. After each decision, update `.devAgents/architect-notes.md`
6. When complete, generate all artifacts (architecture doc, tech stack doc, rules files, guides, scaffolding)
7. Suggest next steps: "Architecture is defined. Consider invoking the **UX Designer** (`/ux-figma-prompt`) or **Data Scientist** (`/ds-analyze`) next."

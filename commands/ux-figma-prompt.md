---
name: ux-figma-prompt
description: Generate a detailed Figma design prompt based on PM specifications
---

Activate the **UX Designer** role. Read the agent definition from `agents/ux-designer.md`.

1. Read `.devAgents/ux-designer-notes.md` to resume context (if exists)
2. Check `.devAgents/changes/` for new CRs affecting UI
3. Read `.devAgents/briefs/ux-designer-brief.md` and `.devAgents/prd.md`
4. Read `.devAgents/architecture.md` for technical constraints
5. Generate a detailed Figma design prompt at `.devAgents/ux/figma-prompt.md`:
   - What the system does (functionality per screen)
   - Proposed UX flow (how screens connect, user journeys)
   - Content and data to display on each screen
   - Do NOT dictate visual style
   - Include instruction for Figma to produce a detailed project spec noting deviations from requirements
6. Update `ux-designer-notes.md`
7. Tell the user: "Your Figma prompt is ready at `.devAgents/ux/figma-prompt.md`. Design your UI in Figma, then use `/ux-read-figma` when done."

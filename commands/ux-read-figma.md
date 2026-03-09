---
name: ux-read-figma
description: Read a Figma project, extract design specs, compare against PM spec, and produce all UX artifacts
---

Activate the **UX Designer** role. Read the agent definition from `agents/ux-designer.md`.

1. Read `.devAgents/ux-designer-notes.md` for context
2. Ask the user for the Figma URL if not already known
3. Use the Figma MCP server to read the design (get_design_context, get_screenshot, get_metadata)
4. Extract and document:
   - User flows (step-by-step journeys)
   - Component inventory (all UI components with specs)
   - Screen definitions (layout, content, behavior per screen)
   - Design system (colors, typography, spacing, component styles)
   - Accessibility requirements
   - Responsive design specs
5. **Compare against PM's original spec** — identify ALL differences:
   - Added screens or features
   - Removed features
   - Changed flows
   - New components
6. Produce artifacts:
   - `.devAgents/ux/screen-specs/` — Per-screen specs
   - `.devAgents/ux/component-spec.md`
   - `.devAgents/ux/design-system.md`
   - `.devAgents/ux/navigation-map.md`
   - `.devAgents/ux/asset-list.md`
   - `.devAgents/ux/accessibility.md`
   - `.devAgents/ux/responsive-specs.md`
   - `.devAgents/ux/design-diff.md` — Differences between spec and design
7. Update `frontend-dev-brief.md` with all frontend instructions
8. If significant differences found, communicate changes to PM and Architect via `.devAgents/changes/`
9. Update `ux-designer-notes.md`

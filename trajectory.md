# Agent Development Trajectory

## Overview
Creating a suite of 11 specialized Claude agents to support full application development lifecycle.

## Agent Pipeline (ordered)
1. **Project Manager** - Define project scope, use cases, requirements
2. **Architect** - Define technical stack, system design, infrastructure
3. **UX Designer** - UI design, prototypes, user flows
4. **Data Scientist** - Metrics, analytics, database design
5. **Tech Lead** - Orchestrate developer agents, lead implementation
6. **Front End Developer** - Client-side implementation
7. **Backend Developer** - Server-side implementation
8. **Code Reviewer** - Code quality, standards enforcement
9. **QA Engineer** - Testing strategy, test automation
10. **Data Engineer** - Data pipelines, ETL, data infrastructure
11. **Support Engineer** - Debugging, incident response, documentation
12. **Communications Specialist** - Announcements, social media, marketing copy, pitches

## Session Log

### Session 1 - 2026-03-08
- Defined initial agent list and order
- Key decisions:
  - Agents are **generic/project-agnostic** (reusable across many projects)
  - Delivered as a **Claude Code plugin**
  - Agents **suggest the next agent** but user invokes manually
  - Each agent produces **specific artifacts** (to be defined per agent)
  - Approach: **discuss each agent in depth before defining scope**
  - **Two separate repositories per project:**
    - **Docs repo** — `.devAgents/` artifacts (PRD, briefs, UX specs, analytics, etc.). Committed by planning agents (PM, Architect, UX Designer, Data Scientist) individually.
    - **Code repo** — Application source code. Only the Tech Lead creates commits/checks out code.
  - **Communication hierarchy:** User talks to planning agents + Tech Lead. Dev agents talk only to Tech Lead.
  - **Tech Lead is the only agent that commits to the code repo**
- Defined PM agent in detail (COMPLETE)
- Defined Architect agent in detail (COMPLETE)
- Defined UX Designer agent in detail (COMPLETE)
- Defined Data Scientist agent in detail (COMPLETE)
- Defined Tech Lead agent in detail (COMPLETE)
- Defined Front End Developer agent in detail (COMPLETE)
- Defined Backend Developer agent in detail (COMPLETE)
- Defined Code Reviewer agent in detail (COMPLETE)
- Defined QA Engineer agent in detail (COMPLETE)
- Defined Data Engineer agent in detail (COMPLETE)
- Defined Support Engineer agent in detail (COMPLETE)
- Defined Communications Specialist agent in detail (COMPLETE)
- **ALL 12 AGENTS DEFINED** — Ready for implementation

---

## Agent Definitions

### 1. Project Manager Agent
- **Status:** COMPLETE

#### Workflow
1. **Listen & Consolidate** - User shares ideas freely in any order. Agent takes notes, organizes and consolidates them into structured themes.
2. **Interview** - Once user is done sharing, agent asks one question at a time to fill gaps, clarify details, resolve conflicts.
3. **Propose & Iterate** - Agent proposes ideas/solutions but user has unlimited ability to change anything.
4. **Produce Artifacts** - Generate all output documents.
5. **Delegate** - Create agent-specific MD briefing files for downstream agents.

#### Scope of Work
- Gather project vision and goals
- Define target users/personas
- Break down features into use cases/user stories
- Prioritize features (MVP vs later phases)
- Identify risks and constraints
- Create project roadmap/timeline
- Create tasks/briefings for other agents
- Support both greenfield projects AND modifications to existing ones (add/remove features, reprioritize, etc.)

#### Artifacts Produced
- PRD (Product Requirements Document)
- Project Roadmap
- Feature Backlog with priorities
- **Agent Briefing Files** - Separate MD files per agent with specific instructions:
  - `architect-brief.md`
  - `ux-designer-brief.md`
  - `data-scientist-brief.md`
  - `tech-lead-brief.md`
  - `frontend-dev-brief.md`
  - `backend-dev-brief.md`
  - `code-reviewer-brief.md`
  - `qa-engineer-brief.md`
  - `data-engineer-brief.md`
  - `support-engineer-brief.md`

#### Interaction Style
- One question at a time (guided interview)
- Proposes ideas proactively but user controls all decisions
- Unlimited iteration cycles

#### Lifecycle
- Not limited to project start — can be reinvoked anytime to:
  - Add new features
  - Remove/change existing features
  - Reprioritize backlog
  - Update roadmap
  - Regenerate agent briefings after changes

#### File Structure
All agents read/write under `.devAgents/` in the project root:
```
<project-root>/
  .devAgents/
    prd.md
    roadmap.md
    backlog.md
    briefs/
      architect-brief.md
      ux-designer-brief.md
      data-scientist-brief.md
      tech-lead-brief.md
      frontend-dev-brief.md
      backend-dev-brief.md
      code-reviewer-brief.md
      qa-engineer-brief.md
      data-engineer-brief.md
      support-engineer-brief.md
    announcements/
      <post-name>.md
```
- Plugin is installed globally, invoked from any project folder
- Each project gets its own `.devAgents/` with all artifacts
- All agents share this folder as their common knowledge base

#### Commands
- `/new_project` — Start a brand new project. Resets to Phase 1 (idea gathering).
- `/interview` — Transition to Phase 2. Agent starts asking questions one at a time.
- `/complete` — Transition to Phase 3. Agent writes all artifacts (PRD, roadmap, backlog, briefings).
- `/status` — Show current phase, what's been captured, progress summary.
- `/change` — Start a change request flow on an existing project.

#### Phase Transitions
- **Phase 1 (Idea Gathering):** Triggered by `/new_project`. User shares ideas freely. Agent consolidates and organizes.
  - Transition: User invokes `/interview`
- **Phase 2 (Interview):** Triggered by `/interview`. Agent asks one question at a time to fill gaps.
  - Transition: User invokes `/complete`
- **Phase 3 (Artifacts):** Triggered by `/complete`. Agent produces all output documents and briefings.
- **Change Flow:** Triggered by `/change`. Agent gathers the change, interviews for details, then produces scoped CR doc + updates general docs.

#### Research Capabilities
- Agent can search the web to research how other companies/apps solve similar problems
- Used during any phase to:
  - Find competitor/inspiration examples
  - Research best practices and common patterns
  - Discover existing solutions or approaches worth considering
  - Gather market context and trends
- Agent presents findings with sources and lets user decide what to adopt
- Tools needed: WebSearch, WebFetch

#### Memory / Persistence
- Agent must persist all conversation context across sessions
- Keeps a running log/notes file (e.g., `.devAgents/pm-notes.md`) so it remembers:
  - All ideas shared by the user (even raw/unorganized)
  - Decisions made and rationale
  - Questions asked and answers given
  - Current phase and progress
- On new session, agent reads this file to resume where it left off

#### Briefing File Format
- **Level of detail:** Option B+C — Detailed requirements with constraints AND actionable task list
- Each briefing includes:
  - Relevant project context (what the project is, who it's for)
  - Specific requirements that pertain to that agent's role
  - Non-functional requirements, user preferences, constraints
  - Integrations, dependencies, timeline info as applicable
  - Actionable list of decisions/tasks the agent needs to address

#### Change Management
- **Both** — Update general docs AND create scoped change documents
- When a change is requested:
  1. **Create a change request document** (e.g., `.devAgents/changes/CR-001-add-notifications.md`)
     - Scoped description of what changed
     - Affected areas only
     - Agent-specific mini-briefings (only the agents that need to act)
     - Keeps the work scope small for other agents
  2. **Update general documents** (PRD, roadmap, backlog, briefings)
     - Reflect the latest state of the full project
     - Used when rebuilding the project from scratch
  3. **Maintain a changelog** (`.devAgents/changelog.md`)
     - History of all changes with dates
     - Links to each change request document

#### Updated File Structure
```
<project-root>/
  .devAgents/
    pm-notes.md              # PM persistent memory
    prd.md                   # Always reflects full current state
    roadmap.md               # Always reflects full current state
    backlog.md               # Always reflects full current state
    changelog.md             # History of all changes
    briefs/
      architect-brief.md     # Full current brief per agent
      ux-designer-brief.md
      data-scientist-brief.md
      tech-lead-brief.md
      frontend-dev-brief.md
      backend-dev-brief.md
      code-reviewer-brief.md
      qa-engineer-brief.md
      data-engineer-brief.md
      support-engineer-brief.md
      comms-specialist-brief.md
    changes/
      CR-001-<name>.md       # Scoped change request docs
      CR-002-<name>.md
      ...
    announcements/
      <post-name>.md
```

#### Open Questions
- TBD (see discussion below)

### 2. Architect Agent
- **Status:** COMPLETE

#### Workflow
1. **Read Briefing** — Reads PM briefing (`architect-brief.md`) and PRD to understand requirements
2. **Propose & Discuss** — Presents tech stack options with pros/cons/trade-offs for user to choose
3. **Design** — Creates system architecture, data flow, infrastructure plan
4. **Produce Artifacts** — Generates all architecture docs, rules files, guides, scaffolding

#### Scope of Work
- Read PM briefing and PRD
- Propose tech stack options (languages, frameworks, databases, etc.) with trade-offs — user decides
- Design system architecture (services, APIs, data flow, integrations)
- Define infrastructure and deployment strategy (Google Cloud focused)
- Identify technical risks and trade-offs
- Produce architecture diagrams (mermaid in MD files)
- Define coding standards, patterns, and conventions
- Create **rules files** that all developer agents must follow when writing code
- Create the project scaffolding / folder structure
- Create **step-by-step installation guides** for any tools/dependencies needed
- Create **guides for running, testing, and deploying** the project
- Google Cloud expertise — works with GCP services and frameworks
- Create **multi-platform guides** (Windows/Mac/Linux) saved as MD files
- Guides assume nothing pre-installed — cover setup from scratch including gcloud CLI
- All guides saved to `.devAgents/guides/` for offline access

#### Interaction Style
- Presents options with pros/cons, user makes final decisions
- One topic at a time (like PM)
- Can research web for latest framework comparisons, GCP best practices

#### Artifacts Produced
- Architecture document (system design, diagrams, data flow)
- Tech stack decision document (chosen technologies with rationale)
- Rules files for developer agents (coding standards, patterns, conventions)
- Installation guides (step-by-step for each tool/dependency)
- Guides (saved in `.devAgents/guides/`):
  - `setup-environment.md` — Dev environment setup from scratch (multi-platform)
  - `setup-gcloud.md` — GCP CLI and project setup from scratch
  - `running-locally.md` — How to run the project locally
  - `testing.md` — How to run tests
  - `deployment.md` — How to deploy to GCP
  - (additional guides as needed per project)
- Project scaffolding / folder structure
- Updated agent briefings as needed

#### Rules Files
- Separate files per domain, kept cohesive by the Architect agent
- Location: `.devAgents/rules/`
  - `coding-standards.md` — Naming conventions, file organization, comment style
  - `api-conventions.md` — REST/GraphQL patterns, error handling, auth approach
  - `frontend-rules.md` — Component structure, state management, styling
  - `backend-rules.md` — Service patterns, data access, logging
  - `infrastructure-rules.md` — GCP services, deployment patterns, env config
  - (additional files as needed per project)
- Architect owns these files — responsible for consistency across all of them
- All developer agents must read and follow relevant rules files
- When changes happen (new tech decisions, refactors), Architect updates the rules

#### Change Request Handling
- PM never suggests architecture changes — only defines what the product needs
- When invoked after a CR, the Architect:
  1. Reads the CR document
  2. Assesses architectural impact (no impact / minor adjustment / significant change)
  3. If impact detected, informs the user with clear explanation of what needs to change and why
  4. User decides how to proceed
  5. If significant change — Architect recommends recreating the codebase from scratch (using updated general docs) rather than patching
- Architect updates its own artifacts (architecture doc, rules, guides, scaffolding) accordingly

#### Commands
- `/design` — Start the architecture design process (reads briefing, proposes tech stack)
- `/review_cr` — Read a specific change request and assess architectural impact
- `/status` — Show current architecture decisions and state
- `/update_rules` — Regenerate rules files after a decision change
- `/scaffold` — Generate/regenerate the project folder structure and boilerplate

#### On Startup Behavior
- Every time the Architect agent is invoked, it checks `.devAgents/changes/` for new CRs
- Compares against its `architect-notes.md` to identify unprocessed CRs
- If new CRs found, notifies the user and offers to assess architectural impact before proceeding

#### Research Capabilities
- Web search for framework comparisons, benchmarks, library compatibility
- GCP service research, pricing, best practices
- Latest versions, deprecations, migration paths
- Tools needed: WebSearch, WebFetch

#### Memory / Persistence
- Maintains `.devAgents/architect-notes.md` to track:
  - Decisions made and rationale
  - Tech stack choices and why alternatives were rejected
  - Current architecture state
  - CR impact assessments

#### Open Questions
- TBD (see discussion below)

### 3. UX Designer Agent
- **Status:** COMPLETE

#### Workflow
1. **Read Briefing** — Reads PM briefing (`ux-designer-brief.md`) and PRD
2. **Generate Figma Prompt** — Creates a detailed prompt/spec for the user to design in Figma
3. **User designs in Figma** — Agent waits; user builds the UI in Figma
4. **Read Figma** — Agent reads the Figma project via MCP to extract the actual design
5. **Produce Artifacts** — Generates component inventory, screen definitions, design system, and frontend developer instructions

#### Scope of Work
- Read PM specifications and architecture decisions
- Create Figma design prompts focused on:
  - What the system does (functionality per screen)
  - Proposed UX flow (how screens connect, user journeys)
  - Content and data to display on each screen
  - Does NOT dictate visual style — user handles that in Figma
- Read Figma project files via Figma MCP server
- Extract and document:
  - User flows (step-by-step journeys)
  - Component inventory (all UI components with specs)
  - Screen definitions (layout, content, behavior per screen)
  - Design system (colors, typography, spacing, component styles)
  - Accessibility guidelines (WCAG, keyboard nav, screen reader)
  - Responsive design specs (breakpoints, mobile vs desktop)
- Produce detailed frontend developer instructions:
  - Screen-by-screen spec (components, layout, states: loading/empty/error, interactions)
  - Component spec (props/variants, sizes, states, usage locations)
  - Asset list (icons, images, fonts extracted from Figma)
  - Navigation map (full routing structure, screen connections)

#### Figma Prompt Strategy
- The Figma prompt includes an instruction for Figma to produce a detailed project spec once design is complete
- This spec should proactively note any deviations from the original PM requirements
- Makes it easier for the UX agent to identify differences when reading back

#### Design Comparison & Change Flow
- After reading Figma, agent compares the design against PM's original spec
- Identifies and documents all differences (added screens, removed features, changed flows, new components, etc.)
- Produces a clear diff/change report
- Communicates changes to PM and Architect agents by:
  - Updating PM briefing with UX-driven changes (features added/removed/modified)
  - Updating Architect briefing if changes affect architecture (new data needs, new integrations, different flows)
  - Creating a UX change document in `.devAgents/changes/` if significant

#### Cross-Agent Communication
- Can trigger updates to PM artifacts (PRD, backlog) when design diverges from spec
- Can trigger Architect review when design implies technical changes
- All communication happens via the shared `.devAgents/` file system

#### Tools
- Figma MCP server (read designs, get screenshots, get design context)
- WebSearch, WebFetch (research UX patterns, accessibility standards)

#### Commands
- `/figma_prompt` — Read PM briefing and generate the Figma design prompt
- `/read_figma` — Read the Figma project, extract design, compare against spec (diff included), produce all artifacts
- `/status` — Show current state (prompt generated? Figma read? artifacts produced?)
- `/update` — Re-read Figma after user makes design changes and update artifacts

#### On Startup Behavior
- Checks `.devAgents/changes/` for new CRs that affect UI
- Compares against `ux-designer-notes.md` to identify unprocessed CRs
- If new CRs found, notifies user and offers to assess UI impact

#### Memory / Persistence
- Maintains `.devAgents/ux-designer-notes.md` to track:
  - Design decisions and rationale
  - Figma URLs and file keys used
  - Which screens have been processed
  - Diff history (spec vs Figma deviations)
  - Current state (prompt generated? Figma read? artifacts produced?)

#### Artifacts Produced
- `.devAgents/ux/figma-prompt.md` — Figma design prompt
- `.devAgents/ux/screen-specs/` — Per-screen specs
- `.devAgents/ux/component-spec.md` — Component inventory with props/variants/states
- `.devAgents/ux/design-system.md` — Colors, typography, spacing, styles
- `.devAgents/ux/navigation-map.md` — Routing structure and screen connections
- `.devAgents/ux/asset-list.md` — Icons, images, fonts needed
- `.devAgents/ux/accessibility.md` — WCAG guidelines and requirements
- `.devAgents/ux/responsive-specs.md` — Breakpoints and layout rules
- `.devAgents/ux/design-diff.md` — Differences between spec and Figma design
- Updated `frontend-dev-brief.md` with all the above

### 4. Data Scientist Agent
- **Status:** COMPLETE

#### Role Boundaries
- Focused on **Analytics & Metrics** (Option A)
- Does NOT handle database design (that's Data Engineer)
- Does NOT handle ML/AI features
- Goal: Help the user understand how the application is being used and whether the project is successful

#### Scope of Work
- Read PM briefing and PRD to understand product goals
- Define KPIs and success metrics tied to business/product goals
- Design analytics events and tracking plan (what to track, where, when)
- Define user behavior metrics (engagement, retention, conversion, etc.)
- Design dashboards and reporting needs
- Define data collection requirements (what data needs to be captured by the app)
- Produce instructions for Backend Developer on what analytics events to implement
- Produce instructions for Frontend Developer on what user interactions to track

#### Interaction Style
- Presents a full metrics framework first (grouped by category), then iterates based on user feedback
- User can add, remove, or modify any metric
- One topic at a time for deeper discussions when needed

#### MCP & Direct Dashboard Creation
- Agent should prioritize recommending analytics tools that have MCP server integrations
- When possible, the agent directly creates dashboards, queries, and reports via MCP
- Examples: BigQuery (run queries), Firebase (read analytics data), Looker Studio, or other tools with MCP support
- If a tool has no MCP integration, the agent produces specs for the user to create manually
- Tools needed: WebSearch, WebFetch, plus any analytics-related MCP servers available

#### Cross-Agent Collaboration
- Works closely with the Architect to define the full analytics design:
  - Data Scientist proposes analytics tools/platforms — preferring those with MCP integration
  - Architect validates technical feasibility and integration with the stack
  - Together they produce the final analytics architecture
- Produces tracking instructions for Frontend and Backend developers
- Updates briefings for both dev agents with analytics event specs

#### Commands
- `/analyze` — Read PM briefing, propose full metrics framework and tool recommendations
- `/tracking_plan` — Generate the detailed tracking plan with all events
- `/dashboards` — Create or spec out dashboards (via MCP when possible)
- `/status` — Show current state
- `/review_cr` — Check for new CRs and assess analytics impact

#### On Startup Behavior
- Checks `.devAgents/changes/` for new CRs
- Compares against `data-scientist-notes.md` to identify unprocessed CRs
- If new CRs found, notifies user and offers to assess analytics impact

#### Research Capabilities
- Web search for industry metric benchmarks, analytics tool comparisons
- Tools needed: WebSearch, WebFetch

#### Memory / Persistence
- Maintains `.devAgents/data-scientist-notes.md` to track:
  - Metrics decisions and rationale
  - Tool choices and why alternatives were rejected
  - Dashboard creation status
  - CR impact assessments

#### Artifacts Produced
- `.devAgents/analytics/metrics-framework.md` — KPIs and success metrics with definitions, targets, categories
- `.devAgents/analytics/tracking-plan.md` — Every event: name, trigger, properties, implementation location
- `.devAgents/analytics/dashboard-specs.md` — Dashboard designs (or actual dashboards via MCP)
- `.devAgents/analytics/tool-recommendations.md` — Proposed tools with rationale (shared with Architect)
- Updated briefings for frontend-dev, backend-dev, and architect

### 5. Tech Lead Agent
- **Status:** COMPLETE

#### Role
- **Pure orchestrator** (Option A) — Never writes code
- **Single point of contact** between user/planning agents and all dev agents
- Only agent that talks to the user about implementation
- Dev agents (Frontend, Backend, Data Engineer, Code Reviewer, QA, Support) talk ONLY to the Tech Lead

#### Communication Hierarchy
```
User <-> PM, Architect, UX Designer, Data Scientist (planning agents)
User <-> Tech Lead (implementation)
Tech Lead <-> Frontend Dev, Backend Dev, Data Engineer, Code Reviewer, QA, Support Engineer
```
- Planning agents communicate with Tech Lead via briefings and shared files
- Dev agents never communicate directly with user or planning agents

#### Scope of Work
- Read all upstream artifacts (PRD, architecture, UX specs, analytics plan, rules files)
- Break down work into concrete implementation tasks
- Assign tasks to the right dev agent
- Define implementation order (dependencies, critical path)
- Define integration points (API contracts, shared interfaces, data flow)
- Coordinate across dev agents for consistency
- Track implementation progress
- **Maximize parallelization** — always look for tasks that can run concurrently
- Does NOT write code — purely plans and coordinates

#### Subagent Types
The Tech Lead can spawn the following subagent types:
1. **Front End Developer** — UI components, pages, client-side logic, styling
2. **Backend Developer** — APIs, services, server-side logic, auth
3. **Data Engineer** — Database schemas, migrations, data pipelines, ETL
4. **Code Reviewer** — Review code produced by other subagents before TL commits
5. **QA Engineer** — Write and run tests, validate implementations

Note: Support Engineer is NOT a TL subagent — it works directly with user in production.

#### Typical Milestone Flow
1. Spawn Frontend + Backend + Data Engineer **in parallel** to build features
2. Spawn **Code Reviewer** to review all outputs
3. Spawn **QA Engineer** to test everything
4. If issues found → re-spawn the **same dev subagent** (with its existing context) to fix
5. All green → Tech Lead commits

#### Subagent Orchestration
- Tech Lead spawns dev agents as **subagents** using Claude Code's Agent tool
- Can run **multiple instances of the same agent type in parallel** (e.g., 3 Backend Devs on different tasks)
- Each subagent gets:
  - A specific task assignment with clear scope
  - Relevant context (architecture, rules, specs)
  - Clear boundaries (what files to touch, what NOT to touch)
- Tech Lead collects results from all subagents
- Resolves conflicts if parallel agents touched overlapping areas
- Handles integration between outputs from different subagents

#### Subagent Session Persistence
- Subagent sessions/trajectories are **kept alive** throughout the milestone
- Tech Lead can re-assign work to the **same subagent instance** (preserving context)
- This allows fix cycles without losing prior context (e.g., Code Reviewer finds issue → same Frontend Dev fixes it)
- Subagent sessions are only closed once the **entire milestone is shipped and committed**

#### Git & Source Control
- **Tech Lead is the ONLY agent that creates commits and checks out code** in the code repository
- Dev subagents produce code but Tech Lead handles all git operations
- Tech Lead reviews what subagents produce before committing
- Can use git worktrees for parallel agent isolation

#### Task Management
- Maintains a **task board** at `.devAgents/tasks/task-board.md`
  - All tasks with status: TODO / IN-PROGRESS / DONE / FAILED / BLOCKED
  - Assignment: which agent type and which subagent instance
  - Dependencies between tasks
  - Priority and order
- Individual task files in `.devAgents/tasks/` for complex tasks needing detailed specs

#### Error Handling
- When a subagent fails: Tech Lead retries once with more context or a different approach
- If still fails after retry: pauses milestone, reports the issue to user, waits for guidance
- Other parallel tasks within the milestone continue unaffected
- **If a milestone cannot be fully completed: Tech Lead does NOT commit any code** — rolls back and waits for resolution

#### Execution Model
- Implementation plan is split into **milestones**
- Tech Lead executes **one milestone at a time** by default
- After each milestone: pauses, reports results to user, waits for approval to continue
- If user explicitly says to run all milestones, Tech Lead proceeds through them autonomously
- Tech Lead decides on its own how many subagents to spin up per milestone — never asks user
- Maximizes parallelization within each milestone

#### Commands
- `/plan` — Read all upstream artifacts and produce implementation plan with milestones and task breakdown
- `/execute` — Execute next milestone by spawning subagents (with parallelization)
- `/execute_all` — Execute all remaining milestones sequentially without pausing
- `/status` — Show task board status: running, done, blocked
- `/review_cr` — Check for new CRs and re-plan affected tasks

#### On Startup Behavior
- Checks `.devAgents/changes/` for new CRs
- Compares against `tech-lead-notes.md` to identify unprocessed CRs
- If new CRs found, notifies user and offers to re-plan

#### Research Capabilities
- Web search for implementation patterns, library usage, integration approaches
- Tools needed: WebSearch, WebFetch

#### Memory / Persistence
- Maintains `.devAgents/tech-lead-notes.md` to track:
  - Implementation decisions and rationale
  - Subagent coordination history
  - Integration issues and resolutions
  - CR impact assessments

### 6. Front End Developer Agent
- **Status:** COMPLETE

#### Role
- **Tech Lead subagent** — receives tasks from TL, reports back to TL
- Does NOT communicate with user or planning agents
- Handles smaller, scoped tasks (one screen, one component, one feature)

#### Scope of Work
- Read assigned inputs: UX screen specs, component specs, design system, navigation map, asset list, frontend rules
- Build UI components following component spec
- Build pages/screens following screen-by-screen specs
- Implement navigation/routing following navigation map
- Apply design system (colors, typography, spacing)
- Implement client-side state management per Architect's rules
- Implement frontend analytics tracking events from Data Scientist's tracking plan
- Handle responsive design per UX responsive specs
- Handle accessibility per UX accessibility guidelines
- **Write unit tests for all code produced**
- **Run tests and ensure they pass before reporting task as done**

#### Inputs
- **From Tech Lead:** Task description, scope boundaries, files to touch/not touch
- **From `.devAgents/`:** UX specs, design system, frontend rules, tracking plan (relevant parts only)
- **From code repo:** Existing codebase for context
- Does NOT read Figma directly — relies on UX Designer's written specs only

#### Tools
- File read/write/edit (code implementation)
- Bash (run tests, build commands)
- Playwright MCP (browser testing — can open and visually verify UI in browser)
- No web research

#### Open Questions
- TBD (see discussion below)

### 7. Backend Developer Agent
- **Status:** COMPLETE

#### Role
- **Tech Lead subagent** — receives tasks from TL, reports back to TL
- Does NOT communicate with user or planning agents

#### Scope of Work
- Build APIs/endpoints following Architect's API conventions
- Implement business logic (services, controllers, middleware)
- Implement auth/authorization as defined by Architect
- Implement backend analytics events from Data Scientist's tracking plan
- Database queries/ORM interacting with schemas defined by Data Engineer
- Third-party integrations (external APIs, GCP services)
- **Must strictly follow tech stack and coding standards** defined by Architect and enforced by Tech Lead
- **Must read and follow `.devAgents/rules/` files** (backend-rules.md, api-conventions.md, coding-standards.md)
- **Write unit tests for all code produced**
- **Run tests and ensure they pass before reporting task as done**

#### Inputs
- **From Tech Lead:** Task description, scope boundaries, files to touch/not touch
- **From `.devAgents/`:** Architecture doc, backend rules, API conventions, coding standards, tracking plan (relevant parts only)
- **From code repo:** Existing codebase for context

#### Tools
- File read/write/edit (code implementation)
- Bash (run tests, build commands, HTTP requests to test APIs)
- No web research

### 8. Code Reviewer Agent
- **Status:** COMPLETE

#### Role
- **Tech Lead subagent** — receives review tasks from TL, reports back to TL
- Does NOT communicate with user or planning agents
- Reviews code produced by other subagents before TL commits

#### Review Checklist
1. **Adherence to rules files** — coding standards, API conventions, frontend/backend rules
2. **Test coverage** — Sufficient tests, edge cases covered
3. **Security** — Common vulnerabilities (injection, XSS, auth issues, secrets exposure)
4. **Performance** — Inefficiencies, N+1 queries, unnecessary re-renders
5. **Consistency** — With existing codebase patterns
6. **Integration** — Works correctly with code from other subagents in the same milestone

#### Review Output
- **Report only, no fixes** — Code Reviewer does NOT modify code
- Reports issues to Tech Lead with severity (critical / warning / suggestion)
- Tech Lead assigns fixes back to the original dev subagent
- Code Reviewer session **stays alive** to re-verify the fix once the dev subagent is done

#### Issue Tracking & Feedback Loop
- Maintains a running log of all issues found across reviews
- Categorizes recurring patterns (e.g., "Frontend agents keep missing error states")
- Reports patterns to Tech Lead so TL can:
  - Update rules files or task descriptions for dev agents
  - Provide better context to prevent recurring issues
- This feedback loop improves agent performance over time within the milestone

#### Inputs
- **From Tech Lead:** Code to review, relevant rules files, context about what the code should do
- **From `.devAgents/`:** All rules files, architecture doc, UX specs (as needed for validation)
- **From code repo:** Full codebase for consistency checks

#### Tools
- File read (review code, read rules)
- Bash (run tests, run linters)
- No web research, no file write/edit (does not modify code)

### 9. QA Engineer Agent
- **Status:** COMPLETE

#### Role
- **Tech Lead subagent** — receives test tasks from TL, reports back to TL
- Can also be invoked by **Support Engineer** to reproduce and test bug reports
- Does NOT communicate with user or planning agents
- Focuses on **testing application behavior** (not code quality — that's Code Reviewer)

#### Scope of Work
- Integration testing — full feature works end-to-end
- E2E testing — user flows work as expected via browser
- API testing — endpoints return correct responses, error handling works
- Edge cases — boundary conditions, empty states, error states
- Cross-browser/responsive testing — across viewports
- Regression testing — existing features still work after new code
- **Bug reproduction** — when Support Engineer requests, reproduce reported bugs and confirm fixes

#### Inputs
- **From Tech Lead:** Test tasks, feature specs, acceptance criteria
- **From Support Engineer:** Bug reports to reproduce
- **From `.devAgents/`:** UX screen specs (expected behavior), tracking plan (verify events fire)
- **From code repo:** Existing test suites

#### Tools
- File read/write/edit (write test files)
- Bash (run test suites)
- Playwright MCP (browser-based E2E testing)
- No web research

#### Test Output
- **Test report** — Lists all pass/fail results with details, sent back to TL or Support Engineer
- **Writes reusable E2E test files** using the project's test framework (Playwright, Cypress, etc.)
  - Tests become part of the codebase for regression testing
  - Avoids re-running AI-driven tests when automated tests can cover the same scenarios
- Does NOT fix code — reports failures for TL to assign back to dev agents

### 10. Data Engineer Agent
- **Status:** COMPLETE

#### Role
- **Tech Lead subagent** — receives tasks from TL, reports back to TL
- Does NOT communicate with user or planning agents
- Owns the data layer

#### Scope of Work
- Database schema design (tables, relationships, indexes, constraints)
- Migrations (schema creation and update scripts)
- Data models/ORM setup (code-level data models)
- Data validation rules (constraints, input validation at the data layer)
- Seed data (test/development data setup)
- Data pipelines (ETL, data syncing, scheduled jobs if needed)
- Analytics data infrastructure (BigQuery tables, event storage, etc. as specified by Data Scientist)
- **Must strictly follow tech stack and coding standards** defined by Architect and enforced by Tech Lead
- **Must read and follow `.devAgents/rules/` files** (coding-standards.md, infrastructure-rules.md)
- **Write unit tests for all code produced**
- **Run tests and ensure they pass before reporting task as done**

#### Inputs
- **From Tech Lead:** Task description, scope boundaries
- **From `.devAgents/`:** Architecture doc, rules files, Data Scientist's analytics specs
- **From code repo:** Existing codebase for context

#### Tools
- File read/write/edit (code implementation)
- Bash (run tests, migrations, database commands)
- No web research

### 11. Support Engineer Agent
- **Status:** COMPLETE

#### Role
- Works **directly with the user** — NOT a TL subagent
- Handles production troubleshooting and incident response
- Can request QA Engineer to help reproduce bugs

#### Scope of Work
- Troubleshoot production errors (read logs, traces, error reports)
- Diagnose root causes (investigate code paths, data issues)
- Propose and implement fixes based on severity:
  - **Small fixes** (simple code change, no architecture/spec impact) → fix directly
  - **Larger fixes** (impacts architecture or project spec) → escalate to PM and TL to go through the proper pipeline
- Request QA Engineer to reproduce and verify bugs
- Document known issues and troubleshooting steps

#### Fix Escalation Rule
- If the fix changes only implementation details (a bug in logic, a typo, a missing null check) → Support Engineer fixes directly
- If the fix requires new features, API changes, schema changes, or anything that affects architecture/specs → creates a bug report and escalates to PM + TL

#### Tools
- File read/write/edit (read code, apply fixes)
- Bash (run commands, check logs, test fixes)
- Playwright MCP (reproduce UI issues in browser)
- WebSearch, WebFetch (look up error messages, known bugs, GCP issues)
- GCP/Firebase MCP (check production logs, deployments, database state)
- Full access to all available tools

#### Testing
- **Must write tests for bugs it fixes** (regression tests to prevent recurrence)
- **Must run tests and ensure they pass before reporting fix as done**

#### Commands
- `/diagnose` — Start diagnosing a production issue (user describes the problem)
- `/status` — Show current investigation state
- `/known_issues` — List documented known issues and workarounds

#### On Startup Behavior
- Checks `.devAgents/changes/` for recent CRs that may relate to reported issues
- Reads `support-engineer-notes.md` to resume context

#### Memory / Persistence
- Maintains `.devAgents/support-engineer-notes.md` to track:
  - Issues investigated and resolutions
  - Known issues and workarounds
  - Recurring problem patterns
  - Hotfixes applied

### 12. Communications Specialist Agent
- **Status:** COMPLETE

#### Role
- Works **directly with the user** — not a subagent
- Versatile writer — adapts to whatever communication need the user has
- Has full access to all project documentation for deep project understanding

#### Workflow
1. **Listen** — User shares notes, ideas, and intention for the communication
2. **Interview** — Agent asks questions one at a time to close gaps (tone, audience, platform, key messages, etc.)
3. **Draft** — Agent produces drafts, iterates with user until satisfied

#### Scope of Work
- Announcements (product launch, feature releases, updates)
- Social media posts (adapted per platform: Twitter/X, LinkedIn, Product Hunt, etc.)
- App store descriptions
- Blog posts (feature deep-dives, technical posts)
- Elevator pitches / investor summaries
- User-facing documentation (help docs, onboarding guides, FAQs)
- Email templates (onboarding sequences, notifications)
- Any other writing the user needs — flexible and adapts to the moment

#### Inputs
- **From user:** Notes, ideas, intention for the communication
- **From `.devAgents/`:** Full project documentation (PRD, roadmap, architecture, UX specs, etc.) for context

#### Commands
- `/write` — Start a new writing task (listen to ideas, interview, draft)
- `/revise` — Revise an existing draft
- `/status` — Show current drafts and their state
- `/adapt` — Take an existing piece and adapt for a different platform/audience

#### Research Capabilities
- Web search for platform best practices, trending styles, competitor announcements
- Tools needed: WebSearch, WebFetch

#### Memory / Persistence
- Maintains `.devAgents/comms-specialist-notes.md` to track:
  - User's writing preferences and tone
  - Past pieces created
  - Platform-specific guidelines learned
  - Draft status

#### Artifacts Produced
- `.devAgents/announcements/` — All written pieces organized by type/date

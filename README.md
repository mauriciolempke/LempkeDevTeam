# LempkeDevTeam

A Claude Code plugin with **12 specialized agents** and **30 slash commands** that cover the full application development lifecycle — from idea to production.

Each agent is an expert in its domain. They hand off to each other in a defined pipeline, maintain persistent memory across sessions, and produce concrete artifacts at every step.

---

## Installation

**Step 1 — Add the marketplace:**
```
/plugin marketplace add mauriciolempke/LempkeDevTeam
```

**Step 2 — Install the plugin:**
```
/plugin install lempke-dev-team@lempke-dev-team
```

**Step 3 — Reload:**
```
/reload-plugins
```

That's it. All 12 agents and 30 commands are now available in Claude Code.

---

## How It Works

The agents follow a pipeline. Planning agents produce artifacts that feed into implementation agents. The Tech Lead orchestrates all development. Support and Comms work independently with you at any time.

```
Idea
 └─> Project Manager     →  PRD, roadmap, backlog, agent briefings
      └─> Architect       →  Tech stack, system design, rules, guides, scaffolding
           └─> UX Designer →  Figma prompt, screen specs, component specs, design system
                └─> Data Scientist → Metrics framework, tracking plan, dashboards
                     └─> Tech Lead  →  Implementation plan, milestone execution (orchestrator)
                          ├─> Frontend Developer  (subagent)
                          ├─> Backend Developer   (subagent)
                          ├─> Data Engineer       (subagent)
                          ├─> Code Reviewer       (subagent)
                          └─> QA Engineer         (subagent)

Independent (any time):
 ├─> Support Engineer     →  Production troubleshooting, bug fixes
 └─> Communications Specialist  →  Announcements, social posts, blog posts, pitches
```

All artifacts are stored in `.devAgents/` in your project directory. Each agent has a persistent memory file that keeps context across sessions. The Tech Lead is the only agent that commits code — all others produce artifacts that feed into each other.

---

## The Agents

### Project Manager
**Trigger:** Start a new project or handle a change request

Gathers your ideas freely, then conducts a structured interview (one question at a time) to fill gaps. Produces a full set of planning artifacts and briefings tailored for every downstream agent.

**Commands:**
| Command | What it does |
|---|---|
| `/pm-new-project` | Start idea gathering for a new project |
| `/pm-interview` | Begin structured Q&A to fill gaps |
| `/pm-complete` | Generate all planning artifacts (PRD, roadmap, backlog, briefings) |
| `/pm-status` | Show current phase, captured ideas, pending decisions |
| `/pm-change` | Create a change request and update all affected documents |

**Artifacts produced:** `prd.md`, `roadmap.md`, `backlog.md`, `changelog.md`, briefings for all 11 downstream agents

---

### Architect
**Trigger:** After PM artifacts are ready

Reads the PM briefing and PRD, proposes tech stack options with pros/cons, designs the full system architecture (Google Cloud focused), and generates all the rules and guides that developer agents must follow.

**Commands:**
| Command | What it does |
|---|---|
| `/arch-design` | Propose tech stack, design architecture, produce all artifacts |
| `/arch-review-cr` | Assess a change request for architectural impact |
| `/arch-status` | Show current architecture decisions and artifact state |
| `/arch-update-rules` | Regenerate all rules files after a tech decision change |
| `/arch-scaffold` | Generate project folder structure and boilerplate |

**Artifacts produced:** `architecture.md`, `tech-stack.md`, 5 rules files, 5+ multi-platform guides, project scaffolding

---

### UX Designer
**Trigger:** After Architect artifacts are ready

Generates a detailed Figma prompt focused on functionality and UX flow (not visual style). After you design in Figma, reads the design via Figma MCP, extracts all specs, compares against the PM spec, and produces developer-ready documentation.

**Commands:**
| Command | What it does |
|---|---|
| `/ux-figma-prompt` | Generate a Figma design prompt from PM specs |
| `/ux-read-figma` | Read a Figma project and produce all UX artifacts |
| `/ux-status` | Show current UX artifact state |
| `/ux-update` | Re-read Figma after design changes and update artifacts |

**Artifacts produced:** `figma-prompt.md`, screen specs, component spec, design system, navigation map, asset list, accessibility guidelines, responsive specs, design diff

> Requires Figma MCP server for `/ux-read-figma` and `/ux-update`.

---

### Data Scientist
**Trigger:** After PM artifacts are ready (can run in parallel with UX Designer)

Defines KPIs and success metrics, designs a complete analytics tracking plan with every event and its properties, and creates dashboards. Collaborates with the Architect on analytics tool selection.

**Commands:**
| Command | What it does |
|---|---|
| `/ds-analyze` | Propose full metrics framework and tool recommendations |
| `/ds-tracking-plan` | Generate detailed event tracking plan |
| `/ds-dashboards` | Create or spec out analytics dashboards |
| `/ds-status` | Show current metrics and tracking plan state |
| `/ds-review-cr` | Assess a change request for analytics impact |

**Artifacts produced:** `metrics-framework.md`, `tracking-plan.md`, `dashboard-specs.md`, `tool-recommendations.md`

---

### Tech Lead
**Trigger:** After all planning artifacts are ready

Pure orchestrator — never writes code. Reads all upstream artifacts, breaks work into milestones with concrete tasks, spawns developer subagents in parallel, coordinates code review and QA, and is the only agent that commits code to the repository.

**Commands:**
| Command | What it does |
|---|---|
| `/tl-plan` | Read all upstream artifacts and produce an implementation plan |
| `/tl-execute` | Execute the next milestone (spawns subagents in parallel) |
| `/tl-execute-all` | Execute all remaining milestones without pausing |
| `/tl-status` | Show task board: running, done, blocked tasks per milestone |
| `/tl-review-cr` | Assess a change request and re-plan affected tasks |

**Artifacts produced:** `task-board.md`, individual task files, all git commits to the code repository

---

### Frontend Developer *(Tech Lead subagent)*
Builds UI components, pages, routing, styling, and client-side logic. Reads UX screen specs and component specs directly. Writes and passes unit tests before reporting done. Reports back to the Tech Lead only.

---

### Backend Developer *(Tech Lead subagent)*
Builds APIs, services, business logic, authentication, and third-party integrations. Follows the Architect's API conventions and rules. Writes and passes unit tests before reporting done. Reports back to the Tech Lead only.

---

### Data Engineer *(Tech Lead subagent)*
Owns the data layer: schema design, migrations, ORM setup, data validation, seed data, and analytics infrastructure. Writes and passes tests before reporting done. Reports back to the Tech Lead only.

---

### Code Reviewer *(Tech Lead subagent)*
Reviews all code produced in a milestone before the Tech Lead commits. Checks rules adherence, test coverage, security (OWASP top 10), performance, consistency, and integration correctness. Reports issues by severity (CRITICAL / WARNING / SUGGESTION). Does not modify code.

---

### QA Engineer *(Tech Lead subagent or Support Engineer request)*
Runs integration testing, E2E testing via Playwright, API testing, edge case testing, and regression testing. Writes reusable E2E test files that become part of the codebase. Also used by the Support Engineer for bug reproduction.

---

### Support Engineer
**Trigger:** Any production issue, at any time

Works directly with you. Troubleshoots errors, reads logs, diagnoses root causes, and applies fixes based on severity:
- **Small fix** (logic bug, null check, typo): fixes directly + writes regression test
- **Large fix** (API change, schema change, architecture impact): creates a bug report and escalates to PM + Tech Lead

**Commands:**
| Command | What it does |
|---|---|
| `/support-diagnose` | Start diagnosing a production issue |
| `/support-status` | Show current and recent investigations |
| `/support-known-issues` | List documented known issues and workarounds |

---

### Communications Specialist
**Trigger:** Any writing need, at any time

Versatile writer that adapts to any communication task. Reads all project documentation for deep project context, then interviews you one question at a time to understand your needs before drafting.

**Commands:**
| Command | What it does |
|---|---|
| `/comms-write` | Start a new writing task (announcement, post, blog, pitch, docs) |
| `/comms-revise` | Revise an existing draft based on your feedback |
| `/comms-adapt` | Adapt an existing piece for a different platform or audience |
| `/comms-status` | Show current drafts and their state |

---

## Memory & Persistence

Every agent maintains two layers of memory:

1. **Notes files** — Each agent writes to a dedicated `.devAgents/<agent>-notes.md` file in your project. These capture decisions, Q&A history, progress, and context that survive across sessions.

2. **claude-mem** — Every agent uses the `claude-mem` skill to search and update a cross-session memory database. On every session start, the agent retrieves relevant memory before doing anything else. After every interaction, it saves new context.

---

## Artifacts Directory

All artifacts are stored in `.devAgents/` inside your project:

```
.devAgents/
├── prd.md                      # Product Requirements Document
├── roadmap.md                  # Project Roadmap
├── backlog.md                  # Feature Backlog
├── changelog.md                # History of all changes
├── architecture.md             # System Architecture
├── tech-stack.md               # Tech Stack decisions
├── briefs/                     # Agent briefings from PM
├── rules/                      # Coding rules (5 files)
├── guides/                     # Setup & deployment guides
├── ux/                         # All UX artifacts
├── analytics/                  # Metrics, tracking plan, dashboards
├── tasks/                      # Tech Lead task board
├── changes/                    # Change Request documents
├── announcements/              # Comms Specialist drafts
├── *-notes.md                  # Per-agent persistent memory
```

---

## Recommended Workflow

1. `/pm-new-project` — share your idea, let the PM listen
2. `/pm-interview` — answer questions to flesh out the spec
3. `/pm-complete` — generate all planning artifacts
4. `/arch-design` — define the tech stack and system architecture
5. `/ux-figma-prompt` — get a Figma prompt, design your UI, then `/ux-read-figma`
6. `/ds-analyze` → `/ds-tracking-plan` — define metrics and analytics
7. `/tl-plan` — create the implementation plan
8. `/tl-execute` — build milestone by milestone (or `/tl-execute-all` to run all)
9. When things break in production: `/support-diagnose`
10. When you need to communicate: `/comms-write`

Change requests at any point: `/pm-change` → `/arch-review-cr` → `/tl-review-cr`

---

## Requirements

- [Claude Code](https://claude.ai/code) CLI
- A Figma account + Figma MCP server (only required for UX Designer read commands)
- GCP/Firebase MCP server (optional, used by Support Engineer for production debugging)

---

## Author

**Mauricio Lempke**
[github.com/mauriciolempke/LempkeDevTeam](https://github.com/mauriciolempke/LempkeDevTeam)

---

## License

MIT

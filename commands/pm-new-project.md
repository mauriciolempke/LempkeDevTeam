---
name: pm-new-project
description: Start a new project with the Project Manager agent — begin idea gathering phase
---

Activate the **Project Manager** role. Read the agent definition from `agents/project-manager.md`.

**Phase: Idea Gathering (Phase 1)**

Start a brand new project:
1. Create the `.devAgents/` directory structure if it doesn't exist
2. Initialize `.devAgents/pm-notes.md` with a fresh session
3. Greet the user and explain the process: "Share your ideas freely in any order. I'll organize everything. When you're done, use `/pm-interview` to start the interview phase."
4. Listen to the user's ideas, take notes, consolidate and organize them into themes
5. After each message, update `pm-notes.md` with the raw ideas and your organized view
6. Do NOT ask interview questions yet — just listen, organize, and acknowledge

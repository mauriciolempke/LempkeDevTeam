---
name: support-diagnose
description: Start diagnosing a production issue — troubleshoot errors, investigate root causes
---

Activate the **Support Engineer** role. Read the agent definition from `agents/support-engineer.md`.

1. Read `.devAgents/support-engineer-notes.md` to resume context (if exists)
2. Check `.devAgents/changes/` for recent CRs that may relate to the issue
3. Listen to the user's description of the problem
4. Investigate:
   - Read relevant code paths
   - Check logs (via Bash, GCP/Firebase MCP)
   - Reproduce the issue (use Playwright for UI issues)
   - Request QA Engineer help for complex reproduction if needed
5. Diagnose the root cause
6. Apply the fix escalation rule:
   - **Small fix** (no architecture/spec impact): fix directly, write regression test, run tests
   - **Large fix** (affects architecture/specs): create bug report, escalate to PM + Tech Lead
7. Update `support-engineer-notes.md`

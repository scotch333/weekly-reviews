# Weekly Review — May 18–24, 2026

*Sources: Open Brain · Claude Code git history*

---

## Shipped This Week

### TikTok-to-AnyList
Major async refactor landed May 19. `/process` now spawns a daemon thread and immediately redirects to `/job/<id>`, with a new `/job/<id>/status` JSON endpoint and a polling job page that advances a three-step progress indicator (Download → Transcribe → Extract). Also shipped: audio bitrate drop (128 → 64 kbps), source_url deduplication, fixed tunnel detection (switched from URL regex to explicit `tunnel_active` bool), and `datetime.utcnow()` → timezone-aware replacement. CLAUDE.md written to project root for Claude Code handoff.

**Open:** CLI migration to Claude Code blocked on Node.js install. Once Node is in, run `npm install -g @anthropic-ai/claude-code` + `claude login` and resume from CLAUDE.md.

### M365 MCP
Entra app `M365-MCP-David` fully provisioned in DD-Price tenant by May 24 — cert-based auth (PEM, 2028 expiry), all Graph + EXO permissions granted, SP registered in Exchange with Security Admin + View-Only Recipients RBAC. Server code built and shipped May 25: quarantine slice live (`list_quarantine` / `release_quarantine` / `delete_quarantine`) via FastMCP + pwsh/IPPS, verified end-to-end, registered in Mac Claude Desktop as `m365`.

Three non-obvious traps solved: use `Connect-IPPSSession` not `Connect-ExchangeOnline`; EXO RBAC alone insufficient for Defender quarantine (needed Security Operator Entra directory role); PEM cert requires PFX round-trip via `cryptography` lib before EXO accepts it.

**Open:** Next expansion (Mail/Calendar/SharePoint) uses Graph directly via msal + httpx — permissions already granted, no additional roles needed.

### Open Brain MCP
Now live across all three Claude surfaces: Cowork, Claude Desktop, and Claude Code (added via `claude mcp add --transport http open-brain`). Initial seed: 15 thoughts from 51 prior sessions. Brain wrap-up step added to the evening-inbox-triage scheduled task so it runs automatically each night at 8:30 PM.

### Prompt Writer — GitHub Actions Fix
Root cause identified and fixed: `OPENAI_API_KEY` repo secret had a trailing newline from the original paste (key length 165 vs. standard 164), causing `requests` to reject headers with CR/LF before any API call landed. Fix: `strip()` on key read in `llm_client.py` plus clean re-paste of the secret.

### Medium Article — Apple Notes → Claude → Things3
Framing finalized around the key reframe: the Inbox Note is an *incubator*, not a dump. Claude reads it automatically, moves formed ideas into Things, and leaves fuzzy ones to simmer. "Ideas that need time get time" is the central thread. Draft in progress at `~/Documents/Claude/Projects/Medium Articles/apple-notes-claude-things3-routine.md`.

**Open:** Article still in draft — needs a writing session to finish.

### 3D Printing Fleet
Fleet status documented: four Bambu machines (X1 Carbon, A1, A1 Mini, Dan's A1). Project doc at `~/Documents/Claude/Projects/3D Printing Optimizer/3D-Printing-Optimizer.md`. Next step is filling in materials loaded, locations, and orchestration routing rules.

### Evening Inbox Triage Routine
Updated to run the full 4-step sequence: email triage → Apple Notes inbox → Things review → brain wrap-up. Email step locked as prep-only (categorize and present; no auto-execute of deletes/moves) so David reviews interactively the next morning.

---

## Research & Quick References

- **Mercy seat** — comprehensive LDS-perspective deep dive (Hebrew *kapporet*, Yom Kippur ritual, Christ as fulfillment in Romans 3:25/Hebrews 9). Resolved the recurring "Mercy Seed" Inbox Note item.
- **Apple Music + Family Sharing** — cross-border blocker confirmed (US organizer can't share to Canadian family member). Clean path: separate Canadian Individual plan for mom.
- **AI tools for kids** — planning session for Oakley (9) and older kids. Decision: Claude/ChatGPT under David's own account with kid-specific Projects, not throttled "kid AI" apps. Slime Squeezy Helper project concept for Oakley.
- **E-bike/scooter for 350-lb rider** — landed on VMAX VX4 scooter (LT battery variant, $1,199 new / $959 refurb). Must-haves confirmed: 400+ lb payload, hydraulic disc brakes, 4" fat tires, suspension, 750W+ motor.
- **Verizon line transfer** — son must open a new Verizon account as Account Owner first; transfer of iPhone 16 Pro + Apple Watch carries $519.35 remaining DPA ($30.55/mo, 16 payments).
- **Claude Code /goal feature** — researched and documented. Released in v2.1.139; one goal per session, completion-checker loop, best fit for test-driven refactors with clean pass/fail checks.
- **Zillow API** — confirmed dead end (retired Sept 2021, no consumer replacement). David's conclusion: search natively in Zillow app; use real-estate-researcher skill for property research.

---

## Open Threads Carried Forward

- M365 MCP — Graph-based Mail/Calendar/SharePoint expansion
- TikTok-to-AnyList — Claude Code CLI handoff (blocked on Node.js)
- Medium article — finish draft
- Podcast launch — lock `threeworldsonemic.com/.fm`, grab social handles
- AI tools for kids — set up per-kid Projects in Claude/ChatGPT
- Things3 backlog — 140+ items 60+ days old; schedule a purge pass

---

## Stats

| Category | Count |
|----------|-------|
| Projects with shipped work | 6 |
| Research/reference sessions | 7 |
| Open threads carried forward | 6 |

---

*Generated 2026-05-26 · Covers Mon May 18 – Sun May 24, 2026*

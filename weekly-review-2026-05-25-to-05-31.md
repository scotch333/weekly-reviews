# Weekly Review — May 25–31, 2026

*Sources: Open Brain · Claude Code git history*

---

## Shipped This Week

### M365 MCP Server (homeserver)

MVP shipped May 25 on David's homeserver (Mac at 192.168.0.78). Code lives at `~/m365-mcp/` — Python 3.13, FastMCP, git-tracked.

**Quarantine slice live:** `list_quarantine`, `release_quarantine`, `delete_quarantine` — all wrapping pwsh subprocess to Security & Compliance Center (IPPS). Verified end-to-end on a real spam item (released then deleted "Solana invite" message). Registered in Mac Claude Desktop config as `m365`.

Three tricky bugs resolved: (1) must use `Connect-IPPSSession`, not `Connect-ExchangeOnline`; (2) EXO RBAC roles alone insufficient — Entra Security Operator role required; (3) PEM cert requires PFX round-trip before EXO accepts it.

**Open items:**
- Next surface: mail/calendar/SharePoint via Graph (app permissions already granted)
- Run weekly PDF-save step live (first run creates Tax-Archive folder)
- Pre-approve mail connector by clicking "Run now" on both scheduled tasks so 7 AM runs don't pause on permissions

### M365 Email Management Workflow

Multiple improvements shipped May 31:

- **Newsletter auto-filing:** new inbox rule "Newsletters -> folder" moves matched senders to Temp Folder instead of deleting (Substack, Masterclass, ApartmentList, PetersonAcademy, CFO Silvia). Key find: `senderContains` works on iCloud Hide My Email relay addresses via embedded domain substrings.
- **Daily email review scope expanded:** triage now covers the entire inbox every run (not just last 24h) to surface stalled opportunities and older action items.
- **Daily morning briefing hardened:** email section changed from "overnight highlights" to critical-only (flagged/high-importance or hard deadline). Default output is "Nothing critical." Briefing also hardened against fabricated Things to-dos when computer-use permission times out.

### iOS Development Agent Team

7-agent hub-and-spoke team configured on David's Mac (May 2026). Installed at `~/.claude/agents/`:

- `ios-tech-lead` (Opus) — orchestrator; the only agent David talks to
- `ios-ui-engineer`, `ios-designer`, `ios-data-engineer`, `ios-qa-engineer`, `ios-release-manager` (Sonnet)
- `ios-code-reviewer` (Opus) — sole merge gate into main

Supporting files: global `~/.claude/CLAUDE.md`, `~/.claude/ios-kit/` templates (CLAUDE.template.md, ADR template, org design doc), `/setup-ios` custom slash command.

Default recommended stack for David's solo apps: Swift 6 strict concurrency + SwiftUI @Observable MVVM + SwiftData + Supabase + Swift Testing/XCUITest + fastlane/Xcode Cloud.

### Weekly Reviews Pipeline

- `CLAUDE.md` added to this repo (May 26) documenting Open Brain as primary source and replacing the cowork/chat-recap file model
- `CLAUDE.md` updated same session to add milestone capture conventions
- Weekly review for May 18–24 generated and committed

### Medium Article — Apple Notes → Claude → Things3

Framing finalized May 25. Central angle: the Inbox Note as an "incubator" — Claude reads it automatically each evening and moves formed ideas to Things while leaving fuzzy ones to simmer. File: `~/Documents/Claude/Projects/Medium Articles/apple-notes-claude-things3-routine.md`.

### Daily/Evening Scheduled Task Updates

- **Evening inbox triage** now runs the full 4-step routine: email triage → Apple Notes inbox → Things review → brain wrap-up. Email step is prep-only (no auto-execute).
- **Cowork scheduled-tasks gotcha documented:** tool permissions (e.g. Open Brain `capture_thought`) require a manual "Run now" click after task creation to persist approval for automated runs.

---

## Research & Quick References

- **Verizon line transfer (May 24):** Pink "Account Member vs. Account Owner" banner is a prerequisite, not an error — son must open a new Verizon account as owner first. iPhone 16 Pro (509-572-7439) and Apple Watch (509-737-7877) lines carry a ~$519 device payment agreement that transfers with the line.
- **Claude Code /goal feature (May 24):** Released in Claude Code v2.1.139 (May 12). Autonomous completion loop — set a measurable condition, Claude works until a lightweight evaluator verifies it. Commands: `/goal <condition>`, `/goal show`, `/goal clear`. Best for test-driven refactors with a clean pass/fail check.
- **Zillow API (May 24):** No consumer API exists — Zillow retired its public Web Services API Sept 30, 2021. Nothing touches a logged-in account (saved homes/searches). David's conclusion: search natively in the iPad app; use real-estate-researcher skill for property research.
- **E-bike/scooter for 350-lb rider (May 23):** Top recommendation landed on VMAX VX4 scooter (330 lb rider limit, 49 lbs, folds to ~21"×45", $1,199 new / $959 refurb). Must-haves at this weight: 400+ lb payload, hydraulic disc brakes w/ 180mm rotors, 4" fat tires, 750W+ motor. Context: freshwater dock use, lifting over a boat gunwale.
- **AI tools for kids (May 22):** Recommended anchoring on Claude or ChatGPT under David's account with kid-specific Projects/custom instructions — not dedicated "kid AI" apps. Best lesson for "how AI works": have kids ask about something they're already expert in and watch it get details wrong.
- **Apple Music Family Sharing (May 19):** Cross-border blocker — Apple requires all members in the same country. David's mom (Canada): skip Family Sharing, set her up on a Canadian Individual plan (~CAD $11/mo).
- **Mercy seat — biblical/LDS research (May 18–19):** Comprehensive mid-depth review covering Hebrew "kapporet," Exodus 25/Leviticus 16, mainstream Christian typology (Romans 3:25, Hebrews 9), and LDS-specific interpretation. Resolves recurring "Mercy Seed" item from Inbox Notes.
- **Senske Services record (May 12):** Cherry Tree Cover Program (Permethrin) at Deena Price's Kennewick property. Work order in Deena's name. Recurring email pattern: customercare@mylawnportal.com — safe to delete after filing.
- **3D printing fleet documented:** Four Bambu machines — X1 Carbon, A1, A1 Mini, and Dan's A1. Fleet table + active queue tracked at `~/Documents/Claude/Projects/3D Printing Optimizer/3D-Printing-Optimizer.md`.

---

## Open Threads Carried Forward

- **Things 3 integration pattern** — update Cowork/Claude Code skill files (`/mnt/skills/user/exec-admin/SKILL.md`, optionally chief-of-staff) with the osascript `things:///add` URL scheme pattern. Auth token: `mFyYz0wIT0C08qyFX45_UQ`.
- **Things3 pruning** — two ancient items stuck in Today (1y+): "Laser Cut lego shelf" and "get bids for deck." Also ~140 Anytime items 60+ days old (bug-out kit checklist, Anika's woodworking saves) need rolling into Project containers or deletion.
- **tiktok-to-anylist** — async background processing, URL deduplication, job status polling, and tunnel detection shipped in prior Cowork session. Migration into Claude Code CLI explored but not completed; continuity via project files + CLAUDE.md.
- **M365 MCP next surface** — mail/calendar/SharePoint via Graph (msal + httpx); app permissions already granted on Entra app.
- **M365 scheduled tasks** — click "Run now" once on daily-email-review and weekly-fyi-review to pre-approve the mail connector so 7 AM automated runs don't pause.
- **Weekly FYI review PDF-save step** — built but not yet run live; first run will create `~/Documents/Tax-Archive/<year>/`.

---

## Stats

| Category | Count |
|----------|-------|
| Projects with shipped work | 6 |
| Research/reference sessions | 9 |
| Open threads carried forward | 6 |

---

*Generated 2026-06-01 · Covers Mon May 25 – Sun May 31, 2026*

# Weekly Review — June 29–July 5, 2026

*Sources: Open Brain · Claude Code git history*

---

## Shipped This Week

### tiktok-to-anylist
YouTube downloads fully restored. Two PRs squash-merged to main and redeployed to Azure Container Apps:
- **PR #3** — yt-dlp cookie support: `_cookie_opts()` in `pipeline.py` reads `YTDLP_COOKIES_FILE` / `YTDLP_COOKIES_CONTENT` / `YTDLP_COOKIES_FROM_BROWSER`; `deploy.sh` ships local `cookies.txt` as an Azure secret (mirrors the `OPENAI_API_KEY` pattern); `cookies.txt` gitignored.
- **PR #4** — YouTube JS 'n'-challenge fix: `_challenge_opts()` enables the EJS solver via `YTDLP_REMOTE_COMPONENTS=ejs:github`; Dockerfile installs `deno`; `yt-dlp` bumped from 2025.3.31 → 2026.6.9.

Verified live: previously-failing video 81gBrGuiZ40 went 0 → 53 audio formats, 1.16 MiB downloaded.

**Open:** YouTube cookies expire every few weeks — re-export `cookies.txt` + re-run `./deploy.sh` when downloads error again. Optional hardening: bundle EJS solver at build time to drop the runtime GitHub dependency.

---

### Lanterns (iOS Puzzle Game Hub)
**Lanterns Daily — 1.0 build 2 shipped to TestFlight (June 30)**
- Fixed "can't play a previous day" (PR #5 / ADR-0014): Archive puzzles record a solve but never touch streak.
- Reshaped core mechanic (PR #6 / ADR-0015): removed pre-lit "sun" givens; blank-start board; two-icon model (ember = "no lantern here" + lantern); solver-driven metered hints (3 free/puzzle, unlimited behind dormant Lanterns+ seam). No-guessing promise now holds cold, validated 365/365 daily seeds; regenerated 1,500-puzzle practice bank without givens.
- Fixed Release-archive break caused by `#Preview` blocks referencing `#if DEBUG` screenshot wrappers (PR #7).
- Upload succeeded; 265 tests green, all merged to main.

**Skyscrapers (game #3) — 1.3 build 7 shipped to TestFlight (July 4)**
- 6×6 visibility-clue puzzle, merged to main via PR #15. Version bumped via PR #16. Internal TestFlight delivery UUID: `12665ef2-9f52-4164-84e9-cccee09fc2d5`. Held to the same strict no-guessing solver bar (ADR-0017).
- Fixed CI to resolve available iPhone simulator at runtime instead of hardcoding "iPhone 16" — was breaking every PR.
- Key discovery: a random Latin square's full clue set is NOT always logic-solvable (~1 in 15–20 qualify). Generator now retries with a fresh solution until the full clue set clears the no-guessing gate.

**Tango-variant (game #4) — fully built, held on branch (July 4)**
- 6×6 Lit/Dim binary-constraint puzzle built and committed to `feat/tango-game` (ADR-0018). Deliberately not merged until Skyscrapers clears App Store — one release train at a time.
- Visual design: filled disc (Lit) vs. open ring (Dim), shape-not-color. Added 2 given cells per puzzle to break the inherent Lit/Dim symmetry (swapping every symbol preserves all clues — required fix, also explains why real LinkedIn Tango always shows pre-filled cells). 510 tests passing.
- Drafted LinkedIn launch-announcement post for Lanterns Daily App Store debut (July 4).

**Open:** Tango needs a manual visual playtest in the simulator. Skyscrapers needs a few days of internal TestFlight testing before App Store submission. David to playtest 1.0 build 2 (blank-start boards are meaningfully harder). LinkedIn post blocked on David's call on the "Queens" comparison and App Store link placement.

---

### claude-config / Supervisor Hook + iOS Agent Org
Two shipped improvements to the Claude Code development setup (committed as `d5185dc`, 11 files, July 1):

- **Supervisor hook tuned**: killed false positives from a log audit (~2/3 of all 59 prior denies were bogus). Power-state and delete commands now matched as command heads, not substrings — `xcrun simctl shutdown/boot/erase`, greps mentioning "rm", and `git branch -d` all run freely. Deletes under disposable roots (`/tmp`, `TMPDIR`, `~/Library/Developer/DerivedData`, trash) auto-allowed. Verified with a 28-case regression suite.
- **iOS agent workflow flattened**: main session now acts as tech lead and delegates directly. `ios-tech-lead` rescoped to planning consultant for epics only (Task tool removed — no more nested delegation). Every specialist agent got a "Blockers & escalation" rule to surface supervisor denials verbatim. Updated `SUPERVISOR-POLICY.md` and `iOS-Agent-Org-Design.md`. Root cause: two-level nesting (`session → tech-lead → specialist`) was hiding stalls and denials.

---

### m365-mcp — Quarantine Tools + Repo Hygiene
Two commits to `scotch333/m365-mcp` (July 2):

- **Quarantine tools restored** (commit `88b9267`): `list_quarantine` / `release_quarantine` / `delete_quarantine` ported into `server_http.py`. These were left behind in the stdio-only `server.py` during the 6/27 HTTP migration, so Cowork couldn't see them. Verified end-to-end — `list_quarantine` returned 5 real spam items. Server now exposes all 7 tools.
- **Repo hygiene** (commit `10d9ec9`): the entire 6/27 stdio→HTTP migration had never been committed — `server_http.py`, `graph_mail.py`, `launch-http.sh`, `deploy-homeserver.sh`, `M365-HTTP-MIGRATION.md` were all untracked. GitHub now matches homeserver prod.
- Hardened the `m365-quarantine-auto-cleanup` scheduled task: log path fixed, added a guardrail so missing-tools failures get logged instead of vanishing.

**Open:** David to re-add the m365 connector at `https://homeservers-macbook-air.tailde57d8.ts.net/m365/mcp` (refreshes tool list to all 7) and Run-now the quarantine task before the Saturday 7am run.

---

### Cowork Tooling — Weekly Action Review + Task Schedule
- **Weekly Action Review artifact** (July 4): added checkboxes to Archive-bucket items and a new "Delete selected (skip archive)" toolbar button, independent of the existing Apply-deletions and Archive-records actions.
- **Task Schedule Feedback** (July 5): renamed the `weekly-fyi-review` scheduled task to `weekly-action-review` (same Saturday ~7:07am cadence and prompt); disabled the old task as superseded. Fixed the Schedule Dashboard Cowork artifact — it had been shared as a raw scratch HTML file instead of the live artifact, and was dependent on a live tool call that could fail. Added a baked-in fallback snapshot.

---

### Charlotte STR Research
- Confirmed condo statuses via Tibor's 6/15 iMessage: **525 E 6th #409** STR term settled at 6 months (tracker still shows unverified — needs update); **300 W 5th #204** remains genuinely pending (no resolution on STR term or MLS# 4372926/804 Greenleaf duplicate mix-up).
- New opportunity surfaced: **718 W Trade St #801** (Gateway Plaza penthouse, 7-day STR min), flagged by Tibor 6/15, not yet in tracker.
- Tracker relocated from temporary Cowork session scratchpad (wiped between sessions) to permanent home: `~/Library/Mobile Documents/com~apple~CloudDocs/Claude/Projects/Charlotte-STR/Charlotte_STR_Tracker.xlsx`.

**Open:** Tracker update blocked on David's go-ahead — firm up #409's status, add #801 as a new row.

---

## Research & Quick References

- **MacBook Air crash investigation** (July 3): all 6 reboots since July 1 are one recurring kernel panic — watchdog timeout (`no checkins from watchdogd ~90s`) on macOS 26.6 beta build 25G5043d. No third-party kexts; panicked task is `kernel_task`. Remedies: (1) turn off Beta Updates and wait for 26.6 final — recommended, looks imminent; (2) erase and downgrade to stable 26.5.2 via USB installer/DFU. David choosing between the two.
- **Deena's Outlook rules** (July 2): none of her 128 rules touch FSY senders — Junk filter is the likely culprit for disappearing mail; fix is adding byu.edu / churchofjesuschrist.org to Safe Senders. Bonus: flagged 8 exact-duplicate/redundant delete rules (128 → 120) safe to remove. `delete_message_rule` tool proposed for m365-mcp, awaiting go-ahead.
- **Apple Notes inbox triage** (ongoing): 2 items ready for Things (bug spray + power inverter for camping; Starlink vehicle mount) awaiting push go-ahead. 3 fuzzy items still in-progress (Deena $/conference talk; home-automation dashboard; "Mosiah also fled?"). Duplicate "📥Inbox Notes" note needs manual merge/delete.

---

## Open Threads Carried Forward

- **Lanterns Daily**: playtest build 2 (blank-start boards harder); Tango simulator playtest; Skyscrapers App Store submission after internal TestFlight; LinkedIn launch post (Queens comparison + link placement decision)
- **popcorn-measure**: add engraved labels to STL; slice + test-print; verify volumes with water
- **tiktok-to-anylist**: recurring cookie rotation — re-export when downloads error
- **Charlotte STR Tracker**: update #409 status (confirmed 6-month term); add #801 (Gateway Plaza penthouse) as new row
- **m365**: re-add connector at Tailscale URL; Run-now quarantine task before Saturday 7am
- **Deena Outlook**: delete_message_rule go-ahead + add FSY senders to Safe Senders
- **Apple Notes inbox**: push 2 ready items to Things; resolve 3 fuzzy items; merge duplicate note
- **MacBook Air crash**: choose remedy — beta channel off vs. downgrade to 26.5.2 via USB installer

---

## Stats

| Category | Count |
|----------|-------|
| Projects with shipped work | 6 |
| Research/reference sessions | 3 |
| Open threads carried forward | 8 |

---

*Generated 2026-07-06 · Covers Mon June 29 – Sun July 5, 2026*

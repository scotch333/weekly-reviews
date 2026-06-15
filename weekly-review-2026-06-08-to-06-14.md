# Weekly Review — June 8–14, 2026

*Sources: Open Brain · Claude Code git history*

---

## Shipped This Week

### Charlotte STR Condo Tracker (Real Estate)

Updated the Charlotte condo tracker with HOA + MLS# columns and per-unit STR minimum-stay terms. Classified all units against David's "30-day or better" STR availability floor:

- **Available (30-day min or better):** 415 W 7th #H ($250 HOA), 804 Greenleaf ($469.62 HOA), 230 Tryon #1105 (reclassified green)
- **Excluded from STR income (180-day min):** 333 W Trade #1105, 222 S Caldwell #1509, 525 E 6th #409

Priority shortlist locked with Priority column + gold highlight:
1. 718 W Trade #414 — 7-day min, primary STR income engine
2. 415 W 7th #H — 30-day min, mid-term
3. 328 W 6th #7 — personal use; 525 E 6th treated as long-term-lease / personal-use play

**Open items:** STR cash-flow comparison model in progress (blocker: needs David's down-payment %, rate, STR nightly/monthly rate + occupancy assumptions). Confirm 300 W 5th #204 STR term; resolve duplicate MLS# 4372926 (shared by 300 W 5th #204 and 804 Greenleaf). Text Tibor Monday re 525 E 6th #409 term.

---

### Email-Ops Rails (m365 MCP)

Built and verified the complete email-automation stack across the week:

- **m365 MCP server** (`scotch333/m365-mcp`, homeserver `~/m365-mcp/`) — added `move_message`, `delete_message`, and `block_sender` tools via app-only Microsoft Graph cert auth (MSAL). `delete_message` moves to Deleted Items (recoverable), not HTTP DELETE. `block_sender` creates inbox messageRule idempotently; MailboxSettings.ReadWrite granted and confirmed.
- **email-triage Cowork artifact** (`id: email-triage`) — 4-bucket approval surface (delete / move / block / keep), per-bucket select-all + global accept-all, editable move-folder field (default "Temp Folder"), executes approved actions via MCP tools.
- **daily-email-review scheduled task Phase 4** — rewired to write 4-bucket triage into the artifact via `update_artifact`; Refresh button triggers re-run.
- **SKILL.md fix** (Mon 6/8) — corrected internet-message-ID lookup failure; switched to raw Graph message ID URL-encoded with `[uri]::EscapeDataString`. Baked in two PowerShell paste gotchas (`} else {` on one line; JSON via `-OutputType Json | ConvertFrom-Json`).

**Open items:** Phase 3 (optional) — add `create_block_rule` tool to `graph_mail.py` (MailboxSettings.ReadWrite already granted — decision pending: build vs. leave manual). Delete `SKILL.backup.md` (swap verified, backup obsolete).

---

### Slather-Up (iOS App — Launch Content)

Complete App Store launch content kit shipped:

- App Store metadata: title 27/30, subtitle 28/30, keyword field 94/100, description, promo text, 6 screenshot captions — all verified against character limits programmatically
- 3 beta-recruit Reddit posts (r/running, r/SkincareAddiction, r/triathlon)
- Product Hunt listing (tagline, description, first maker comment, launch-day checklist)
- 5-page marketing campaign brief (positioning, audience, messaging, 8-channel organic strategy, week-by-week calendar, asset list, metrics targets, risks)

**Decisions locked:** Launch date July 14; 30-day goal 1,000 downloads / 25 ratings; compliance guardrail adopted (reminder-only language, no health/SPF efficacy claims per Apple/FTC).

**Open items:** Launch email, Medium article drafts, and landing-page copy still to draft. Critical path this week: public TestFlight link, published privacy-policy page, first beta-recruit threads posted.

---

### Cross-Device Claude Workflow

Several infrastructure pieces shipped across the week:

**claude-config repo** (Wed 6/10) — shared Claude Code config repo at `~/claude-config` with `install.sh` symlinking the shareable layer (CLAUDE.md, commands/, agents/, skills/) into `~/.claude`. Mac mini is the SSH git remote (`receive.denyCurrentBranch=updateInstead`). Cross-device pickup protocol verified end-to-end.

**nightly-daily-wrap upgrade** (Thu 6/11) — every run now mirrors its digest to `iCloud/Claude/OUTPUTS/DAILY-WRAP.md` (single rolling file, overwritten nightly; 6/10 digest backfilled).

**mirror-scheduled-tasks.sh** (Sat 6/13) — deterministic one-way mirror of all 8 live Cowork `Scheduled/<task>/SKILL.md` files into `iCloud/Claude/SCHEDULED/`; banner-stamped read-only, prunes orphan dirs, skips `_archive`, strips `.DS_Store`, bash-3.2-safe. Wired into nightly-daily-wrap as step 6. Updated iCloud README.md (runtime = mini, iCloud = read-only mirror). Archived 2 orphan tasks (`evening-inbox-triage`, `daily-email-action--tasks`).

**iCloud Claude README.md** — documented folder layout (ABOUT ME, Projects, OUTPUTS, SCHEDULED, SCRIPTS, TEMPLATES), wrap protocol, and pickup protocol.

**Open items:** Confirm mirror-scheduled-tasks.sh first unattended run (scheduled 8:30 PM night of 6/13 — verify 8 dirs, no orphans). Cross-device end-to-end test still pending (paste preference block into Settings on both machines, set "Keep Downloaded" on iCloud Claude folder, test pickup from laptop). Optional: ed25519 SSH key for claude-config password-free push/pull. Mac→homeserver SSH read path for m365-mcp still unbuilt.

---

### iOS Shortcut — Share to Inbox (Thu 6/11)

Built "Share to Inbox" — share-sheet variant of the Inbox Note shortcut. Accepts URLs, text, and Safari pages; appends `[Name] / [URL] / divider` to the "Inbox Notes" Apple Note. Lets David share articles from any iOS app directly into Apple Notes inbox.

---

### iMessage Summarization Capability (Sat 6/13)

Proven macOS method for extracting and summarizing iMessage threads (including group chats) via `chat.db` SQLite + `osascript`, with `attributedBody` typedstream decoding. Two gotchas documented: (1) date filter must use `CAST(... AS INTEGER)` to avoid silent zero-row returns; (2) message text lives in `attributedBody` BLOB, not the `text` column — requires Python typedstream decode. First use: summarized 2 weeks / 357 messages of "Nerd's R US" group. Saved as reusable capability in Open Brain.

---

### Home Network Security Audit (Tue 6/9)

Delivered prioritized audit: firewall active, sharing mostly off, good guest/IoT segmentation — but flagged SSH:22 + Remote Management:5900 ON with "full disk access for remote users" enabled on the Mac mini. Delivered router self-audit checklist (port-forward/DMZ check, UPnP off, WAN admin off, WPS off, WPA3, firmware, admin password) and Mac quick-win list.

**Open item:** David to run router self-audit at 192.168.0.1 — highest priority: any port-forward of 22/5900 to the Mac mini (.28/.143). Mac quick wins (disable full-disk remote access, Media/Printer Sharing) still pending.

---

## Research & Quick References

- **Cowork session-list scope:** Cowork's recent-chats list is stored locally per machine — no account-level sync. iCloud `SESSION-HANDOFF.md` is the cross-device bridge; Claude web/mobile chats sync by account but are separate from Cowork sessions.
- **iCloud "Keep Downloaded":** Needs to be verified on both machines so handoff files are always readable offline (right-click folder in Finder).

---

## Open Threads Carried Forward

- STR cash-flow comparison model — needs David's down-payment %, rate, STR nightly rate + occupancy assumptions
- Confirm 300 W 5th #204 STR minimum-stay term; resolve MLS# 4372926 duplicate
- Text Tibor re 525 E 6th #409 STR term (Monday)
- Email Phase 3 (optional): `create_block_rule` in `graph_mail.py` — decide build vs. leave manual
- Router self-audit at 192.168.0.1 — highest priority: port-forward/DMZ of 22/5900 to Mac mini
- Mac quick wins: disable full-disk remote access, Media/Printer Sharing
- Verify "Keep Downloaded" on iCloud Claude folder on both machines
- Cross-device end-to-end test (preference block in Settings, Keep Downloaded, laptop pickup test)
- Optional: ed25519 SSH key for claude-config push/pull
- Mac→homeserver SSH read path for m365-mcp (flagged multiple weeks — still unbuilt)
- Slather-Up: draft launch email, Medium articles, landing-page copy; ship public TestFlight link, privacy-policy page, first beta-recruit threads
- Confirm mirror-scheduled-tasks.sh ran unattended night of 6/13 (8 dirs, no _archive)
- Delete `daily-email-review/SKILL.backup.md` (swap verified, backup obsolete)

---

## Stats

| Category | Count |
|----------|-------|
| Projects with shipped work | 7 |
| Research/reference sessions | 2 |
| Open threads carried forward | 13 |

---

*Generated 2026-06-15 · Covers Mon Jun 8 – Sun Jun 14, 2026*

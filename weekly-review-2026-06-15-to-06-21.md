# Weekly Review — June 15–21, 2026

*Sources: Open Brain · Claude Code git history*

---

## Shipped This Week

### Slather-Up App Launch

Full organic launch marketing program built and site live (Cowork, Jun 20–21).

**Deliverables shipped:**
- Marketing Plan, Launch Content Kit (App Store metadata — title 27/30, subtitle 28/30, keyword field 94/100, description, promo text, 6 screenshot captions), Social Campaign (IG/TikTok/X calendar Jun 22–Jul 21 + all copy), Profile Setup Pack
- Social graphics (7 SVG/PNG assets), avatar
- Hub site `slatherup.app` LIVE (hub + `/demo` with demo video); `slather-up.app` 301-redirects to canonical domain
- Reddit beta posts (r/triathlon, r/running, r/SkincareAddiction) paste-ready with TestFlight link
- Product Hunt listing (tagline, description, maker comment, launch-day checklist)
- Email: hello@/support@/privacy@slatherup.app defined; M365 email intact
- Social handles secured: instagram.com/slather_up, tiktok.com/@slather_up, x.com/Slather_Up
- Daily scheduled task `slather-daily-social` (8 AM) surfaces each day's posts

**Open (before Jun 22):** Brand the three social profiles (avatar, bios), set link-in-bio to slatherup.app, warm up accounts with 2–3 non-promotional posts. DNS activation: update nameservers at registrar → add Pages custom domain → delete Squarespace A/www records; email aliases at registrar. Optional: wire X auto-posting via Zapier.

**Launch Day: Tue Jul 14** — swap primary CTA from TestFlight → App Store URL in index.html + demo.html, then redeploy on Cloudflare Pages.

---

### Email Automation — Triage Rebuild + Saturday Briefing Consolidation

**Email triage refactor (Cowork, Jun 19):**
- Rebuilt email-triage Cowork artifact with 5-bucket system (delete / file-action / file-fyi / block / keep), 9-category taxonomy, category tags per item
- Fixed latent `block_sender` bug (was called but not declared in mcpTools — had been silently failing since rollout)
- Repointed weekly-fyi-review → "Weekly Action" folder; saturday-newsletter-briefing → "Weekly FYI" folder
- First saturday-briefing-pdf generated: 4-page Weekly FYI briefing to iCloud Drive OUTPUTS/2026-06/

**Saturday briefing consolidation (Cowork, Jun 20):**
- `saturday-newsletter-briefing` now one consolidated run: reads Weekly FYI once → writes iPad `briefing.json` (with clickable source-link titles) → renders PDF to iCloud OUTPUTS/\<YYYY-MM\>/ → posts one-line chat confirmation
- `saturday-briefing-pdf` task retired (folded in)
- `weekly-fyi-cleanup` (Sat 08:00) auto-soft-deletes past-7-day Weekly FYI batch (recoverable ~30 days); first manual run: 43 items soft-deleted
- BRIEFING-FRAMEWORK.md saved and indexed in README.md
- Tailscale CLI wrapper installed at `/usr/local/bin/tailscale` (App Store variant needs wrapper script, not symlink)

**Open:** "Saturday Briefing" artifact has old Temp Folder URI hardcoded (~line 291 FOLDER_URI + refs at ~263/287/330/392) — needs repoint to Weekly FYI. Temp Folder backlog (~137 items) not yet migrated to Weekly Action/Weekly FYI. Pre-approval needed: hit "Run now" on daily-email-review + saturday-briefing-pdf once so scheduled runs don't pause on M365 permission prompts. Older Weekly FYI backlog (>7 days, ~100 items) — David go/no-go on one-time recoverable sweep.

---

### Mac Migration: Mini → M5 Laptop

Project stood up (Cowork, Jun 21):
- Things 3 project "Mac Migration: Mini → M5 Laptop" created with 29 to-dos across 6 phases, per-task commands in notes
- MAC-MIGRATION-CHECKLIST.md saved to iCloud Claude folder as the detailed reference

**Open:** Execution pending (working tracker = Things; reference = iCloud markdown).

---

## Research & Quick References

- **Home NAS pick** (Jun 21): Time Machine + shared drive, 6–12 TB usable, RAID 1. Lead pick: UGREEN NASync DXP2800 (Intel N100, 8 GB, 2.5GbE, ~$200–250, best value). Safe pick: Synology DS225+ (~$339, best-in-class DSM/Time Machine software). ~$600–750 all-in with two 12 TB drives (12 TB usable mirror). Note: Synology 2025 drive lock-in was reversed in DSM 7.3 (Oct 2025) — third-party drives are no longer a dealbreaker. Decision is David's.
- **Apple Passwords for Deena** (Jun 20): PDF guide created covering iCloud for Windows setup, browser extension, and migration from 1Password. Sent to Deena. Topic closed.
- **Shawn's podcast** (Jun 21): No podcast reference found across 144 sessions or Open Brain. Blocker: David to supply details or approximate session/date.

---

## Open Threads Carried Forward

- **Slather-Up DNS activation** — update nameservers at registrar, wait for Active, add Cloudflare Pages custom domain, delete Squarespace A/www records
- **Slather-Up email aliases** — set up hello@/support@/privacy@ forwarding at registrar
- **Slather-Up social profiles** — finish branding (avatar, bios from Profile Setup Pack), warm-up posts
- **Slather-Up asset bundle** — index/README consolidating all launch deliverables (pending David's go)
- **Email: artifact URI repoint** — "Saturday Briefing" artifact FOLDER_URI still points to old Temp Folder
- **Email: Temp Folder backlog** — ~137 items not yet migrated to Weekly Action/Weekly FYI
- **Email: pre-approval runs** — daily-email-review + saturday-briefing-pdf (prevent 4:54 AM pause)
- **Email: older Weekly FYI backlog** — >7 days, ~100 items; one-time recoverable sweep pending David go/no-go
- **Mac Migration** — execution of 6-phase checklist
- **Home NAS** — David's final pick (UGREEN vs Synology)
- **Charlotte STR cash-flow model** — needs down payment %, rate, STR nightly/monthly rate + occupancy assumptions from David
- **Charlotte STR: 300 W 5th #204** — STR min-stay term unconfirmed; duplicate MLS# 4372926 (shared with 804 Greenleaf) unresolved; text Tibor re: 525 E 6th #409 term

---

## Stats

| Category | Count |
|----------|-------|
| Projects with shipped work | 3 |
| Research/reference sessions | 3 |
| Open threads carried forward | 12 |

---

*Generated 2026-06-22 · Covers Mon Jun 15 – Sun Jun 21, 2026*

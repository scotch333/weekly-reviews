# Weekly Review — June 1–7, 2026

*Sources: Open Brain · Claude Code git history*

---

## Shipped This Week

### Sacred Colors

Flutter iOS coloring app (scotch333/sacred_colors). Major feature week.

**Shipped (June 3):** "Color your own photo" feature (#9) — build 1.0.0(4) uploaded to TestFlight.
- On-device photo-to-line-art conversion via pure-Dart XDoG converter (runs in isolate; photos never leave the device — preserves privacy promise for users photographing kids)
- Two-layer canvas architecture: fill + brush layer + black edge-pencil that closes gaps so tap-fill seals; line layer kept separate from color for independent future shareability
- My Photos gallery tab + Add Photo picker
- User pages persist on-device under `documents/user_pages` (line layer only, never the original photo)
- Hardening pass: converter quality tuning, flood fill moved off main thread, 1200px resolution cap, rename/delete user pages, a11y, 29 tests green, project CLAUDE.md + ADRs

**Also shipped (June 1):** Line-art authoring tooling — `tool/lineart/` on branch `claude/nice-davinci-C9sq4` (pushed, not yet merged). Turns source artwork into coloring pages that satisfy the app's flood-fill engine contract. Three Dart files (scorer.dart, validate.dart, clean.dart) + `extract/extract.py` for photo-to-line-art conversion.

**Open items:**
- David's hands-on test of the photo→coloring flow on real device (TestFlight build 1.0.0(4) pending)
- Line-art tooling branch needs merge/PR to main
- Compose step (caption/footer text rendering) not yet built
- Phase 3 (higher-quality ML conversion via Core ML/TFLite) — ON HOLD; do NOT use cloud to preserve privacy promise
- Phase 4 (community sharing of user line art) — deferred; needs product decisions on moderation, identity, policy

---

### Retirement Tracker iOS

SwiftUI app (scotch333/retirement-tracker-ios) that ports David's retirement-planning Excel workbook into a native app.

**Shipped (June 2):** Build 3 (version 1.0) uploaded to TestFlight — closes the entire beta-feedback backlog:
- P0: numeric-entry keyboard loop (#7/#8/#9/#13, ADR-0011)
- Edit/delete cluster: Log snapshots (#11), base-year balance in Accounts editor (#10, parity-tested), delete accounts (#3), goal edit verified (#2), day-level Log dates (#12). ADR-0012 (shared edit/delete affordance pattern)
- Chart year-axis auto-ranging from zero (#6)
- Reminder cadences expanded to Off/Daily/Weekly/Monthly/Quarterly with always-visible inline picker (ADR-0013)
- Swipe nav (#5) and onboarding (#4) verified already shipped in build 2
- Fixed pre-existing iPad iOS 26.5 SwiftData test crash (dangling ModelContainer in 5 test suites); test matrix now covers both iPhone 15 Pro/iOS 17 and iPad Pro 11" (M5)/iOS 26.5

HEAD: 97f0188 on origin/main.

**Open items:**
- Confirm build 3 finished processing in TestFlight and release to testers (especially iPad testers who hit the crash)
- Watch for new round of tester feedback
- Phase-2 chart work (log actual contribution per balance snapshot for exact gains-vs-contributions split)
- App Store screenshots + FAQ/Help section before public submission

---

### Slather

iOS sunscreen-reapplication reminder app (scotch333/slather). SwiftUI/MVVM — fetches UV index by location, lets you pick SPF, counts down to reapply, fires local notification.

**Shipped (June 2):** Build 1 (version 1.0) to TestFlight — first successful archive, sign, and upload. "Day 8" of the project.
- Key decision: Apple WeatherKit chosen over WeatherAPI.com as UV provider — WeatherKit ships no extractable API key in the binary, preserving security posture (ADR-0009)
- App Store Connect record: "Slather-Up" (bundle ID DDP.slather, team 7687WS6WZ7)
- Internal TestFlight testing group "Slather Test" wired up with invite sent

**Open items:**
- Accept TestFlight invite on device and test live WeatherKit UV fetch on real hardware
- Store metadata, screenshots, privacy-policy URL, and App Privacy questionnaire before public App Store release

---

### Claude Code Infrastructure

Two pieces of Mac-side tooling shipped June 2.

**Idle notification hook:** Installed `~/.claude/hooks/notify-idle.sh` + Notification hook in `~/.claude/settings.json`. Sends iMessage to david@ddprice.com when Claude Code sits idle ~60 seconds waiting on input or a permission approval. Filters to the idle/"waiting" event only (doesn't ping on every permission prompt). AppleScript → Messages; permission already granted, test iMessage sent successfully.

**Supervisor Approval Policy:** Rule-based PreToolUse gate (`~/.claude/hooks/supervisor.py`) in global settings — auto-approves local/reversible ops, pauses for David's approval on consequential actions, hard-blocks catastrophic commands. Three tiers:
- **ALLOW (auto):** reads, greps, file edits, git status/diff/log/pull/fetch/add/commit, builds and tests, safe-rm
- **ASK (manual):** git push, destructive git working-tree ops, sudo, fastlane/pilot, xcrun altool/notarytool, gh release, npm publish, Supabase deploy/migration, keychain ops, pipe-to-shell, secret file edits
- **DENY (blocked):** direct `rm`/`rmdir`/`unlink` (must use `~/.claude/bin/safe-rm` instead), catastrophic commands, force-push to protected branches

`safe-rm` moves targets to `~/.claude/.trash/<timestamp>/` instead of destroying — nothing permanently deleted until David empties the trash. Tested 18/18 decision cases. Applied globally; per-project `.claude/settings.json` can override.

---

### M365 MCP Server

Personal M365 MCP server (scotch333/m365-mcp) that exposes quarantine management tools (`list_quarantine`, `release_quarantine`, `delete_quarantine`).

**Shipped (June 2, session end):** Final cleanup and persistence locked in.
- Repo pushed to private GitHub (initial commit 9df47b5 on main)
- Homeserver deploy key (`~/.ssh/m365-mcp-github`) + `Host github.com` block in `~/.ssh/config`; `git push` from homeserver works non-interactively
- Housekeeping: removed `~/exo-debug`, uninstalled ExchangeOnlineManagement 3.2.0 side-by-side install, removed `/tmp/m365-mcp-stage` staging dir
- Tools confirmed working end-to-end in Claude Code, Claude.app chat, and Cowork

**Open items:**
- Next-surface expansion: Mail/Calendar/SharePoint via Graph (msal + httpx); app permissions already granted, no additional directory role needed

---

## Research & Quick References

- **Sacred Colors — art licensing caution:** churchofjesuschrist.org / Intellectual Reserve imagery (including temple photos from official sources) is almost certainly NOT licensed for redistribution in a third-party App Store app. Cleanest sources: original owned art, true public-domain/CC0 works, or commissioned/traced original line drawings. AI-generating specific recognizable temples muddies derivative-work/licensing picture.

- **Sacred Colors — flood-fill engine contract documented:** Outlines are walls only when R,G,B all < 80 (gray/colored lines leak); fill spreads within ±20 tolerance (shading/gradients stall fills); any single-pixel gap leaks and floods the whole page. Constants `kOutlineThreshold=80` and `kTolerance=20` mirrored from app into `tool/lineart/scorer.dart` — must stay in sync if the app changes them.

- **Sacred Colors — line-art pipeline mental model captured:** Two distinct jobs: CONVERSION (find the lines) then CLEANUP (enforce engine contract). Flow: `photo → extract/extract.py → gray line art → clean.dart BINARIZE mode → validated page`. For existing line drawings, skip extraction. Biggest quality lever for busy backgrounds: `--denoise 3-4` plus tight-cropping the subject.

---

## Open Threads Carried Forward

- Sacred Colors: David hands-on test of photo→coloring flow on real device (TestFlight build 1.0.0(4))
- Sacred Colors: Line-art tooling branch (`claude/nice-davinci-C9sq4`) — needs merge/PR to main
- Sacred Colors: Compose step for captions/footers not yet built (clean.dart reserves the bands but doesn't draw text)
- Sacred Colors: Phase 3 higher-quality conversion (Core ML/TFLite) — on hold; preserve on-device privacy promise
- Sacred Colors: Phase 4 community sharing — deferred pending product decisions (moderation, identity, policy)
- Retirement Tracker: Confirm build 3 processing complete; release to TestFlight testers
- Retirement Tracker: Watch for and process new beta feedback round
- Retirement Tracker: Phase-2 chart (log actual contributions per snapshot for exact gains split)
- Retirement Tracker: App Store screenshots + public submission prep
- Slather: Accept TestFlight invite; verify live WeatherKit UV fetch on device
- Slather: Store metadata, screenshots, privacy policy before public App Store release
- M365 MCP: Expand to Mail/Calendar/SharePoint via Graph API

---

## Stats

| Category | Count |
|----------|-------|
| Projects with shipped work | 5 |
| Research/reference sessions | 3 |
| Open threads carried forward | 12 |

---

*Generated 2026-06-08 · Covers Mon June 1 – Sun June 7, 2026*

*Note: GitHub MCP access was scoped to scotch333/weekly-reviews only this run. Git history for scotch333/sacred-colors, scotch333/retirement-tracker-ios, scotch333/slather, scotch333/m365-mcp, scotch333/podcast-survey, scotch333/podcast-analysis, scotch333/3d-print-queue, and scotch333/costco-rebate could not be pulled directly — Open Brain is the complete source for this week.*

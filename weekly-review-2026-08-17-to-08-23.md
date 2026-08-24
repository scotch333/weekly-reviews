# Weekly Review — August 17–23, 2026

*Sources: Open Brain · Claude Code git history*

---

## Shipped This Week

### ops-runbook / job-monitor / Homeserver Infrastructure

A week-long infrastructure thread covering a homeserver outage, a 3-week briefing silence, and a scheduler migration.

- **Homeserver outage (2026-08-15/16)** root-caused and recovered remotely — reconciled two sessions' contradictory root causes into one account; corrected a wrong outage duration (~8h) before it propagated further. ops-runbook 88bda6c.
- **ops-runbook PR #9** merged (b681782) — `process/parallel-sessions.md` + homeserver LAN recovery procedure in `overview.md`; five blockers fixed in the code-reviewer gate pass, including a LAN-address row still pointing to the dead Ethernet interface.
- **Saturday morning briefing silence (3 weeks) root-caused and fixed** — Cowork task hung 1848s on an unanswered `store_briefing` permission prompt; fixed via `permissionMode: auto` + corrected `approvedPermissions`; David applied 2026-08-15 11:36, has now survived four registry rewrites. Added `~/.claude/bin/fix-saturday-task-permissions.py` (claude-config 7662787, hardened 33d62c4).
- **job-monitor PR #2** merged (12dc98a) — host-outage rollup, 1 MiB read cap + 30s read deadline, hermetic heartbeat-guard test via `--check`, MAX_DETAIL tested, placeholder-recipient guard; tests 49 → 72.
- **job-monitor PR #6** merged (771c0d5) — funnel-watchdog publishes `hb-funnel-watchdog.json` on verdict runs, withholds it otherwise; `deploy/homeserver/` adopts 144 previously unversioned lines that restart the tunnel.
- **ops-runbook scheduler attribution corrections** — `weekly-action-archive` and `weekly-receipts.json` re-attributed to the correct cloud routine; `weekly-action-archive` reclassified as on-demand; tax-archiving stop documented.
- **Saturday newsletter briefing migrated from Cowork to a claude.ai cloud routine** — trigger `trig_01CLz1dCe9fBD43Uwu3BFwnj`, cron `50 13 * * 6`; verified end-to-end (23 items published, texted from vesper, `store_briefing` 753ms). Cowork task disabled and confirmed surviving reboot via `claude-config/bin/disable-saturday-cowork-task.sh`. **job-monitor PR #8** merged (7bd509d), manifest repointed; tests 72 → 74.

---

### Slather (iOS)

ADR-0025 StoreKit/subscription capstone shipped in three PRs.

- **PR #20** — `HomeView` missing `import StoreKit`; the #14 rating prompt was compiling on an unimported StoreKit environment value.
- **PR #19** — ADR-0025 T4: first checked-in shared scheme (`slather.xcscheme`), `Slather.storekit` added to project (was on disk but not in the project), subscription group id reconciled `group.DDP.slather.premium` → `DDP.slather.premium`.
- **PR #21** — ADR-0025 T3: `restore()` no longer reports "Restored" to founders with zero purchases; new `PremiumCopy` replaces raw `error.localizedDescription`.
- Open: issue #17 (privacy-policy content refresh) filed but not yet done.

---

### claude-config

- Built `skills/google-dev-voice/SKILL.md` encoding the voice-and-tone rules from Google's developer documentation style guide (decoded from a TikTok capture).
- Added 27-line always-on voice section to `CLAUDE.md` pointing at the skill as authoritative.
- Moved `scheduled-tasks/` out of `~/.claude` into `claude-config`, symlinked it back, added to `install.sh`'s link loop.
- Committed 3ec7090, pushed to `scotch333/claude-config` master.

---

### cube-scrambler

New hardware project kicked off and scaffolded (Aug 20):

- MIT license, safety layer with tests, corrected BOM, measured filament figures.
- Motor moved to a 23mm body and re-rendered.
- 9 commits, latest b39262c.
- Added to portfolio in AI-Project-Backlog.

---

### Topper (iOS — "Topped")

Major week: accessibility, watch face, HealthKit, release signing, App Store submission, and rejection recovery — all in three days.

**Accessibility + Dynamic Type (Aug 21, build 7):**
- **PR #25** merged — Dynamic Type.
- **PR #27** merged (a5f6942) — `.accessibilityLabel` on "Add a size" field, `.accessibilityHidden(true)` on trailing unit Text in `GlassSizesCard.swift` and `SettingsView.swift`; 57 VoiceOver stops → 55, zero unlabelled stops; closed #26.
- Modal-focus and cycle-log fixes (ad28058). Build 7 to TestFlight.

**Watch face, HealthKit, IPA gate (Aug 22–23, build 8):**
- **PR #39** merged — HealthKit ingest moved off main thread via `@ModelActor` (ADR-0016); SwiftData ghost-row fix in `HydrationStore`; closed #35.
- **PR #41** merged — watch-face complication as WidgetKit appex reading an App Group snapshot (ADR-0017); closed #37; verified live on the simulator pair.
- **PR #42** merged — pre-upload IPA gate (`scripts/verify-ipa.sh` wired into `archive.sh`) + manual release signing (ADR-0018). Build 8 to TestFlight (VALID), release notes live.

**App Store submission + rejection recovery (Aug 23):**
- Topped 1.0.0 submitted to App Review (reviewSubmission `4315132d`) — all listing metadata written via ASC API (subtitle "Weather-smart water reminders", 1297-char description, keywords, 7 screenshots).
- Build 8 rejected under ITMS-90111 — root cause: archived on the macOS 27 public beta laptop; Apple rejects beta-OS `BuildMachineOSBuild` stamps.
- Mac mini provisioned as release build machine (signing identities + WWDR G3 in a dedicated `build.keychain`, profiles, ASC key, watchOS platform download).
- Build 9 archived on the mini (`BuildMachineOSBuild 25F80`, gate PASS, zero debug-fixture strings), uploaded, resubmitted → **WAITING_FOR_REVIEW**.
- Stale #29 closed.
- Open: #38 Voice Control input-label sweep.

---

### Lanterns (iOS)

Deep links, test isolation, and Fuse polish wrapped up the week.

**Aug 21:**
- Deep links for In-App Events shipped (#151, ADR-0034; commit 23884dd).
- Game hub card title wrap/clip fix (#141/#165; commit 45966e1).
- Settings sheet view-model + Game Center test isolation fix (#164).

**Aug 22–23:**
- **#122 and #124 closed** on David's on-device verification (Fuse control row + difficulty ladder).
- **PR #171** merged (f282071) — deep-link cluster #167–170: `DeepLink.swift` moved to `Support/`, `DeepLinkRouter` `@MainActor`, hub selection state extracted to `GameHubNavigationModel` (13 tests); link over a live game cover now parks the destination and presents from `fullScreenCover(item:onDismiss:)`. Verified live twice via `simctl openurl`.
- **PR #172** merged (fde0b0b) — `UpdateAvailabilityService.live()` resolves to `NoopAppStoreVersionLookup` under XCTest (#163, ADR-0027 amended); pinned in `TestEnvironmentTests`.
- **PR #173** merged (07c169c) — Fuse drag haptics coalesced to one tick per touch sample via `extend(through:)`; corner-cut bridge resolves from where the chord crossed the column boundary vs the shared vertex (#138/#139, ADR-0028 amended). New geometry test verified failing on the old heuristic before trusting it.

---

### Lanterns-Android

Target API upgrade and four parity features landed ahead of the Play 2026-08-30 deadline.

**API upgrade + landscape (Aug 22):**
- targetSdk/compileSdk 35→36, AGP 8.5.2→8.9.2, Gradle 8.9→8.11.1, versionCode 5→6.
- `BoardStage.kt` caps all seven board surfaces to `min(available width, content height − chromeReserve)`; `verticalScroll` added to `PuzzleScreen.kt` and `SudokuGameScreen.kt`; `StartGate.kt` restructured; portrait lock removed from `AndroidManifest.xml`.
- `LandscapeLayoutTest` (7 tests); ADR-0018.

**Four parity PRs (Aug 22–23):**
- **PR #77** — hint UX parity: `HintEngine.explain`/`forcedUnplacedUnit` direct-ported from iOS, coaching messages, 2.5s flash ring with TalkBack suffix, 10s cooldown (ADR-0019).
- **PR #78** — Undo: snapshot history capped at 40 on `GameState`, never refunds hint cost/cooldown; control row → `FlowRow`.
- **PR #79** — archive replay: `isReplay` mode structurally cannot write (spy-testable recorder), per-entry nonce for fresh boards (ADR-0020).
- **PR #80** — process-death restore via required-nullable `SavedStateHandle`.

**Open-issue run-through (Aug 23):**
- **PR #84** (e13f341) — game scaffold display cutout padding via shared `gameContentInsets`.
- **PR #86** (7fdf583) — onboarding CTAs pinned as non-scrolling footer, above fold at any font scale.
- **PR #87** (0a11293) — non-game scaffolds + TopAppBars get cutout padding.
- **PR #88** (3703d78) — tutorial routing fixed (pins route forward, completion state included), mutation-tested landscape guards.
- Instrumented tests (emulator) restored as a required check on main after 8 green boots; closed #81.

---

### Apple Music Library

One-time library cleanup (Aug 17):

- Merged 9 playlists (Boat, Car Ride, Dhdh, New, Pool, Plane, Shower, Top, Untitled Playlist) into "On the Go" (85 tracks, including Roxette's 5 songs).
- Merged Dashing into Get Moving (44 tracks, shared hard-rock/metal artist pool).
- Deleted 5 stale playlists: New Year, Mamma Mia, Homecoming, 45, 25.
- Identified 17 near-identical duplicate tracks library-wide; not removed (David declined). Duplicates playlist deleted at David's request; tracks remain.

---

### watchlist

Weekly digest delivery wired up (Aug 23):

- Added delivery step to `~/projects/watchlist/wl.py` digest: `scp` to mini's `~/server/briefings/public/` + text via imessage-mcp (top picks + Funnel link); verified end-to-end (real text sent, page live).
- Old Cowork presentation task retired (was failing on pre-migration iCloud path).
- Karen-reviewed; all must-fixes applied (exit 1 on delivery failure, stderr/body in errors, SSE parse assertion, 30s retry per leg, `--no-notify`/`WL_NO_NOTIFY` de-risked).

---

## Research & Quick References

- **google-dev-voice (TikTok capture, Aug 17)** — How to stop Claude writing in "Claudish" (poetic language, fake warmth): build a skill from the Google developer documentation style guide — active voice, present tense, second person, conditions before instructions, conversational but never cutesy. Effective as a mid-conversation correction, not just an upfront instruction. Source: @nate.b.jones TikTok, Aug 2026.

---

## Open Threads Carried Forward

- **Slather** — Issue #17: privacy-policy content refresh (filed, not yet started).
- **Topper** — Issue #38: Voice Control input-label sweep (filed, not yet started).
- **Lanterns (iOS)** — Deep link and test isolation work ongoing; additional open issues in the backlog.

---

## Stats

| Category | Count |
|----------|-------|
| Projects with shipped work | 9 |
| Research/reference sessions | 1 |
| Open threads carried forward | 3 |

---

*Generated 2026-08-24 · Covers Mon Aug 17 – Sun Aug 23, 2026*

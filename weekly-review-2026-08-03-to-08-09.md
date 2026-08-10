# Weekly Review — August 3–9, 2026

*Sources: Open Brain · Claude Code git history*

---

## Shipped This Week

### Lanterns (iOS)

**Monday Aug 3 — 1.8 build 21 released:**
- PR #118: bumped to 1.8 build 21
- PR #117: refreshed App Store screenshot set for 1.8
- PR #116: result shares now embed the App Store link in share text (issue #111)
- PR #115: ADR-0020 amended — board taxonomy locked to four streak boards (Fireflies' board documented)

**Friday Aug 7 — 1.9 with Fuse (game #5) cut:**
- PR #121: Fuse shipped as game #5 (full module: FuseTypes, FuseSolver, FuseGenerator, 6 views); ADR-0028 records the position-domain solver design; 797 tests green
- PR #126: pre-start gate (ADR-0029) — daily opens behind a Begin button; solve clock starts on Begin, not first tap, closing a leaderboard-integrity hole
- PR #128: App Store screenshots refreshed to 9 shots including Fuse + five-game hub
- PR #129: Fuse Game Center best-streak leaderboard created via ASC API and wired; ADR-0020 updated four→five boards
- PR #131: bumped to 1.9 (build 22); What's New copy updated
- commit b699add: in-app prompt when a newer version is on the App Store (ADR-0027)
- Build 22 archived, IPA verified, uploaded to TestFlight (VALID), set to Personal QA

**Saturday Aug 8 — Fuse made fully playable, build 23:**
- PR #136: fixed Fuse charges rendering in wrong squares (nil optional view in SwiftUI stack); fixed fast drag drawing nothing (FuseBoardGeometry.cellsCrossed now walks between touch samples); board re-themed
- Walls added on Fuse Medium/Hard (ADR-0030): placed on non-solution edges only, solver treats walls as disallowed edges, BFS-through-walls replaces Manhattan distance; levels retuned (Easy 18 charges/0 walls, Medium 10/6, Hard ~4/13)
- Build 23 uploaded to TestFlight (VALID); ASC metadata corrected (walls bullet in What's New, stale screenshot replaced)
- Opening screen decluttered — overflow menu, hub gear, honest streak (#143/#145)
- 872 tests green

**Sunday Aug 9 — Fuse difficulty retuned, build 25 submitted to App Store:**
- PR #147: Fuse difficulty retune across all three levels from measured hardware data; version bumped 24→25
- Build 25 uploaded to TestFlight (VALID), promoted to Friends & Family (9 testers)
- App Store submission swapped from build 24 to build 25 (build 24's release notes already described the retuned difficulty — shipping mismatched binary was the alternative); submission 0e8910c4 WAITING_FOR_REVIEW
- issue #27 (daily solve recording) closed via Lanterns-Android fix
- Open: #148, #124, #135; App Store under review

---

### Lanterns-Android

**Friday Aug 7 — 10-PR catch-up sprint:**
- Settings surface (#30/#35: Sudoku as game #2, ADR-0005)
- GameHub nav + shared solve banner (#36, ADR-0006)
- First-launch onboarding + interactive tutorial (#37, ADR-0007)
- Daily reminder + streak-warning notifications (#38, ADR-0008)
- Practice mode — disposable boards isolated from daily (#39, ADR-0009)
- Haptic feedback across both games (#40, ADR-0010)
- Fireflies as game #4 (#42, ADR-0011)
- Leaderboard seam + submission points + silent restore (#43, ADR-0012)
- CI: build + unit tests on every PR (#44); main branch protected with unit job required (#45)

**Saturday Aug 8 — Achievements, Ember Trail, CI stall fix:**
- 16 achievements shipped with local authoritative ledger (#16/#46)
- Ember Trail daily share, frozen time, gated Play Store link (#17/#47)
- CI stall root-caused and fixed: seed+launch combined into one method so CI can't wipe the seed (#49); two flaky achievement test timeouts widened (#48)
- ADRs 0004–0012 locked

**Sunday Aug 9 — Concurrency fix, CI hardening:**
- Root-caused #49: unbounded thumbnail fan-out starving Dispatchers.Default; capped ArchiveViewModel at limitedParallelism(1) (5039fdb, closing #50); ADR-0014 documented (fb28a50)
- Emulator tests promoted from continue-on-error to blocking required (6cd430e, #54); branch-protection escape hatch fixed (faf58cf, #55)
- Fixed daily solve recording under board's day-key not wall clock (ebf354e, closing #27)
- Portrait locked to stop landscape hiding controls (3e8fd5e, closing #33)
- Cached solve-history parse + closed Archive thumbnail race (07962fa, closing #51/#52)
- Open: #51, #52

---

### Topper (iOS + Apple Watch hydration app — new project)

**Wednesday Aug 5 — Scaffolded:**
- Initial build committed (1aa4afc) at ~/projects/topper using ios-kit modern stack (Swift 6, @Observable, SwiftData)
- Five ADRs written: ADR-0001 (stack), ADR-0002 (hydration goal engine: 35 ml/kg + activity + heat/UV), ADR-0003 (WeatherKit), ADR-0004 (HealthKit dietary water), ADR-0005 (syncIdentifier idempotency)

**Thursday Aug 6 — Build 1 to TestFlight, Friends & Family group live:**
- Build 1.0.0 (1) verified VALID in TestFlight via ASC API
- Personal QA (hasAccessToAllBuilds=true) and Friends & Family (9 testers) beta groups created, mirroring the Lanterns two-group pattern
- Build 1 released to Friends & Family; privacy policy URL set (scotch333.github.io/topped-legal)

**Friday Aug 7 — Builds 2 and 3 shipped same day:**
- Build 2: body weight now honours mass unit (SettingsStore.MassUnit + MassFormatting, US locale defaults to pounds); fixes only piece of build-1 TestFlight feedback
- "I drank it" action on sip nudge (ADR-0008): wrist logging via iOS forwarding a mirrored notification's custom action back to phone; DrinkLogger extracted as single log path
- Build 3: first build with watchOS app (ADR-0009); topperWatch logs dietaryWater straight to HealthKit; HealthIngestService pulls watch drinks into SwiftData via anchored query; WatchSyncPublisher pushes goal+total to wrist
- Nudge re-arm bug fixed (reconcile() re-derived overdue state but never re-scheduled)
- Privacy policy updated in both places (docs/privacy.html + scotch333/topped-legal) to disclose new dietaryWater READ scope
- 103 tests / 12 suites green

**Saturday Aug 8 — Build 4:**
- Sip-nudge crash fix + wrist notification delivery (ADR-0010)

**Sunday Aug 9 — Build 4 released to Friends & Family:**
- English release notes published on build 4 (testers coming from build 1, leads with weight-unit correction)
- Build 4 assigned to Friends & Family group; wrist test passed on real hardware
- Issues #11 and #12 closed; #19 filed for unexplained watch battery drain
- Open: #19 (battery drain), #20, #21

---

### smash-stack (new project, Sunday Aug 9)

Scaffolded from zero in a single session:
- ADR-0001: Unity over Godot
- ADR-0002: structural reading + par as the core hook
- ADR-0003: levels are code, not scenes
- Unity toolchain pinned at 6000.3.21f1 (iOS-only); license activated and verified via headless launch
- Greybox prototype shipped: 3 archetypes, par scoring, validation harness, view-mode cycling (side/three-quarter/front)
- Added to portfolio in AI-Project-Backlog (#9)
- Open: next milestone not yet committed

---

### Slather (iOS)

- **Aug 5**: ADR-0026 HealthKit syncIdentifier idempotency fix merged to origin/main — same-day fix of a latent externalUUID bug surfaced by Topped's code review (externalUUID does NOT enforce uniqueness per HKMetadata.h)
- Open: #11–14; device verification rides #13

---

### claude-config

**Saturday Aug 8:**
- Skill surface cut 78 → 49 by disabling five Cowork plugins (engineering, marketing, design, productivity, zapier — 33 skills)
- Merge gate now enforced by supervisor.py instead of prose: asks on `git push` / `gh pr create` / `gh pr merge` when no reviewer agent completed this session
- CLAUDE.md trimmed 190 → 111 lines
- Four new tools: `/ship` (branch→review→commit→push→PR), `/setup-server` (non-iOS analog of /setup-ios), `skills/homeserver`, `skills/article-candidates`
- `bin/config-audit.py` measures skill usage and classifies each surface BURST/EMERGING/DURABLE/INTERMITTENT/DORMANT
- ADR template and SUPERVISOR-POLICY.md moved to claude-config root, shared by both kits
- Open: #1, #2, #3, #5, #6, #7, #8

---

### notes-reviewer (imessage-mcp)

**Saturday Aug 8:**
- Closed osascript argv-injection security hole (and other review findings)
- Added `create_things_todo` + a Things button on review pages
- RCE test wired to the real code path (not a mock)
- Hardened the carry-over map against inherited keys
- Warns before filing an action item whose Things task was never created

---

### ops-runbook

- **Aug 8**: Documented Mac app automation on the mini: Apple Events, Things 3, the argv-injection hole (commit 5550556)
- **Aug 9**: Decision 0002 recorded — weekly review action items become Things tasks (4df53b6)

---

## Research & Quick References

- **TikTok capture backlog hygiene** (Aug 3) — Five technique notes from working the capture queue: TikTok slideshow posts (carousels) can be read end-to-end by screenshotting auto-advancing slides, no login or transcript tooling needed. Unnamed-clip movie accounts withhold titles as engagement; identifiable from video frames (~2 min via TMDB). Cluster hygiene rule: when a queued capture gets done, re-point it to its own row so the cluster count is accurate. Claude Code's `/doctor` four-item scope is a creator's unverified claim, not confirmed against the command.
- **AI context instruction files** (captured Aug 7) — Three independent creators converged on the same structural point: always-resident instruction files re-read every turn cost tokens and dilute attention. Decision rule: convert instructions you INVOKE, keep resident the ones that CONSTRAIN unconditionally (safety gates, supervisor policies must stay resident; wrap-up routines are exactly the convertible kind).
- **capture-system domain gap** — Physical/workshop builds have no home in the Open Brain capture system (`create_build` is 3D-print-only, shelf domain enum has no fabrication value). Noted; no fix landed.

---

## Open Threads Carried Forward

- **Tax organizer (2025 return)** — Second reminder from Account Sense PLLC received Aug 3; questionnaire still not submitted
- **Charlotte condos — 300 W 5th #204** — STR term and duplicate-listing question open with Tibor
- **Email rules .ps1** — written and delivered; awaiting David to run via `Connect-ExchangeOnline`
- **Deena's 8 duplicate Outlook rules** — `delete_message_rule` proposal awaiting David's go-ahead
- **Lanterns Daily LinkedIn launch post** — blocked on David's call re: "Queens" comparison and App Store link placement
- **MacBook Air crash remedy** — Beta Updates off vs. clean downgrade still undecided
- **Lanterns (iOS)** — App Store submission WAITING_FOR_REVIEW; #148, #124, #135 open
- **Lanterns-Android** — #51, #52 open (Archive thumbnail race resolved but follow-up issues remain)
- **Topper** — #19 watch battery drain (leading hypothesis: notification cadence waking the device), #20, #21 open; F&F device feedback pending
- **Slather** — #11–14 open; device verify on #13
- **smash-stack** — Greybox prototype live; next milestone not committed
- **claude-config** — #1, #2, #3, #5, #6, #7, #8 open
- **Game Center friend-sharing Q** — David's question from Aug 7 was interrupted mid-session and went unanswered
- **notes-reviewer** — 3 FUZZY items still awaiting David's answers (Deena $ / giving note, home-automation dashboard scope, "Mosiah also fled")

---

## Stats

| Category | Count |
|----------|-------|
| Projects with shipped work | 8 |
| Research/reference sessions | 2 |
| Open threads carried forward | 14 |

---

*Generated 2026-08-10 · Covers Mon Aug 3 – Sun Aug 9, 2026*

# Weekly Review — August 10–16, 2026

*Sources: Open Brain · Claude Code git history*

---

## Shipped This Week

### Slather — T2 Paywall, Privacy Ship Gate Cleared, App Store Rating Prompt

- **PR #16 merged** — ADR-0025 T2 paywall UI complete: paywall UI, Settings premium section, locked affordances, milestone nudge.
- **PR #18 merged** — two commits: `privacyURL` set to the already-ASC-registered URL (clears Guideline 3.1.2 ship gate, closes #12); App Store rating prompt shipped (closes #14) — new `RatingPrompt.swift` + `HomeViewModel+Rating.swift`, 19 hermetic tests. Ship gate now T6 App Store Connect products only. Full suite: 439 tests / 0 failed.
- Filed issue #17 (refresh privacy-policy content for paid app). Deleted 4 stale local + 3 merged remote branches.
- Open: issues #11, #13, #17.

---

### job-monitor — New Repo, Staleness Monitor Live

- **Repo created** (Aug 13) — staleness checker for ~26 jobs across five schedulers (2cd7d61); deployed with routine prompt, heartbeat wrapper, and deadman plist.
- **PR #1 merged** (70c0f59) — closes every silent-failure path: host-outage rollup, 1 MiB read cap + 30s deadline, hermetic `--check` guard, MAX_DETAIL tested, placeholder-recipient guard, triggers off /tmp; tests 49 → 72. Two deadman.sh bugs fixed.
- **PR #2 merged** (12dc98a) — published-payload coverage for `daily-inbox-review` + `saturday-zillow-briefing`; rollup classification bug fixed; reads bounded by time; docs synced to 25 jobs; expected_jobs 16 → 18.
- **PR #6 merged** — funnel-watchdog now publishes `hb-funnel-watchdog.json` on verdict runs only; deploy/homeserver/ adopts 144 previously unversioned lines that restart the tunnel; expected_jobs → 19.
- `~/server/funnel-watchdog/check.sh` rewritten and deployed on the mini: multi-resolver, tailnet-IP rejection, restart cooldown, state off world-writable /tmp.
- Cloud monitor routine created and attached; kept disabled — sandbox can't reach Funnel/RemoteTrigger (issue #5).
- Open: issues #3, #4, #5.

---

### ops-runbook — Saturday Briefing Root Cause + Homeserver Outage

- **Saturday briefing silence root-caused** (67fd1e8, 29dba11) — Cowork task fired on cron but hung 1848s at an unanswered `store_briefing` permission prompt; fix applied Aug 15 (permissionMode auto + corrected approvedPermissions); survived four registry rewrites since.
- Added `~/.claude/bin/fix-saturday-task-permissions.py` (claude-config 7662787, hardened 33d62c4) — idempotent, self-backing-up, refuses to run while Claude is up.
- **2026-08-15/16 homeserver outage diagnosed and fixed remotely** — Wi-Fi not joined to the tailnet while Ethernet NIC had negotiated 1000baseT but couldn't ARP the gateway; no physical access needed.
- PR #9 merged (b681782) — `process/parallel-sessions.md` + LAN recovery procedure in overview.md; five code-reviewer blockers fixed before merge. Reconciled two sessions' contradictory root-cause accounts into one on master.
- `weekly-portfolio-survey` cron corrected (d564c1b) — was `0 7 * * 1`, corrected to `0 22 * * 0`.
- Weekly-action.json and weekly-receipts.json re-attributed to the cloud routine that actually writes them; weekly-action-archive reclassified as on-demand (not broken).
- Open: issues #2, #3, #7, #8.

---

### open-brain-mcp — Watchlist Destination + Supabase v17

- **'watchlist' capture destination shipped** (migration 20260814000000, deployed Aug 14/15 as 20260815004211) — widens `captures_destination_kind_check` to 8 values; 4 rows moved off `none`+`trakt:watchlist`; `landed_at` fixed (was keyed on `destination_ref` alone). Edge function deployed as Supabase v15 (also shipped the shelf-identity changeset that had sat undeployed 11 days).
- **Supabase v16 → v17 deployed** (Aug 16) — `20260815000000_none_clears_landing.sql` applied in corrected order: cleans 2 rows, 18 real landings intact, backup RLS on.
- `decide_capture` now resolves the shelf row for every project destination and prints the shelf's own `shelf_id`; 'none' clears `destination_ref` and `landed_at` instead of leaving them.
- Five worktrees consolidated onto main (16 commits, 0ef23cc..5d909e5); stale worktree `sad-allen` closed.
- Issues #9, #10 filed; issue #8 corrected.
- Open: issues #8, #9, #10.

---

### Topped / Topper — Build 5 + 6, Custom Glass Sizes, Friends & Family

- **Aug 10** — Build 5 uploaded to TestFlight (VALID); durable ASC client (`scripts/asc.rb`) + TestFlight feedback puller (`scripts/testflight-feedback.rb`) shipped (565d447/c7f918c, closes #21); fixed fabricated `pairedAppleWatch=false` (ASC returns null); hardened sip-nudge shared log token and fixed path traversal + terminal-escape (OSC) sanitization holes in ASC scripts — 10 commits.
- **ADR-0012 (custom glass sizes) shipped** via PR #24 (merged Aug 11, 5cdb930) — `GlassSize` converted from an oz-keyed enum to a value type over `amountML` (Double); presets preserved bit-for-bit; custom sizes clamped 15–120 mL; existing users see no behavioral change.
- **Build 6 (1.0.0(6)) to TestFlight** (Aug 12, VALID) — first build carrying ADR-0012; phone + watch verified from IPA before upload.
- **Build 6 assigned to Friends & Family** (Aug 12) — group now holds builds 1, 4, and 6; delivery confirmed by David.
- en-US release notes live (2,001 chars), written for testers coming from build 1 or 4; battery-watching ask included to harvest real-world data.
- Open: issue #19 (watch battery drain — symptom stopped presenting but nothing measured), #20, #21.

---

### Lanterns — App Store Custom Product Pages + Build 25

- **Promotional text live** on the default product page (five-game line, 152/170 chars).
- **5 custom product pages submitted for review** (reviewSubmission 75f0d260, WAITING_FOR_REVIEW) — one per game (Lanterns, Mini Sudoku, Skyscrapers, Fireflies, Fuse); 20 screenshots total, each verified assetDeliveryState=COMPLETE and md5-matched.
- **PR #149 merged** (41bf88b) — 8 new `ScreenshotState` cases, `ScreenshotSolver`, `ScreenshotSeamTests` (9 tests), `scripts/capture-screenshots.sh` driving all 17 states at 1320×2868.
- **Build 25 shipped** — Fuse difficulty retune (PR #147) from measured hardware data; uploaded to TestFlight, swapped in-flight App Store submission from build 24 to build 25 (submission 0e8910c4, WAITING_FOR_REVIEW).
- Open: issues #150, #151, #152, #153; App Store submission awaiting review.

---

### Lanterns-Android — Play Console Build 4, Portrait Lock, Bug Fixes

- **Cleanup build 4** (versionCode 4, Aug 10) — covers #27/#33/#51/#52; cached solve-history parse + closed Archive thumbnail race (#61); portrait lock (userPortrait, gated on targetSdk ≥ 36 + sw ≥ 600dp).
- **Play Console** — build 4 rebuilt from clean main, verified via bundle manifest + keytool + 271 green unit tests; staged on alpha track with release notes (not submitted — managed publishing is OFF, submit stays David's click).
- Production gate: 12 testers / 3 days as of Aug 11; unlock ~2026-08-22 (tester-day clock, not build pushes).
- Open: #15 (App-Signing correction posted, awaiting response), #57 (scrollable board, narrowed in scope), #59, #60.

---

### sacred_colors — App Name Locked, ADR-0003, PRs Merged

- **ADR-0003 merged** via PR #17 (7517f4c, Aug 13) — product direction locked: free devotional coloring app, TestFlight-only (not App Store listed); cancels activity-platform pivot and deferred Phase 4 sharing backend.
- **PR #20** — emptied published manifest, decoupled CI from live Azure.
- **PR #22** — fixed stale "still live" placeholder header.
- **PR #23** — app name confirmed final as "Sacred Colors" (ADR-0003 addendum, closes #14).
- **PR #24** — gallery tabs derived from pages present (closes #13).
- TestFlight status confirmed shipped, expires 2026-09-01.
- Stale worktree `exciting-merkle-d13876` confirmed as a stale copy (not divergent work), closed.
- Open: first Temple page next — blocked on licensed source art (tool/lineart/ already in place).

---

### smash-stack — Greybox Prototype + Playtest Procedure

- Greybox prototype shipped through PR #8 (3 archetypes, par scoring, validation harness, 77ce26e).
- Playtest PASS on arch recorded (PR #10), STAND validation case (PR #11), SUSPENDED archetype (crane) added with cantilever retired as test case and its KNOWN exemption mechanism fully removed (PR #12, PR #13).
- `docs/playtest-script.md` added — session procedure, briefing script, scoring traps (711936d).
- Cantilever level pulled from playlist — two valid solutions contaminate the core measurement (issue #9).
- Next milestone: run an actual playtest session.

---

### AI-Project-Backlog / Portfolio

- **PR #10 merged** (eade1cf) — PORTFOLIO.md 08-12 refresh: added Topped, corrected ADR-0025/Lanterns 1.9/Smash Stack/m365-mcp launchd rows.
- **PR #11 merged** — PORTFOLIO.md now defers the scheduler inventory to ops-runbook instead of duplicating it (commits 7698bbe, 6e824d2, 57068c4).
- Stage-of-life portfolio artifact published and revised 4× (42 repos, 32 projects, five scheduler surfaces, grouped by stage).
- Weekly portfolio survey scheduled task built end-to-end — prompt, artifact template, history.jsonl baseline; moved to Sundays 22:00; iMessage-on-risk-only added; test-fired correctly.

---

### Apple Music — Library Cleanup

- Merged 9 stray playlists (Boat, Car Ride, Dhdh, New, Pool, Plane, Shower, Top, Untitled) into "On the Go" (85 tracks, including Roxette's 5 songs).
- Merged Dashing into Get Moving (44 tracks — same hard-rock/metal artist pool).
- Deleted 5 empty/home playlists (New Year, Mamma Mia, Homecoming, 45, 25).
- 17 near-duplicate tracks identified but left untouched per David; "Duplicates To Review" playlist deleted at his request.
- Session complete with no open action items.

---

## Research & Quick References

- **Systems thinking (Donella Meadows)** — Four parts of any system: elements (weakest lever), relationships/default decisions, feedback loops, purpose (strongest lever). Applied to open-brain: the needs-context zone is its feedback loop; a destination with no retrieval trigger is a write-only store.
- **TikTok ASR transcript extraction** — TikTok ships WebVTT captions at a URL in `__UNIVERSAL_DATA_FOR_REHYDRATION__` JSON under `subtitleInfos`; fetching it recovers the full spoken transcript. Complement to the slide-stepping technique for photo/carousel posts.
- **AI tool listicle staleness filter** — Check whether named models/tools are current; a post recommending "Claude 3.5 Sonnet" in August 2026 is regurgitating two-generation-old training data.
- **AutoResearch loop conditions** — The score must BE the goal (not a proxy); search space large and unexplored; trial cheap and self-verifying. Only build time and flaky-test elimination survive this test for an app codebase.
- **Four UI patterns for LLM-generated apps** (from @haydenschmitty) — skeleton loaders, semantic colors, cut non-useful clutter (especially two-sentence subheaders under self-explanatory headers), accessibility standards.
- **Watchlist provenance** — Emily recommended *To the Bone* (2017) and *Veep* (HBO, 2012–2019) to David; added to Trakt watchlist Aug 13. Captured here because Trakt stores the title but not who suggested it.
- **ADR + issue practice packaged** — De-identified handout: claude.ai artifact published, 4-page PDF + Markdown source in ~/Downloads as `Decisions-and-Open-Work.{pdf,md}`. Not committed to any repo.

---

## Open Threads Carried Forward

- **job-monitor #4** — heartbeat write path (adding hb-monitor to store_briefing enum would make watchdog's own heartbeat forgeable; left as open ADR decision)
- **job-monitor #5** — cloud monitor sandbox can't reach Tailscale Funnel or RemoteTrigger; monitor routine stays disabled pending an environment with repo access
- **job-monitor #3** — silent-failure-path hardening continues
- **ops-runbook #2, #3, #7, #8** — open issues
- **slather #11, #13, #17** — open issues (including privacy-policy content refresh)
- **open-brain #9, #10** — open issues filed this week
- **Lanterns iOS #150–#153** — open PRs; App Store submission (build 25) WAITING_FOR_REVIEW
- **Lanterns-Android** — production track unlock gated on tester-day clock (~2026-08-22); PR #15 App-Signing correction awaiting response
- **sacred_colors** — first Temple page blocked on licensed source art
- **smash-stack** — next milestone is running an actual playtest session
- **Topper #19** — watch battery drain: symptom stopped presenting, nothing was measured; unresolved

---

## Stats

| Category | Count |
|----------|-------|
| Projects with shipped work | 12 |
| Research/reference sessions | 7 |
| Open threads carried forward | 11 |

---

*Generated 2026-08-17 · Covers Mon Aug 10 – Sun Aug 16, 2026*

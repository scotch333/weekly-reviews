# Weekly Review — August 31–September 6, 2026

*Sources: Open Brain · Claude Code git history*

---

## Shipped This Week

### tunepop

New iOS app — scoped and named with Oakley and David. Full week of milestone work:

- **Project started** (9/4): Repo created and scoped (`f0f9207`, `2e9c9b1`), `/setup-ios` scaffold with CLAUDE.md and ADR template
- **M0** spike built on branch with camera-hardware guard, then reviewed and merged; ADR-0002 (segment-per-pause sync) accepted after a real iPhone 17 Pro Max device test — 32 unit tests green, lint clean
- **M1** core loop built: 7 screens (Home, Choose Music, Record Video, Mark, Name & Save, Video, My Videos), SwiftData persistence (ADR-0003), NavigationStack/Route enum (ADR-0004), design spec at `docs/design/m1-screens.md` — 62 tests, lint clean
- **M2 Beat Maker milestone** (9/6): 8-step grid beat sequencer, tempo presets, offline renderer, view model (`7d138cf`, `26c9489`); ADR-0005 beat maker engine/render, ADR-0006 eight-step beat grid, ADR-0007 confidence-gated rotation (`18335b9`)
- **Orientation/rotation bug (issue #1)** fixed across export, preview, and per-take capture: unmirrored front camera, per-take `RotationCoordinator`, `StepClock` contract, phase-locked playhead — 16 commits total

Open: M2 review through round seven (rotation coordinator now main-actor) — finish remaining feedback, then device walkthrough before merge.

---

### Topped (topper)

**First App Store release shipped.**

- 1.0.1 build 13 submitted for App Review (9/3, `a672c5b`) after recovering a stuck "Apple status check" session by reading the transcript file — the work on disk was real and uncommitted
- WeatherAttributionView added (ADR-0021): Apple Weather mark (U+F8FF escape + legal link) beside Home's Today card; running/expired goal pill and Settings goal row — ios-code-reviewer approved after one round
- build 13 archived and gated on the mini (43 checks, BuildMachineOSBuild 25F80), uploaded VALID, submitted 2026-09-03 10:31 PT
- **Approved 2026-09-05; released via appStoreVersionReleaseRequest 2026-09-05 14:29 PT → READY_FOR_SALE**

Open: scotch333/topper#50, #51.

---

### cube-scrambler

Hardware design iteration week:

- **Fail-safe redesign** (9/4, `306635f`): retraction springs were compression springs that would drive the claw into the cube on servo failure; replaced with extension springs between printed anchor posts (39mm from face axis, 25–31mm travel in tension); carriage widened to 90mm; `params.scad` now owns shared carriage/panel geometry with assert-based collision checks against carriage edge, claw aperture, and rod bosses; `cad/render.sh` pinned with OpenSCAD version and CUTAWAY flag
- **Three unbuildable faults fixed** (9/5, `b6daf1e`), sourcing notes added with spring line deliberately unpriced (`899c054`), claw print orientation corrected (`1ea0027`)
- **First hardware-verified claim** (9/5, `4f50de3`): FIT_GAP fits in PLA
- PETG claw fit recorded, rationale for all-PETG parts documented (`fabbd49`)

---

### Retirement Tracking App

CSV import parser hardened with new tests and a dogfood checklist (`ed436c5`).

---

### open-brain / OpenBrainGit

- 8/31 changeset committed as `52f633f`: header-only auth, `landingPatch()` + test, skill destination, Trakt slugs, backup drops — closed #6–#11
- **Supabase v18 deployed** (9/3) via MCP deploy tool; post-deploy hash verification across all six files confirmed identical
- Scoring rubric updated for the offline-decode step and normative `fuel_unreadable` text (`db5a2ec`)
- Named-item rule added and wired into the prompt (`dea2c36`)
- UTC/local date-reading bug fixed; last kind-only landing count backfilled (`7a64090`, `4a4431d`)

Open: scotch333/open-brain#13, #14, #15.

---

### ops-runbook

- **ADR 0003 adopted and executed**: homeserver Funnel MCP endpoints move to tailnet-only delivery (`:8443 Serve`), closing the unauthenticated-Funnel exposure — `imessage-mcp` and `watchlist` both repointed
- **Morning briefing WebFetch outage fixed** (9/1, `d27a5dd`): rewritten from WebFetch to Bash+curl for NWS weather + NPR/TechCrunch RSS; verified on two real unattended fires (8/31, 9/1 — 68s each, all sections populated); full incident audit across 22 days of run logs committed, scotch333/ops-runbook#14 closed
- **Stream Deck profile sync runbook** added (9/5, `b401899`); `workstation/` folder established as new domain for MacBook Air-side ops docs
- iOS release-build rule documented: archive on the mini, never on beta-macOS laptop
- No scheduled tasks run on the default permission mode — recorded

---

### slather

- **4.10 resubmit** (9/1): Apple Health and Watch ungated via PR #30 (`6420473`), `CURRENT_PROJECT_VERSION` bumped 16→17, build 17 submitted; three verified simulator/ASC gotchas from the resubmit documented
- **3.1.2 rejection** recorded: EULA link must appear in the App Description, not only in the app

Open: build 17 in App Review; 3.1.2 EULA-in-description fix still needs a resubmission.

---

### claude-config / claude-watch

- **watch@claude-watch plugin** installed and enabled (`8113bd6`); two local patches in plugin cache (ffmpeg 9 `-fps_mode`, yt-dlp `sub-langs en.*/eng.*`)
- **nightly-capture-triage skill** decode section rewritten: yt-dlp description first, claude-watch for videos, embed player as fallback (`b36fa92`); upstream PR taoufik123-collab/claude-watch#18 filed (TikTok eng-US caption track)
- **bin/asc.rb** (shared App Store Connect client) added to claude-config (`36409d8`); topper repointed (`0e689e8`)

Open: taoufik123-collab/claude-watch#18.

---

### Boat Explorer / aj-boat-app

Project paused. v1 meets every line of its definition of done; AJ's interests moved on.

- `PROJECT-STATUS.md` added: v1 completion, placeholder-vs-real asset inventory, four open threads (`0194122`); pause banner in `app/README.md`; `npm run build` verified still succeeds before pausing

Backlog: SH-31 — resume if AJ's interest returns.

---

## Research & Quick References

- **AI tooling hazard analysis**: two hazard classes flagged from a GitHub trending post — agent-control MCPs that route around the PreToolUse gate (e.g., DesktopCommanderMCP), and free multi-provider gateways that transit prompts/source/credentials through a third party (e.g., OmniRoute)
- **Tailcat vs McMini transport problem**: Tailcat removes the control plane, which is not the actual problem; the open issue is unauthenticated Funnel MCP endpoints on the public internet; recorded options remain tailnet-only `tailscale serve` or an authenticating proxy
- **GitHub repo audits** (×3 TikTok capture batches): context-mode (98% tool-output reduction + session memory, queued SH-20), marketingskills (CRO/copy/SEO skills, plausible fit for SH-5 + App Store listings), taste-skill (anti-slop design), cc-switch (skipped — fights claude-config symlink farm), Obsidian skills (skipped — no vault), last30days-skill (covered by WebSearch + existing skills), Graphify (speculative), trigger.dev (would be a sixth scheduler — skipped)
- **yt-dlp TikTok behavior**: `/photo/` URLs rejected; same ID under `/video/` returns audio + description; TikTok ships an ASR caption track labelled `eng-US` that yt-dlp pulls as VTT; photo-post audio is licensed music, so Whisper adds nothing on that class
- **Recipes captured**: Crockpot Tuscan chicken and orzo (pasta added only for final 30 min on high); Mongolian ground beef noodles (soy/broth/brown sugar/hoisin/garlic/ginger, cornstarch-thickened, broccoli steamed in the same skillet)

---

## Open Threads Carried Forward

- **tunepop**: M2 review (round 7) — finish remaining feedback, device walkthrough, then merge
- **topper**: scotch333/topper#50, #51
- **slather**: build 17 in App Review; 3.1.2 EULA-in-description fix + resubmit
- **open-brain**: scotch333/open-brain#13, #14, #15
- **claude-watch**: taoufik123-collab/claude-watch#18 (TikTok eng-US caption track upstream PR)
- **lego-arbitrage-scanner**: monitoring live homeserver scheduled scans and iMessage alerts for correctness (shipped prior week, now in observation)
- **Lanterns**: #195, #197, #200, #204, #205, #208, #150; PR #207 (post-1.11 submission open items; VoiceOver device pass still pending)

---

## Stats

| Category | Count |
|----------|-------|
| Projects with shipped work | 9 |
| Research/reference sessions | 7 |
| Open threads carried forward | 7 |

---

*Generated 2026-09-07 · Covers Mon 2026-08-31 – Sun 2026-09-06*

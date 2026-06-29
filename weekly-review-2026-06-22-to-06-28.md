# Weekly Review — June 22–28, 2026

*Sources: Open Brain · Claude Code git history*

---

## Shipped This Week

### Wonder Slimes Build
Fixed the editor save-draft 403 error by switching owner save from `upsert` to `UPDATE` (commit 408da96); ADR-0018 documents the decision. Release prep for build 4: bumped `CURRENT_PROJECT_VERSION` 3→4 (commit 208b427).

**Open:** Build 4 staged and ready to submit/release to App Store.

### Lanterns (SwiftUI iOS Puzzle Game)
Scaffolded the full project in Cowork — 15 files covering app entry, Puzzle/GameState models, carve-and-destroy puzzle generator + solver, board/cell/main views, streak persistence, XcodeGen `project.yml`, `CLAUDE.md`, and `GeneratorTests.swift`. Replaced a broken naive generator (0 of ~149k random boards were unique-solution) with the carve-regions-then-destroy-alternative-solutions algorithm. Daily generator verified: 40/40 seeds produce unique-solution, no-guessing puzzles in ~100ms each. Project handed off to `/Users/davidprice/Documents/Claude Co-Work/Lanterns`.

**Open:** No build run yet — first action is `xcodegen generate && open Lanterns.xcodeproj`. Onboarding/tutorial screen (sun = pre-lit, keep lanterns off it) not yet built.

### AnyList MCP
Node MCP server deployed to homeserver (192.168.0.78) and verified end-to-end against the real AnyList account: login, 75-recipe library, collections, and meal-planning calendar round-trip (plan_meal/unplan_meal). Deployed as stdio initially (mirroring m365-mcp launch.sh), then migrated to Streamable-HTTP for Cowork compatibility (pm2 on :3000, Tailscale Funnel). Boot-persistence via pm2 launchd finalized. Project files + handoff mirrored to iCloud at `Claude/anylist-mcp/`.

Key durable finding: AnyList 401-blocks cloud/datacenter IPs — Tailscale Funnel is the required architecture (proxies inbound only; homeserver's outbound AnyList login stays on the residential IP).

**Open:** Reboot-test auto-start via `list_recipes`. Goal 2 (TikTok/YouTube video → `create_recipe`) deferred.

### m365 MCP
Migrated from stdio (invisible to Cowork) to Streamable-HTTP: `server_http.py` wrapping `graph_mail.py`, running as `pm2 m365-mcp` on port 3001, exposed on the existing AnyList Tailscale Funnel path at `/m365`. All three tools (move_message, delete_message, block_sender) verified live in Cowork. Boot-persistence done; redundant `:8443` funnel removed. Full runbook at iCloud `Claude/m365-mcp/M365-HTTP-MIGRATION.md`; summary in `EMAIL-OPERATIONS.md`.

Connector URL: `https://homeservers-macbook-air.tailde57d8.ts.net/m365/mcp`. Key gotcha: server keeps `MCP_PATH=/mcp` (Tailscale strips the `/m365` prefix); use `tailscale funnel --set-path`, never `tailscale serve`.

**Open:** Apply current email triage batch (8 keep / 2 Weekly Action / 4 Weekly FYI / 35 delete) to confirm moves/deletes execute end-to-end.

### sacred_colors
Line-art authoring tooling merged on `tool/lineart` (commit 11e4813, PR #3). CI workflow (analyze + test) and git-workflow docs merged (commit 3a76ed4, PR #2). PROGRESS.md and ADRs 0000/0001/0002 updated. sacred_colors is now the reference implementation of the locked GitHub Flow / CI-as-gate solo-dev standard.

**Open:** Slime-Squeezy CI green + branch protection; Lanterns CI green + `master`→`main` rename + remote push; Lanterns stray branch `claude/brave-goodall-136956` to prune.

### smart-shopper (Skill)
Built and delivered as an installable Cowork skill: asks-first flow, full-market research, price-history/deal vetting, trust vetting, and ranked shortlist output.

**Open:** Live test-drive on a real purchase offered — awaiting David's go.

### Git Workflow Standard
Audited the solo git workflow across all projects (prompted by a TikTok Gitflow recommendation). Verdict: existing GitHub Flow model is correct; real gap is that rules are aspirational/unenforced. Delivered to iCloud: `GIT-WORKFLOW.md`, `GIT-WORKFLOW-claude-md-snippet.md`, `GIT-SETUP-STEPS.md`, and minimal CI workflows for Lanterns + Slime-Squeezy. Nothing committed/pushed yet.

Audit findings: no CI in any repo; Lanterns on `master` (others on `main`); Lanterns + tiktok-to-anylist have no GitHub remote; Slime-Squeezy has an active feature branch (`fix/testflight-feedback-2026-06-21`).

**Open:** Push CI workflows on `chore/ci` branch for Slime-Squeezy first, get "Build & unit test" green, add branch protection — then Lanterns (Section B of GIT-SETUP-STEPS.md).

### claude-config
Three Claude Code config improvements shipped: `/wrap-up` command + "wrap up" convention added to CLAUDE.md (commit 39b7428); auto-commit-memory Stop hook + `/promote-memory` command (commit 412950e); memory auto-commits landing (commits 9102454 / f1d0537 / 0bf5b1c).

### popcorn-measure
Built a print-ready double-ended 3D-printable popcorn measuring paddle in pure Python. Deliverables: `popcorn_measure.stl` (watertight, 0 non-manifold edges), parametric `popcorn_measure.scad` (engraved labels), `build_stl.py` (pure-Python STL regenerator, zero dependencies). Cups: big end = ½ cup kernels (118.29 mL verified), small end = 2 tbsp oil (29.57 mL) + 1.5 tsp seasoning (7.39 mL). Dimensions ~160×72×45 mm, prints flat cups-up, no supports, PETG recommended for food contact. Commit 7e7abe5.

Note: STL is currently label-free — Homebrew's only OpenSCAD is a deprecated Intel build requiring sudo Rosetta; the `.scad` has engraved labels but the Python build doesn't emit them yet.

**Open:** Add engraved labels to the STL, tune cup proportions (R_BIG / R_OIL / R_SEAS), slice + test-print, verify volumes with water.

### apple-notes-inbox-triage (Scheduled Task)
New daily-at-4PM scheduled task created in Cowork. Promotes ready `📥Inbox` Notes items to Things 3, appends `↳` brainstorm prompts to fuzzy notes, confirms by iMessage. Delivery model locked: fuzzy ideas get inline running-brainstorm prompts; ready ideas auto-promote to Things 3.

**Open:** Hit "Run now" once to pre-approve Mac/Notes/Things/iMessage permissions so the 4PM run doesn't stall. Guardrail consideration: Apple Notes converts inline images to attachments on programmatic rewrite.

### Homeserver Brief
HOMESERVER-CODE-BRIEF.md written and saved to iCloud Claude folder — full picture of the mini-migration: MacBook Air M4 at 192.168.0.78, AnyList :3000, m365 :3001/m365, launchd, financial dashboard, CrewAI, OpenClaw.

Migration architecture decision locked: Cowork connector URLs are tied to the Tailscale node name (new mini = new node = re-add both connectors unless node renamed); mini must stay on home residential network because AnyList 401-blocks datacenter IPs — no cloud VM option.

**Open:** §7 has 4 open questions — confirm which older services (financial dashboard, CrewAI, OpenClaw) are still live and worth migrating before the next Code session.

---

## Research & Quick References

- **NAS pick** — UGREEN NASync DXP2800 + 2× Seagate IronWolf 8TB (~$930 box-direct, before UGREEN sale end) as lead pick; Synology DS225+ (~$339 + drives) as safe/software pick. Flagged: ~50% HDD price spike in 2026 (AI-driven shortage projected through 2028). Synology third-party drive lock-in reversed in DSM 7.3 (Oct 2025) — no longer a dealbreaker.
- **AR glasses** — Viture Luma (~$399) over Beast (~$549); built-in focus dials are a hard requirement (Beast lacks them).
- **1st Ave (218 E 1st Ave, Kennewick) toilet repair receipt** — Mr. Rooter invoice, Nov 30 2024, tech Mario Bermudez. Campbell & Co May 2026 is an estimate, not the repair record.
- **MasterClass weekly pick** — Chris Voss (Art of Negotiation) as lead; menu also Mark Cuban (New Rules of Wealth), Will Wright. "Sessions" format fits the one-per-week goal. Recurring Monday auto-pick task offered; awaiting David's go + classic-vs-Sessions format choice.

---

## Open Threads Carried Forward

- **wonder-slimes-build** — submit/release build 4
- **Lanterns** — first Xcode build run (`xcodegen generate && open Lanterns.xcodeproj`); onboarding/tutorial screen
- **AnyList MCP** — reboot-test auto-start; Goal 2 (video → `create_recipe`) deferred
- **m365 MCP** — Apply current email triage batch to confirm moves/deletes end-to-end
- **sacred_colors / git hardening** — Slime-Squeezy CI green + branch protection; Lanterns CI + `master`→`main` + remote push; stray branch prune
- **smart-shopper** — live test-drive on a real purchase (awaiting David's go)
- **Git workflow enforcement** — push CI workflows on `chore/ci`, get branch protection live on Slime-Squeezy, then Lanterns
- **popcorn-measure** — engraved labels on STL, cup proportion tuning, slice + test-print, water volume verification
- **apple-notes-inbox-triage** — "Run now" to pre-approve permissions; image-in-note guardrail
- **homeserver brief** — confirm which older services are live before migration Code session (§7)
- **MasterClass** — recurring Monday auto-pick task (awaiting go + classic-vs-Sessions choice)

---

## Stats

| Category | Count |
|----------|-------|
| Projects with shipped work | 11 |
| Research/reference sessions | 4 |
| Open threads carried forward | 11 |

---

*Generated 2026-06-29 · Covers Mon Jun 22 – Sun Jun 28, 2026*

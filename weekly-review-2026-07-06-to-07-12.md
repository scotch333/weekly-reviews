# Weekly Review — July 6–12, 2026

*Sources: Open Brain · Claude Code git history*

---

## Shipped This Week

### Lanterns Android

Major milestone: complete native Kotlin + Jetpack Compose Android port of the Lanterns daily puzzle game, scaffolded from scratch and pushed to github.com/scotch333/Lanterns-Android (private).

**Jul 6–7 (build-out):**
- Core game logic (SeededRng, PuzzleGenerator, Solver, HintEngine) hand-ported to Kotlin and verified byte-for-byte against the Swift generator via a 60-seed fixture — full parity confirmed
- Full gameplay loop verified live in Android emulator: board rendering with bloom glow, tap-cycle, conflict detection, hints, staggered illuminate-on-solve animation, native share sheet, Archive screen with thumbnails and past-day replay
- Release signing configured: keystore at `~/.android-keystores/` (never committed); `./gradlew bundleRelease` produces a verified-signed `.aab`
- Play Store graphics: 512×512 icon (downsize of real iOS icon), feature graphic and adaptive launcher icon reusing real iOS wordmark/lantern glyph, four live-app screenshots
- Fixed in-game lantern glyph to draw the real shape (handle/cap/body/base/lit core) instead of placeholder diamond
- Canonical Android/iOS privacy policy at scotch333/lanterns-legal (GitHub Pages) — one policy for the whole Lanterns family
- Store listing content (descriptions, Data Safety answers) finalized at `docs/store-listing/`
- ADRs: 0001 (Kotlin over KMP), 0002, 0010 (Archive always-unlocked on Android)
- Google Play Console developer account created Jul 7; identity verification submitted

**Jul 9 (follow-on commits — 5 commits total):**
- `df3448e` — targeted API 35 for Play Console upload requirement
- `d59e786`, `63b4565` — bumped versionCode 2→3 (v1/v2 burned by a blocked upload attempt)
- `e6fbab4` — corrected store listing app name to match iOS title
- `86f0f19` — dropped unowned `lanterns.game` domain from daily share text

**Open items:** Play Console identity verification pending; need 20+ continuously opted-in testers for the mandatory 14-day closed-testing window before production publishing unlocks.

---

### Slather-Up — Pre-Launch Social Campaign

Daily social deliverables executed per campaign calendar across the week (launch date: Jul 14):

- **Jul 6** — IG static countdown post (asset Slather-social-01.png) identified; blocked briefly on David confirming asset accessibility in iCloud folder
- **Jul 7** — TikTok before/after asset generated and delivered: 1080×1080 sunburn-silhouette vs. Slather-Up mockup, TikTok cover doubling as CTA banner (also repostable to IG)
- **Jul 8** — IG carousel post copy delivered: "3 myths about sunscreen timing" (asset Slather-social-02.png), ready to paste
- **Jul 9** — X post copy delivered: tester quote + 5-day-to-launch countdown, ready to paste, no asset needed
- **Jul 12** — No post scheduled; next is Jul 13 (X, launch eve)

Also drafted this week: Slather-Up TestFlight promo post (WeatherKit build-decision angle), blocked on David's call on the "last week" timing reference relative to the Lanterns launch post.

---

### Charlotte STR Tracker

- **525 E 6th St #409** — STR term firmed from "likely" to confirmed 180 days, based on Tibor's 6/15 message; next action updated to "Pass (above 30-day floor)"
- **718 W Trade St #801** (Gateway Plaza penthouse) — new row added; 7-day STR minimum, flagged by Tibor 6/15, pending listing link
- Header date bumped to 2026-07-05; file verified clean
- File permanently relocated from session scratchpad to `~/Library/Mobile Documents/com~apple~CloudDocs/Claude/Projects/Charlotte-STR/Charlotte_STR_Tracker.xlsx`

**Open items:** 300 W 5th #204 and MLS# 4372926 duplicate-listing question still open with Tibor.

---

### Email Rules Audit (m365 / Exchange Online)

Exchange Online PowerShell script (`.ps1`) drafted to reorganize two Outlook rules:
- Split rule 24 ("Newsletters → folder") into two: SUBSTACK/MASTERCLASS/PETERSONACADEMY/CFOSILVIA → Weekly FYI; APARTMENTLIST → Weekly Realestate
- Repoint rule 26 ("Art with Flo") to Weekly FYI

Delivered for David to run via `Connect-ExchangeOnline`. No create/edit tool available on the m365 connector; not yet executed.

---

### data-science-intern Skill

EDA + basic modeling skill built and packaged: `profile_data.py` script + `statistical_methods.md` reference guide. Presented for install during a Cowork session on Jul 6.

---

### Weekly Cowork Recap (prior week)

`cowork-recap-2026-06-29.md` generated covering Jun 29–Jul 5 (12 questions, 10 decisions/milestones, 10 completed, 9 in-progress items). claude-config push skipped — repo not reachable from sandbox.

---

## Research & Quick References

- **Presearch node staking (Jul 12)** — Confirmed legacy PRE staking (4,000 PRE minimum, grandfathered for pre-raise stakers) and the new Node NFT License system (Series I/II/IV, "Presearch 3.0," launched Dec 2025) are separate parallel systems. No official Presearch statement found linking the Series IV auction (closing the 26th) to existing legacy stakes or earlier Node NFTs. Recommended checking nodes.presearch.com/dashboard for account-specific state.
- **Apple Notes inbox triage (ongoing)** — 2 items confirmed READY (bug spray + power inverter for camping; Starlink vehicle mount); 3 items still FUZZY awaiting David's clarifying answers (Deena $/conference-talk giving; home-automation dashboard scope; "Mosiah also fled?" scripture/writing note); duplicate "📥Inbox Notes" note still needs manual merge/delete.
- **MacBook Air crash investigation (from Jul 3, no update this week)** — All 6 reboots since Jul 1 traced to one recurring kernel panic (watchdog timeout, no checkins from watchdogd ~90s) on macOS 26.6 beta build 25G5043d; photolibraryd crashes are aftermath, not cause. Options identified: (1) turn off Beta Updates and wait for 26.6 final, or (2) erase-and-downgrade to 26.5.2 via USB installer.

---

## Open Threads Carried Forward

- **Email rules .ps1** — written and delivered; awaiting David to run it via `Connect-ExchangeOnline` and confirm folder placement
- **Apple Notes inbox triage** — 2 READY items not yet pushed to Things; 3 FUZZY items awaiting David's answers
- **Lanterns Android Play Console** — identity verification pending; 14-day closed-testing window with 20+ testers not yet started
- **Deena's 8 duplicate Outlook rules** — `delete_message_rule` proposal awaiting David's go-ahead
- **Lanterns Daily LinkedIn launch post** — drafted and ready; blocked on David's call re: "Queens" comparison and App Store link placement
- **MacBook Air crash remedy** — Beta Updates off vs. clean downgrade to 26.5.2 still undecided
- **Charlotte condos — 300 W 5th #204** — STR term and MLS# 4372926 duplicate-listing question open with Tibor
- **m365 connector** — re-add at Tailscale URL + run quarantine task before next Saturday 7am scheduled run
- **Slather-Up TestFlight promo post** — drafted, blocked on timing call relative to Lanterns launch post

---

## Stats

| Category | Count |
|----------|-------|
| Projects with shipped work | 6 |
| Research/reference sessions | 3 |
| Open threads carried forward | 9 |

---

*Generated 2026-07-13 · Covers Mon Jul 6 – Sun Jul 12, 2026*

# Weekly Review — May 11–17, 2026

*Sources: Claude.ai chat · Cowork · Claude Code*

---

## Shipped This Week

### Podcast
Named the show: **Three Worlds, One Mic** — generated, narrowed, and stress-tested against existing shows, domain availability, and social handles. Clean across all dimensions. Runner-up: Three Backgrounds, One Question.

Built and deployed the **Podcast Survey App** — two-part LinkedIn topic survey. Part 1: 2-minute form for topic interests, free-text ideas, and content format preferences. Part 2: opt-in LinkedIn OAuth + data export upload for richer engagement signal. Stack: FastAPI, Azure Table Storage + Blob Storage, Docker → Azure Container Apps. GitHub: `scotch333/podcast-survey`.

**Open:** Lock `threeworldsonemic.com` + `.fm` · Grab `@threeworldsonemic` across IG / X / TikTok / YouTube / Spotify · Optional USPTO TESS search.

---

### Open Brain
Now live across all surfaces (Cowork, Claude Desktop, Claude Code). Capture/search confirmed end-to-end (89.6% match on test). Nightly 8:30 PM automation running: inbox triage first, then auto-capture of session content to the brain.

**Open:** System is live but sitting mostly empty — no real thoughts captured beyond the initial test.

---

### Medium Article & Writing Voice
Built the **Medium-Voice Skill** from scratch — voice profile (keep/lift/avoid), three default article structures, dual entry points (notes→draft and edit-my-draft), calibrated against Slack rant samples. Packaged as `medium-voice.skill`.

Drafted and revised the **Apple Notes + Claude + Things 3 article** twice. Revised framing: Inbox Note as an incubator, not a dump — Claude reads it, moves formed ideas into Things, leaves the rest to simmer. Saved to `Documents/Claude/Projects/Medium Articles/apple-notes-claude-things3-routine.md`.

---

### 3D Printing Optimizer
Documented the full Bambu fleet (X1 Carbon, A1, A1 Mini, Dan's A1) with bed sizes, queue structure, and routing rules. Built `print-queue.html` — self-contained, no install required, gift-able by AirDrop. MakerWorld bookmarklet flow: one click to capture model → one click to paste into queue. Saved to `Documents/Claude/Projects/3D Printing Optimizer/print-queue.html`.

---

### TikTok-to-AnyList
Major push: added async background processing, URL deduplication, job status polling page, and fixed tunnel detection bug. `CLAUDE.md` written for CLI handoff.

**Open:** Blocked on Node + `@anthropic-ai/claude-code` install — can't run `claude` from Terminal yet.

---

### Guest Message Autopilot
Done: Pushcut installed, iOS Shortcut built, Zap wired (Catch Hook → Claude → POST to Pushcut), Hostaway webhook pointed at Zapier.

**Open:** Blocked on confirming the Hostaway webhook payload — test fire returned a near-empty "querystring" ping. Resume: send a real guest message through Hostaway, then hit *Find new records* on the Catch Hook in Zapier. Fallback: add a Hostaway API lookup step to fetch the message by ID.

---

### Costco Rebate Agent
Built a local AI tool that scans Costco receipts (PDF/photos), scrapes weekly deals and the monthly coupon book, matches by item number (fuzzy fallback + Claude tiebreaker), and outputs HTML + Markdown reports of price adjustments you can claim. Runs entirely locally via Anthropic API. GitHub: `scotch333/costco-rebate`.

---

### AI Project Backlog
Reorganized and written to a live file at `Documents/Claude/Projects/AI Project Backlog/backlog.md`. #01 (Monthly Financial Review) marked Done. #02 (Guest Message Autopilot) marked Active. #03 (STR Expense Auto-Classifier) removed.

---

### GitHub Copilot
Mapped the M365 license path — seat assigned to GitHub handle, not Microsoft account. Self-serve signup paused April 20–22; recommended Copilot Free to validate usage first.

Hands-on CLI walkthrough: `gh copilot suggest` and `gh copilot explain`, `??` / `?!` aliases wired into `~/.zshrc`.

---

### Productivity Tracking
Set up CLAUDE.md profile, TASKS.md mirror of Things 3 (last synced 5/14), and `dashboard.html` in Claude Code.

---

### Evening Inbox Triage
7 nights completed (May 11–17). Two items consistently ready: *Write the Medium article* and *Research the Mercy Seed*. Two consistently parked: *Glass box sessions → customer personas* (sentence trails off) and *Deena $ — conference talk* (still tangled). New items added mid-week: *Send the podcast to the nerds/Sandi's siblings* and *Check out Hermes* (OpenClaw replacement).

**Open inbox items:**
- Glass box sessions → customer personas → test new concepts with… (needs one pass to finish the thought)
- Deena $ — conference talk / how can I give more? (still tangled — money, time, intro, speaking slot)
- 13 F.info — still cryptic, no context
- Dyson Swarm research — not yet confirmed in Things 3

---

### Research & One-Offs
- **Work/travel bag** — WaterField Executive Leather Messenger (Full) to replace the Solegard Optimist for the two-laptop + iPad load. Pad & Quill Valet as value alternative.
- **Senske work order** — PDF parsed (Cherry Tree Cover Program, Permethrin, May 12, Kennewick). Filing location still unresolved — not yet actioned.
- **Slime Squeezy Content Engine** — project descriptor drafted, Cowork project not yet created. Backlog #03 still needs to be removed from Cowork project settings.

---

## Stats

| Category | Count |
|----------|-------|
| Projects with shipped work | 9 |
| Open threads carried forward | 8 |
| Inbox triage nights completed | 7 |

---

*Generated 2026-05-18 · Covers Mon 5/11 – Sun 5/17*

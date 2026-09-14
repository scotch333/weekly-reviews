# Weekly Review Agent — scotch333/weekly-reviews

Runs every Monday at 6 AM Pacific. Generates a consolidated weekly review and commits it to this repo.

## Source of Truth

**Open Brain is the primary source.** Do not look for or require cowork recap files or Claude.ai chat recap files. Those are no longer part of the process.

Pull all Open Brain thoughts captured during the target week (Monday–Sunday) using the Open Brain MCP tools (`mcp__open_brain__list_thoughts`, `mcp__open_brain__search`). Thoughts may be dated slightly after the week ends (e.g. a Monday capture of a Sunday event) — include any thought whose underlying event falls within the target window.

### What to expect in Open Brain

Coding sessions are expected to capture three classes of events explicitly:
- **Project started** — new repo or project kicked off
- **Major milestone** — significant feature complete, architecture decision locked, blocker resolved
- **Shipped** — working code pushed, deployed, or handed off

These are the anchor events the weekly review is built from. If a project appears in git history but has no Open Brain milestone, note it under Shipped This Week with "no milestone captured" so the gap is visible.

## Step 1: Determine target week

Today is Monday. The review covers the Monday–Sunday that just ended (7 days ago through yesterday).

## Step 2: Pull Open Brain thoughts

Use `mcp__open_brain__list_thoughts` with `days: 10` and `limit: 50` to catch the full window. Supplement with targeted searches if needed. Group what you find by project/topic.

### Fallback when Open Brain is unavailable

If the `mcp__open_brain__*` tools are not present in the session, run `ListConnectors` with keyword `open brain` to confirm the state (typically `needs_reconnect` or `enabledInChat: false`). Then:

1. **Do not produce an empty review.** Use Outlook (`mcp__ms365__*`) as the supplemental source: search the calendar, Sent Items, and Inbox for the target week, plus targeted queries for active project names (Lanterns, Slather-Up, Turo, etc.).
2. Add a `> **⚠ Data gap:**` banner directly under the *Sources* line naming which source is missing, why, and how many consecutive weeks it has been missing.
3. Tag each project section with `*(sourced from email/calendar; no Open Brain milestone captured)*`.
4. Add "Open Brain connector — needs reconnect" to Open Threads Carried Forward so it stays visible until fixed.
5. Carry forward the prior review's open threads verbatim with "status unknown" rather than dropping them.

## Step 3: Pull Claude Code git history

Do not use a hardcoded repo list. Discover repos dynamically each run:

1. `mcp__github__search_repositories` with query `user:scotch333` (paginate until exhausted).
2. Include every repo where `archived` is `false`.
3. Also include repos where `archived` is `true` **and** `updated_at` falls inside the target week — archiving bumps `updated_at`, so this catches projects wrapped up this week. Note them as "archived this week" in the review.
4. Skip forks unless they have commits by scotch333 in the window.

For each included repo, `mcp__github__list_commits` with `since` = target Monday and `until` = review day. Skip repos with zero commits in the window.

If the session's GitHub access is scoped to `weekly-reviews` only (calls to other repos return "Access denied"), that is an environment configuration problem — note it in the data-gap banner. Then search Outlook for `[scotch333/` GitHub notification emails from the target week; CI runs, PR activity, and merges all land there and are enough to reconstruct what shipped.

## Step 4: Generate the consolidated review

Combine Open Brain + git history into a single markdown file using this structure:

```
# Weekly Review — [Month DD]–[DD], [YYYY]

*Sources: Open Brain · Claude Code git history*

---

## Shipped This Week

[Group by PROJECT as ### headings. Merge any project that appears in both Open Brain and git commits into one section. Include what was completed and any open items inline under that project.]

---

## Research & Quick References

[Bullet list of research sessions, quick questions answered, decisions made — things that aren't ongoing projects but are worth logging.]

---

## Open Threads Carried Forward

[Bullet list of in-progress items not yet closed.]

---

## Stats

| Category | Count |
|----------|-------|
| Projects with shipped work | N |
| Research/reference sessions | N |
| Open threads carried forward | N |

---

*Generated [YYYY-MM-DD] · Covers Mon [date] – Sun [date]*
```

## Step 5: Save and commit

- Filename: `weekly-review-[YYYY-MM-DD]-to-[MM-DD].md` (Monday date through Sunday date)
- Commit message: `Add weekly review [Month DD–DD, YYYY]`
- Push to the main branch of scotch333/weekly-reviews

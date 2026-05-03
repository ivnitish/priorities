# Ideas Parking Lot

Captured ideas for the priorities app and broader assistant setup. Not committed to building — pick when ready.

---

## Mobile → Desktop Assistant Bridge (2026-05-03)

**Problem:** Two disconnected assistants today — `claude.ai` mobile (no file/repo access) and Claude Code on desktop (full access). Want one assistant or at least a clean bridge.

**Options ranked by effort:**

### Option A — Inbox tab on this priorities app (LOW effort, recommended starting point)
- Add a new "Inbox" tab to the priorities web app
- Big text box + Save button
- Saves to `inbox.md` in this repo via GitHub API + a fine-scoped PAT stored in browser localStorage
- Mobile flow: end claude.ai chat with "summarize as action items" → copy → paste into Inbox tab → Save
- Desktop flow: I run `gh api repos/ivnitish/priorities/contents/inbox.md`, file each entry into todos / watchlist / memory / research, clear processed entries
- Cost: $0
- Build effort: ~30 lines of JS/HTML, one-time PAT setup
- Pros: uses UI already loved, versioned via git, free, zero new infra
- Cons: manual copy/paste step on mobile (~10 sec per dump)

### Option B — Gmail inbox + cron (LOW effort, alternative)
- Use Gmail label `claude/inbox` as the bus
- iOS Shortcut: "Hey Siri, note for Claude" → emails self with `[claude]` subject
- Cron job on Mac runs Claude Code daily, reads label via Gmail MCP, files items, archives email
- Cost: $0
- Pros: voice capture, no UI work, hands-free
- Cons: feels old-school, round-trip not instant

### Option C — Full Claude Agent SDK assistant (HIGH effort)
- Custom chat web app at e.g. `assistant.ivnitish.com`
- Frontend: simple chat UI (HTML/JS, hosted on GitHub Pages — free)
- Backend: Node/Python running Claude Agent SDK on Cloudflare Workers (free tier) or Vercel (free tier)
- Custom tools: `add_to_watchlist`, `update_memory`, `fetch_stock_price`, `send_email`, etc.
- Persistent memory via Anthropic's memory API
- Cost: $0 hosting (free tiers) + $5-30/mo Claude API tokens (Sonnet) or $2-10/mo (Haiku)
- Build effort: weekend project (~250 lines + tool wrappers)
- Pros: real unified assistant, mobile gets full powers, persistent memory across devices
- Cons: real engineering, ongoing maintenance, may duplicate things claude.ai already does well (voice, artifacts, image gen)

**Note on costs:** Claude API pricing roughly Sonnet $3 in / $15 out per M tokens, Opus 5x more, Haiku ~1/3. For light personal use, API < claude.ai Pro $20/mo. For heavy use, Pro is better value.

**Recommended path:** Start with Option A (inbox tab). Use it for a few weeks. Only graduate to Option C if mobile usage becomes heavy and the manual paste step gets annoying.

---

## /todo Tab on Priorities App (2026-05-03)

**Idea:** Add a Todo tab alongside the priorities list.
- Same GitHub-backed pattern: stores in `todo.json` in this repo
- Browser writes via GitHub API + PAT
- Mobile + desktop both edit same list
- Replaces the local memory file `project_priorities_app_todo.md` which can't be edited from mobile

**Effort:** small, can piggyback on the Inbox tab plumbing if Option A above is built.

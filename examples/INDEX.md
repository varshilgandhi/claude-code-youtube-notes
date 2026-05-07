# YouTube Notes — Master Index

Notes from videos Varshil watches on Claude Code, AI engineering, LLMs, and dev tooling.
Focus: **actionable techniques** that can be applied to daily development.

How this works:
- Paste any YouTube link in a Claude Code conversation → the `youtube-notes` skill auto-fires
- Each video gets its own file in `videos/<id>-<slug>.md`
- This index lists every video + the top takeaways

---

## Top Cross-Video Themes

_(Will populate as more videos are processed)_

---

## Videos Processed

| Date | Channel | Title | File | Top Takeaway |
|------|---------|-------|------|--------------|
| 2025-12-17 | AI with Avthar | How I Start EVERY Claude Code Project | [aQvpqlSiUIQ-psb-system.md](videos/aQvpqlSiUIQ-psb-system.md) | Use the PSB system: Plan (spec doc) → Setup (7-step checklist) → Build (3 workflows). Plan mode is the most underused feature. |

---

## Quick-Apply Backlog

_Things to actually try in your projects, pulled from notes above. Tick them off as you adopt them._

- [ ] Create a project spec doc (product requirements + engineering design) before starting any new project
- [ ] Set up a `claude.md` that links off to other docs instead of inlining everything
- [ ] Set up automated docs: `architecture.md`, `changelog.md`, `project-status.md`, plus per-feature reference docs
- [ ] Pre-configure permissions in `~/.claude/settings.json` so Claude isn't waiting on approvals while you're away
- [ ] Try a stop hook that checks tests pass and tells Claude to keep fixing if not
- [ ] Use `#` (hash) in chat to push a learning into the relevant `claude.md` automatically
- [ ] Default to plan mode on any feature that touches more than one file
- [ ] Try issue-based development: turn project spec → GitHub issues, work them in parallel

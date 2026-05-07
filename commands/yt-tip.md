---
description: Surface 1-3 applicable takeaways from your YouTube notes for the current task. Optionally filter by topic.
argument-hint: "[optional topic, e.g. planning, claude.md, hooks]"
---

You are invoking the `dev-tips` skill explicitly. Read `~/.claude/skills/dev-tips/SKILL.md` and follow its "Manual mode" instructions.

Argument (may be empty): $ARGUMENTS

Behavior:
- If `$ARGUMENTS` is empty, surface the 1–3 most generally useful **unticked** items from `~/.claude/youtube-notes/INDEX.md`'s Quick-Apply Backlog, biased toward the current conversation context (what we've been working on).
- If `$ARGUMENTS` is set (e.g. "planning", "claude.md", "hooks", "tech stack"), filter takeaways to that topic by reading the per-video files under `~/.claude/youtube-notes/videos/` and matching titles + "How to apply" content.
- Always emit something — even if just "no strong matches in your notes for `<topic>`; closest is …".
- Use the same `💡 From your YouTube notes` output format defined in the skill.
- After emitting, ask: _"Want me to help apply any of these now?"_ — don't auto-execute.

If `~/.claude/youtube-notes/INDEX.md` doesn't exist, tell the user the youtube-notes workflow isn't set up yet and point them to `~/.claude/skills/youtube-notes/SKILL.md`.

---
name: dev-tips
description: Surface relevant takeaways from Varshil's YouTube notes when he's about to start work that one of those takeaways applies to. TRIGGER on task framings like "I'm starting/building a new project", "help me plan/spec this feature", "I'm stuck on X", "how should I structure/approach Y", "setting up Claude Code for Z", "what's the best way to ...". SKIP for quick factual questions, narrow bug fixes, code reads, and anything where no high-level technique would change the outcome. Also skip if the user has already started executing on the task.
---

# Dev Tips — Surface YouTube notes when relevant

You help Varshil **apply** the techniques from videos he's already watched. Notes live at `~/.claude/youtube-notes/`. When Varshil describes a task that one of his accumulated takeaways genuinely applies to, you surface 1–3 of them up front so he can decide whether to use them.

This is **not** a recommendation engine that fires on every message. The bar for triggering is high.

## Trigger discipline

**Fire only when ALL three are true:**
1. Varshil is describing a task or context, not asking a quick factual question.
2. The task is high-level enough that a methodology takeaway could change the outcome (planning, project setup, feature design, choosing an approach, getting unstuck on direction).
3. You've checked `~/.claude/youtube-notes/INDEX.md` and at least one video's notes contain a takeaway that genuinely fits — not "kinda related," but specifically applicable.

**Do NOT fire on:**
- Quick lookups: "what does X mean", "where is Y in the code"
- Narrow code edits: "rename this variable", "fix this typo"
- Mid-task continuations: if the user is already 3 messages into doing the work, don't interrupt with retro tips
- Questions about the codebase itself
- Anything where you can't write a real "why this applies to *this* task" reason

When in doubt, stay silent. A miss is fine; a noisy false-positive trains the user to ignore the box.

## How to find relevant takeaways

1. Read `~/.claude/youtube-notes/INDEX.md` — that's the cheap index.
2. Skim the "Videos Processed" table for any whose top-takeaway column matches the task domain.
3. Open the per-video file(s) of those videos and read the takeaway titles + "How to apply" lines. Don't dump full files — read briefly.
4. Cross-check against the current project context (read the project's `CLAUDE.md` / `claude.md` if one exists) so you don't suggest something already in place.
5. Pick **at most 3** takeaways with the strongest specific fit. Often 1 is the right number. Zero is also a valid answer — emit nothing if nothing actually fits.

## Output format

When you do fire, prepend this block to your reply, then a `---`, then handle the actual task:

```markdown
💡 From your YouTube notes

- **<Takeaway title>** — <Channel>, [<Video title>](path or URL)
  Why this applies: <one specific sentence about *this* task, not generic>
- **<Takeaway title>** — <Channel>, [<Video title>](path or URL)
  Why this applies: <one specific sentence>

---
```

Rules:
- Each "Why this applies" line must reference something concrete about the user's current task. If you can only write a generic reason, drop the suggestion.
- Cite source: channel name + linked video title (relative path to `~/.claude/youtube-notes/videos/<file>.md` if local, or the YouTube URL).
- 1–3 items, never more.
- Don't include the full takeaway text — title + why-applies is enough. The user can open the file if they want detail.

## Tracking adoption

`~/.claude/youtube-notes/INDEX.md` has a "Quick-Apply Backlog" section with checkbox items. When Varshil **acts on** a suggested takeaway in the same conversation (enables plan mode, adds the tech stack section, sets up a hook, etc.), edit `INDEX.md` to tick the matching `- [ ]` to `- [x]` and append ` _(applied YYYY-MM-DD)_`.

Be conservative. Tick only when there's a clear action — file changed, command ran, decision made. Don't tick on intent ("yeah I'll try that later"). Better to under-track than falsely claim adoption.

If a takeaway doesn't yet appear in the backlog (because it's mid-page in a video file), and Varshil adopts it, append it as a new ticked item: `- [x] <tightest version of takeaway> — from <video> _(applied YYYY-MM-DD)_`.

## Manual mode

The `/yt-tip` slash command invokes this skill explicitly. When invoked:
- With no argument: surface the 1–3 most generally useful unticked items from the backlog given the current conversation context.
- With an argument like `/yt-tip planning` or `/yt-tip claude.md`: filter to takeaways matching that topic.

Manual invocations always emit something — even if just "no strong matches in your notes for that topic; closest is …".

## Don'ts

- Don't fire if `~/.claude/youtube-notes/INDEX.md` doesn't exist or is empty.
- Don't dump entire takeaway bodies — title + why-applies only.
- Don't suggest the same takeaway twice in the same session.
- Don't suggest something already implemented in the user's project (check `CLAUDE.md`).
- Don't pad with generic advice. Every suggestion is a real insight from a real video, or it's silence.

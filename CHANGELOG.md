# Changelog

All notable changes to this project are documented here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and this project
uses date-based versioning until it stabilizes.

## [0.2.2] — 2026-05-07

### Fixed
- **Takeaway formatting:** the `**What:** / **How to apply:** / **Source:** / **Why:**` lines were emitted as adjacent lines without blank separators, which Markdown collapsed into a single running paragraph on GitHub. Each label now gets its own paragraph block (blank line before it). Skill template + example file both updated.

## [0.2.1] — 2026-05-07

Cleanup release after dogfooding on real videos.

### Changed
- **Filename convention:** per-video files now use `<YYYY-MM-DD>-<slug>.md` (upload-date prefix) instead of `<videoId>-<slug>.md`. Folder sorts chronologically; video id stays in frontmatter for traceability. Existing example renamed: `aQvpqlSiUIQ-psb-system.md` → `2025-12-17-psb-system.md`.
- **Duplicate detection:** since filename no longer carries the video id, the skill now greps frontmatter (`grep -l "video_id: <id>"`) before deciding whether to overwrite.

### Fixed
- YAML frontmatter quoting guidance: titles containing colons or inner quotes need to be wrapped in **single quotes**. The previous template would silently produce broken frontmatter that GitHub flagged with `"did not find expected key while parsing a block mapping"`.

## [0.2.0] — 2026-05-07

Closes the loop on watch → apply.

### Added
- `dev-tips` skill: when you describe a task one of your accumulated takeaways could change the outcome of, Claude prepends a `💡 From your YouTube notes` tip box (1–3 takeaways with a "why this applies" line) before handling the task. Conservative trigger discipline — silent for quick lookups, narrow edits, and mid-task continuations.
- `/yt-tip` slash command for explicit pulls. Optional topic argument: `/yt-tip planning`, `/yt-tip hooks`, etc.
- Adoption tracking: when a suggestion gets acted on in a session, the matching item in `INDEX.md`'s Quick-Apply Backlog flips from `- [ ]` to `- [x]` and is logged in a new "Recently Adopted" section.

### Changed
- Top of README and `examples/INDEX.md` rewritten to describe the **capture → apply** flow as the headline value, not just capture.
- Install steps now copy both skills + the slash command in one block.

## [0.1.0] — 2026-05-07

Initial public release.

### Added
- `youtube-notes` skill that auto-fires on YouTube URLs in Claude Code conversations.
- VTT caption extraction via `yt-dlp`, with manual-caption preference and auto-caption fallback.
- Python step that strips VTT formatting, dedupes overlapping cues, and produces a clean transcript with `[HH:MM:SS]` markers.
- Per-video output template: TL;DR, 5–15 actionable takeaways (What / How to apply / Source / Why), Skipped section, and a quick-apply checklist.
- Master `INDEX.md` updated on every run, with a cross-video themes section.
- Example notes from Avthar's _How I Start EVERY Claude Code Project_ (PSB system) as a real-world reference.
- MIT license, README with install + usage + customization, `.gitignore`.

### Notes
- Verbatim transcripts are saved locally for follow-up questions but are intentionally **not** committed to this public repo (out of respect for source creators).

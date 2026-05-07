# Changelog

All notable changes to this project are documented here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/) and this project
uses date-based versioning until it stabilizes.

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

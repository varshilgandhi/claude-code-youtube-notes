# claude-code-youtube-notes

Two Claude Code skills that close the loop on **watch → apply** for YouTube videos about Claude Code, AI engineering, and dev tooling.

1. **`youtube-notes`** — paste a YouTube URL in any conversation. Auto-fires, downloads the captions, reads the full transcript, extracts 5–15 sharp takeaways with timestamps and "how to apply" lines, and writes structured markdown to `~/.claude/youtube-notes/`.
2. **`dev-tips`** — when you describe a task one of those takeaways could help with (planning a project, getting unstuck, "how should I structure X"), Claude surfaces 1–3 relevant ones at the top of its reply, with a "why this applies" line. Plus a `/yt-tip` slash command for explicit pulls.

Built because I kept watching long videos, taking sloppy notes, and never actually applying anything. This system makes the apply step the default.

---

## What you get

```
~/.claude/skills/youtube-notes/SKILL.md       # capture: URL → notes
~/.claude/skills/dev-tips/SKILL.md            # apply: surface notes when relevant
~/.claude/commands/yt-tip.md                  # /yt-tip — manual pulls

~/.claude/youtube-notes/
├── INDEX.md                     # master index, quick-apply backlog, recently-adopted log
├── transcripts/<id>.txt         # cleaned full transcripts (kept locally for follow-ups)
└── videos/<id>-<slug>.md        # per-video actionable notes
```

Each per-video file has:
- **TL;DR** — 2–3 sentences of what the video is actually about
- **Top Takeaways** — each with **What / How to apply / Source timestamp / Why** (when non-obvious)
- **Skipped** — sponsor reads and pitches called out by timestamp so you know they weren't missed
- **Quick-apply checklist** — the tightest version of every takeaway

The `dev-tips` skill is conservative on triggers: it fires when you describe a task one of your takeaways could change the outcome of, and stays silent for quick lookups, narrow code edits, and mid-task continuations. When you adopt a suggestion, the matching item in `INDEX.md` flips from `- [ ]` to `- [x]` and gets logged in the "Recently Adopted" section.

See [`examples/`](./examples/) for a real run — notes from Avthar's 34-minute video [_How I Start EVERY Claude Code Project_](https://youtu.be/aQvpqlSiUIQ).

---

## Install

**1. Install yt-dlp** (the skill uses it to fetch captions):

```bash
# Ubuntu / Debian (Python 3.12+)
pip install --user --break-system-packages yt-dlp

# macOS
brew install yt-dlp

# anywhere with pipx
pipx install yt-dlp
```

Make sure it's on your `PATH`. The skill defaults to `~/.local/bin/yt-dlp` — adjust `SKILL.md` if yours lives elsewhere.

**2. Clone this repo and copy the skills + command:**

```bash
git clone https://github.com/varshilgandhi/claude-code-youtube-notes.git
cd claude-code-youtube-notes
cp -r skills/youtube-notes ~/.claude/skills/
cp -r skills/dev-tips      ~/.claude/skills/
mkdir -p ~/.claude/commands && cp commands/yt-tip.md ~/.claude/commands/
```

**3. Create the output folder + seed the index:**

```bash
mkdir -p ~/.claude/youtube-notes/{videos,transcripts}
cp examples/INDEX.md ~/.claude/youtube-notes/INDEX.md  # optional — seeds with section structure
```

That's it. Restart Claude Code so it picks up the new skills.

---

## Use

### Capture (youtube-notes)

Paste a YouTube URL in any Claude Code conversation:

```
Notes from this please: https://youtu.be/aQvpqlSiUIQ
```

The skill auto-fires. After a minute or two, you have:

- A new file at `~/.claude/youtube-notes/videos/<id>-<slug>.md`
- The transcript at `~/.claude/youtube-notes/transcripts/<id>.txt`
- An updated row in `~/.claude/youtube-notes/INDEX.md`

You can also explicitly call it: `Use the youtube-notes skill on https://youtu.be/...`

### Apply (dev-tips)

Just describe what you're working on. When a takeaway from your notes genuinely applies, Claude prepends a tip box like this to its reply:

```markdown
💡 From your YouTube notes

- **Plan mode is the most underused feature** — AI with Avthar, How I Start EVERY Claude Code Project
  Why this applies: you're starting a multi-file feature — plan mode catches ambiguity before edits.
- **Be explicit about your tech stack** — AI with Avthar, How I Start EVERY Claude Code Project
  Why this applies: you haven't specified DB/auth in your spec yet.

---
```

…then continues with the actual task. The skill is conservative: it stays silent for quick lookups and narrow code edits.

For an explicit pull, use the slash command:

```
/yt-tip                    # surface the most useful unticked items for current context
/yt-tip planning           # filter to a topic
/yt-tip claude.md
/yt-tip hooks
```

When you adopt a suggestion in a session, the matching item in `INDEX.md`'s Quick-Apply Backlog flips from `- [ ]` to `- [x]` automatically and gets logged in the "Recently Adopted" section.

---

## Example output

Real run on Avthar's [_How I Start EVERY Claude Code Project_](https://youtu.be/aQvpqlSiUIQ) (34:15) → 15 takeaways extracted in ~90 seconds, full file at [`examples/videos/aQvpqlSiUIQ-psb-system.md`](./examples/videos/aQvpqlSiUIQ-psb-system.md).

<!--
SCREENSHOT SLOT: Once a preview image exists at docs/preview.png, replace this
HTML comment with:    ![Notes preview](docs/preview.png)

Quick way to capture one:
  1. Open examples/videos/aQvpqlSiUIQ-psb-system.md on github.com in your browser.
  2. Screenshot the rendered top half (TL;DR + first 2-3 takeaways).
  3. Drag-and-drop the PNG into a new GitHub issue's text area on this repo —
     GitHub uploads it and gives you a CDN URL like
     https://github.com/user-attachments/assets/<uuid>.
  4. Paste that URL into the README in place of this comment.
  (Or commit the PNG to docs/preview.png and reference it locally.)
-->


A few takeaways pulled directly from that file:

> ### 11. Plan mode is the single most underused feature
>
> **What:** Avthar says the most common mistake people make with Claude Code is not using plan mode enough. Plan mode = Claude lays out the approach, asks clarifying questions, doesn't touch files until you approve.
> **How to apply:** Default to plan mode for any task touching more than one file. Press `Shift+Tab` twice or type `/plan`. If you've been one-shotting features, change the default.
> **Source:** [00:26:32]

> ### 12. The `#` (hash) trick — push learnings into `claude.md` on the fly
>
> **What:** When Claude makes a mistake, don't just fix it. Type `#` followed by an instruction — Claude will save that instruction into the relevant `claude.md` automatically. This is "regression prevention" — turn every mistake into a permanent rule.
> **How to apply:** After any "no, don't do X, do Y" correction, immediately follow up with `# When doing <thing>, always do Y, never X`. Costs 5 seconds, prevents the mistake forever.
> **Source:** [00:31:47]
> **Why:** This is the closest thing Claude Code has to a "learn from this mistake" button.

<details>
<summary><b>Click to see all 15 takeaway titles</b></summary>

1. Before opening Claude Code, answer two questions on paper
2. Force the AI to ask *you* clarifying questions during planning
3. Use voice mode when ideas are fuzzy
4. Be ruthlessly specific in product requirements
5. Be explicit about your tech stack — Claude will pick random ones if you don't
6. Provision infra during Plan, not Build
7. Keep `claude.md` finite — link off to other docs
8. Set up four "automated docs" Claude updates as it works
9. Pre-configure permissions in settings — stop Claude from waiting on you
10. Stop hook: keep Claude going until tests pass
11. Plan mode is the single most underused feature
12. The `#` (hash) trick — push learnings into `claude.md` on the fly
13. Model selection: Opus for plan/complex, Sonnet for implementation, Haiku for trivial
14. Three build workflows — pick by task shape
15. Code is cheap — throw away aggressively

Each one has What / How to apply / Source timestamp / Why in the [full file](./examples/videos/aQvpqlSiUIQ-psb-system.md).
</details>

---

## Customize

Open `~/.claude/skills/youtube-notes/SKILL.md` and tune the prompt to your taste. Common changes:

- **Different note style:** Edit the "Write the per-video file" section template — switch from actionable to comprehensive summary, bullet-only, or a hybrid.
- **Different output location:** Change every `~/.claude/youtube-notes/` reference to wherever you want notes to live.
- **More/fewer takeaways:** Adjust the "Aim for 5–15 high-quality insights" line.
- **Different language:** The skill prefers manual English captions. Adjust the `--sub-lang` flag and the prompt language for non-English source videos.

---

## How it works under the hood

1. `yt-dlp --skip-download` fetches video metadata (title, channel, duration).
2. `yt-dlp --write-auto-sub --write-sub` downloads VTT captions — manual captions when available, auto-generated as fallback.
3. A small Python step strips VTT formatting, dedupes overlapping cues (auto-captions repeat lines a lot), and produces a clean transcript with `[HH:MM:SS]` markers every minute.
4. Claude reads the transcript end to end and extracts notes following the template in `SKILL.md`.
5. Output is written to per-video files, the master index is updated, and sponsor segments are explicitly called out in a "Skipped" section.

No video download — captions only. Fast and cheap.

---

## Limitations

- **No captions = no notes.** Some videos have neither manual nor auto-generated captions. The skill will tell you and stop.
- **Long videos cost more context.** A 2-hour video transcript can run 100K+ tokens. The skill works fine but you'll feel it.
- **YouTube rate limits.** If you process many videos in a row, yt-dlp may hit `HTTP 429`. Wait a few minutes.
- **English-first.** Works for non-English with tweaks but the default template assumes English source.

---

## Credits

- Example notes are from [_How I Start EVERY Claude Code Project_](https://youtu.be/aQvpqlSiUIQ) by [AI with Avthar](https://www.youtube.com/@AIwithAvthar) — go subscribe to him, the video is genuinely good.
- Built with [Claude Code](https://claude.com/claude-code).

---

## License

MIT — see [LICENSE](./LICENSE).

# claude-code-youtube-notes

A Claude Code skill that turns any YouTube video into **actionable notes** — instead of you watching the whole video and taking notes manually.

Paste a YouTube URL in any Claude Code conversation. The skill auto-fires, downloads the captions, reads the full transcript, extracts 5–15 sharp takeaways with timestamps and "how to apply" lines, and writes structured markdown to `~/.claude/youtube-notes/`.

Built because I kept watching long Claude Code / AI engineering videos, taking sloppy notes, and never actually applying what I learned. This system makes the apply step the default.

---

## What you get

```
~/.claude/youtube-notes/
├── INDEX.md                     # master index — every video + a quick-apply backlog
├── transcripts/<id>.txt         # cleaned full transcripts
└── videos/<id>-<slug>.md        # per-video actionable notes
```

Each per-video file has:
- **TL;DR** — 2–3 sentences of what the video is actually about
- **Top Takeaways** — each with **What / How to apply / Source timestamp / Why** (when non-obvious)
- **Skipped** — sponsor reads and pitches called out by timestamp so you know they weren't missed
- **Quick-apply checklist** — the tightest version of every takeaway

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

**2. Drop the skill into your Claude Code skills folder:**

```bash
mkdir -p ~/.claude/skills/youtube-notes
curl -fsSL https://raw.githubusercontent.com/varshilgandhi/claude-code-youtube-notes/main/skills/youtube-notes/SKILL.md \
  -o ~/.claude/skills/youtube-notes/SKILL.md
```

Or clone this repo and copy:

```bash
git clone https://github.com/varshilgandhi/claude-code-youtube-notes.git
cp -r claude-code-youtube-notes/skills/youtube-notes ~/.claude/skills/
```

**3. Create the output folder:**

```bash
mkdir -p ~/.claude/youtube-notes/{videos,transcripts}
```

That's it.

---

## Use

Open Claude Code in any project. Paste a YouTube URL with a hint that you want notes:

```
Notes from this please: https://youtu.be/aQvpqlSiUIQ
```

The skill auto-triggers. After a minute or two, you'll have:

- A new file at `~/.claude/youtube-notes/videos/<id>-<slug>.md`
- The transcript saved at `~/.claude/youtube-notes/transcripts/<id>.txt`
- An updated row in `~/.claude/youtube-notes/INDEX.md`

You can also explicitly call it: `Use the youtube-notes skill on https://youtu.be/...`

---

## Example output

From [_How I Start EVERY Claude Code Project_](https://youtu.be/aQvpqlSiUIQ) by [AI with Avthar](https://www.youtube.com/@AIwithAvthar):

> ### 12. The `#` (hash) trick — push learnings into `claude.md` on the fly
>
> **What:** When Claude makes a mistake, don't just fix it. Type `#` followed by an instruction — Claude will save that instruction into the relevant `claude.md` automatically. This is "regression prevention" — turn every mistake into a permanent rule.
>
> **How to apply:** After any "no, don't do X, do Y" correction, immediately follow up with `# When doing <thing>, always do Y, never X`. Costs 5 seconds, prevents the mistake forever.
>
> **Source:** [00:31:47]
>
> **Why:** This is the closest thing Claude Code has to a "learn from this mistake" button.

Full notes file: [`examples/videos/aQvpqlSiUIQ-psb-system.md`](./examples/videos/aQvpqlSiUIQ-psb-system.md).

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

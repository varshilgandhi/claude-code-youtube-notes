---
name: youtube-notes
description: Extract actionable notes from a YouTube video. TRIGGER when the user pastes a YouTube URL (youtube.com/watch, youtu.be/, youtube.com/shorts/) and either asks for notes/summary/takeaways, or the URL appears in a context about learning/watching/studying. Writes structured notes to ~/.claude/youtube-notes/. SKIP if the user is sharing a link for a different reason (e.g. debugging an embed, asking about the URL format itself, or just chatting).
---

# YouTube Notes

You extract **actionable, practical notes** from YouTube videos and store them in `~/.claude/youtube-notes/`. The user is Varshil — a developer who watches Claude Code / AI engineering content and wants to apply techniques in his daily work without having to watch the whole video.

## Output structure

```
~/.claude/youtube-notes/
├── INDEX.md                          # master index — update on every run
├── transcripts/<videoId>.txt           # cleaned transcript (raw material)
└── videos/<YYYY-MM-DD>-<slug>.md       # per-video actionable notes (upload date + slug)
```

## Workflow

### 1. Fetch metadata + transcript

Use yt-dlp (installed at `~/.local/bin/yt-dlp`):

```bash
~/.local/bin/yt-dlp --skip-download \
  --print "%(id)s|%(title)s|%(uploader)s|%(duration_string)s|%(upload_date)s" \
  "<URL>"
```

Then download captions:

```bash
mkdir -p /tmp/yt-work && cd /tmp/yt-work
~/.local/bin/yt-dlp --skip-download --write-auto-sub --write-sub \
  --sub-lang "en.*" --sub-format "vtt/best" \
  --output "%(id)s.%(ext)s" "<URL>"
```

Prefer manual captions (`<id>.en-US.vtt` or `<id>.en.vtt`) over auto-generated (`<id>.en-orig.vtt`) — manual is cleaner. Auto is fine if manual is missing.

### 2. Clean VTT → transcript

Run this Python to strip VTT formatting and dedupe:

```python
import re
from pathlib import Path
import sys

vid = sys.argv[1]
candidates = [f"{vid}.en-US.vtt", f"{vid}.en.vtt", f"{vid}.en-orig.vtt"]
src = next((Path(c) for c in candidates if Path(c).exists()), None)
text = src.read_text()

lines, seen = [], set()
for block in re.split(r'\n\n+', text):
    parts = block.strip().split("\n")
    ts = next((p for p in parts if "-->" in p), None)
    if not ts: continue
    start = ts.split("-->")[0].strip().split(".")[0]
    content = " ".join(p for p in parts if "-->" not in p and not p.strip().isdigit())
    content = re.sub(r"<[^>]+>", "", content).strip()
    if content and content not in seen:
        seen.add(content); lines.append((start, content))

out, last = [], None
for start, c in lines:
    bucket = start.rsplit(":", 1)[0]
    if bucket != last: out.append(f"\n[{start}]"); last = bucket
    out.append(c)

Path(f"transcript-{vid}.txt").write_text("\n".join(out))
```

Then copy the cleaned transcript to `~/.claude/youtube-notes/transcripts/<id>.txt` — you'll often want to re-read it later for follow-up questions.

### 3. Read it and extract notes

**Read the entire transcript** before writing notes. Don't skim. The actionable insights are often buried mid-sentence.

For each insight, capture:
- **What** it is (one sentence)
- **How to apply** it (concrete: command, prompt, file change, habit)
- **Source** timestamp in `[HH:MM:SS]` format
- **Why** (only if the reasoning is non-obvious)

Skip:
- Sponsor reads, course/product pitches, "subscribe" segments
- Generic motivation ("you'll level up your skills") with no concrete technique
- Repetition of an earlier point

Aim for **5-15 high-quality insights** per video. Better to have 5 sharp ones than 30 fluffy ones.

### 4. Write the per-video file

**Filename convention:** `~/.claude/youtube-notes/videos/<YYYY-MM-DD>-<slug>.md`
- `YYYY-MM-DD` is the **upload date** of the video (so the folder sorts chronologically)
- `slug` is a 2-4 word kebab-case summary of the topic, not just the title verbatim
- The `video_id` is preserved in the frontmatter for traceability — don't put it in the filename

**YAML title gotcha:** if the title contains a colon (`:`) or starts with a quote character (`"`), wrap the whole title in **single quotes** in the YAML frontmatter. Double quotes inside double quotes break the YAML parser. Examples:
- `title: 'How I Start EVERY Claude Code Project'` (safe)
- `title: '"Software Fundamentals Matter More Than Ever" — Matt Pocock'` (inner quotes need single-quote wrapper)
- `title: '"Missions: Multi-Agent Systems That Ship for Days" — Luke Alvoeiro'` (inner colon AND inner quotes)

Template:

```markdown
---
video_id: <id>
title: '<full title — single-quoted to be safe>'
channel: <uploader>
duration: <HH:MM>
url: https://youtu.be/<id>
watched: <today's date YYYY-MM-DD>
upload_date: <YYYY-MM-DD>
---

# <title>

**Channel:** <channel> | **Length:** <duration> | **URL:** <url>

## TL;DR

<2-3 sentence summary of what the video is actually about and who should care>

## Top Takeaways

### 1. <Insight title>

**What:** <one sentence>

**How to apply:** <concrete: a command, a prompt, a file change, a habit>

**Source:** [HH:MM:SS]

**Why** _(optional, only if non-obvious)_: <reasoning>

### 2. ...

## Skipped
```

**Formatting note:** Always put a **blank line between each `**Label:**` row** within a takeaway (What / How to apply / Source / Why). Without blank lines, Markdown collapses them into a single running paragraph and they render as one wall of text on GitHub. Each label deserves its own paragraph block.

```markdown
## Skipped

<one line noting any sponsor / pitch / off-topic segments and their timestamps, so future-you knows you didn't just miss them>

## Quick-apply checklist

- [ ] <tightest version of takeaway 1>
- [ ] <tightest version of takeaway 2>
- ...
```

### 5. Update INDEX.md

Add a row to the "Videos Processed" table (newest at top), and append any new checklist items to "Quick-Apply Backlog" — but **dedupe** against existing items. If a takeaway from this video has already appeared in the backlog from a prior video, don't add it again; instead consider adding it to "Top Cross-Video Themes" if it's now appeared in 2+ videos.

### 6. Tell the user where to find it

Brief message: video file path + 2-3 most striking takeaways. Don't dump the whole notes file into chat — they can open it.

## Edge cases

- **No captions available:** Tell the user. Don't fabricate notes. Suggest they find a different video on the same topic, or paste a written summary they trust.
- **Non-English video:** yt-dlp can fetch other languages — ask the user if they want notes in English or the source language before processing.
- **Live stream / very long (>2h):** Ask the user if they want full coverage or just a specific section (timestamp range).
- **Already processed:** Filenames don't carry the video id, so check via `grep -l "video_id: <id>" ~/.claude/youtube-notes/videos/*.md`. If a match exists, ask before overwriting. Default to opening the existing file instead of regenerating.
- **Tool not installed:** If `~/.local/bin/yt-dlp` is missing, install with `pip install --user --break-system-packages yt-dlp` (Python 3.12+ on Debian/Ubuntu needs `--break-system-packages`).
- **HTTP 429 from YouTube:** Wait and retry once. If it persists, tell the user.

## Don'ts

- Don't write notes from the title or thumbnail alone — you must read the transcript.
- Don't include sponsor segments or course pitches in takeaways.
- Don't pad with generic advice ("plan before you build") that adds nothing — every takeaway needs a specific, applicable action.
- Don't overwrite an existing per-video file without asking.

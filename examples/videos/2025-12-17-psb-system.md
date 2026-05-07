---
video_id: aQvpqlSiUIQ
title: How I Start EVERY Claude Code Project
channel: AI with Avthar
duration: 34:15
url: https://youtu.be/aQvpqlSiUIQ
watched: 2026-05-07
upload_date: 2025-12-17
---

# How I Start EVERY Claude Code Project

**Channel:** AI with Avthar | **Length:** 34:15 | **URL:** https://youtu.be/aQvpqlSiUIQ

## TL;DR

Avthar's full system for starting a Claude Code project: **PSB = Plan → Setup → Build**. The Plan phase produces a project spec doc (product requirements + engineering design). The Setup phase is a 7-step Claude Code config checklist. The Build phase has 3 workflows (general, issue-based, multi-agent). The single biggest leverage point in the whole video: **most people don't use plan mode enough**.

## Top Takeaways

### 1. Before opening Claude Code, answer two questions on paper

**What:** (a) What is the goal of this project — learn, validate, ship, or prototype? (b) What are the milestones of functionality, broken into MVP + v2 + v3?

**How to apply:** Open a Google Doc or notebook. Don't touch Claude Code until both questions are answered. The answers determine how much polish vs speed you need and what to cut from v1.

**Source:** [00:02:32]

**Why:** "Prototype to see if it works" vs "ship to real users" leads to wildly different code — Claude can't infer which one you want.

### 2. Force the AI to ask *you* clarifying questions during planning

**What:** Instead of dumping a brief and asking Claude to build, give Claude your brainstorm and prompt: _"What are the three most important questions I need to answer in order to build an MVP of this successfully?"_

**How to apply:** Use this prompt verbatim at the start of every new project, and reuse it for any sub-task (tech choices, debugging, refactor scoping). Save as a slash command if you reuse it often.

**Source:** [00:04:51]

### 3. Use voice mode when ideas are fuzzy

**What:** When you can't yet write down what you want, talk it through with Claude/ChatGPT voice mode, then ask it to summarize the conversation into a markdown file.

**How to apply:** Open the Claude or ChatGPT mobile app, talk for 5-10 min about user flows, ask "summarize this into a project brief I can use as input to Claude Code." Drop the output into your repo as the seed of your spec doc.

**Source:** [00:05:28]

### 4. Be ruthlessly specific in product requirements

**What:** Don't write "users can create journal entries." Spell out: blank page or prompted? photos? edit past entries? Claude fills ambiguity with assumptions you'll regret.

**How to apply:** For every user action in your spec, write at least 3 lines about what the interaction looks like. If you can't, you don't yet know what you're building.

**Source:** [00:07:30]

### 5. Be explicit about your tech stack — Claude will pick random ones if you don't

**What:** Specify language, framework, DB, hosting, auth, payments, etc. up front. Don't let Claude inject Prisma when you wanted Drizzle.

**How to apply:** Add a `## Tech Stack` section to your spec doc with one line per choice. If unsure, ask Claude to produce a research report with options + recommendation.

**Source:** [00:08:42]

**Why:** Avthar's reference stack: Next.js + Tailwind + shadcn on Vercel; Mongo or Supabase; Clerk auth; Stripe; Resend email; DigitalOcean for backend; Cloudflare R2 storage; Anthropic for AI text, Gemini Nano Banana for images.

### 6. Provision infra during Plan, not Build

**What:** Create the database, set up hosting, generate API keys *before* you start building. Otherwise Claude stops mid-flow waiting for credentials.

**How to apply:** End the Plan phase with a checklist: DB created? Hosting connected to GitHub? API keys in `.env`? Don't let Claude touch the keyboard until all green.

**Source:** [00:11:13]

### 7. Keep `claude.md` finite — link off to other docs

**What:** `claude.md` is loaded into every conversation, so bloat = wasted context. Put pointers in `claude.md`, put detail in `architecture.md`, `project-status.md`, etc.

**How to apply:** `claude.md` should contain: project goals, architecture summary, design/UX rules, constraints (e.g. "never push to main directly"), git etiquette, common build/test commands. Detail lives in linked files.

**Source:** [00:13:50]

### 8. Set up four "automated docs" Claude updates as it works

**What:** Four docs that live alongside `claude.md` and get auto-updated:
- `architecture.md` — system design, components, how they interact
- `changelog.md` — running list of changes
- `project-status.md` — milestones, what's done, where you left off (huge for resuming after a few days away)
- Per-feature reference docs for anything important (auth, payments, time zones, etc.)

**How to apply:** Add an instruction in `claude.md`: _"After every feature, update `architecture.md`, `changelog.md`, and `project-status.md`."_ Or build a `/update-docs` slash command and run it after each feature.

**Source:** [00:15:49]

### 9. Pre-configure permissions in settings — stop Claude from waiting on you

**What:** Add allowlists in `~/.claude/settings.json` (or project settings) so Claude doesn't pause for approval on routine commands like git, file edits, etc.

**How to apply:** Use the `/fewer-permission-prompts` skill (already on your machine) — it scans your history and proposes a project-scoped allowlist. Run it after a week of building any new project.

**Source:** [00:23:24]

**Why:** "Thinking Claude is working, going for coffee, coming back to find it was stuck on permission" — exact pain we all have.

### 10. Stop hook: keep Claude going until tests pass

**What:** Hooks let you inject deterministic logic into the Claude Code lifecycle. Avthar uses a Stop hook that runs the test suite when Claude finishes; if tests fail, the hook tells Claude to keep going and fix them.

**How to apply:** Use the `/update-config` skill to add a Stop hook to `settings.json`. Start with one hook; doesn't have to be the test one — even a Slack notification when Claude needs permission is a win.

**Source:** [00:24:22]

### 11. Plan mode is the single most underused feature

**What:** Avthar says the most common mistake people make with Claude Code is not using plan mode enough. Plan mode = Claude lays out the approach, asks clarifying questions, doesn't touch files until you approve.

**How to apply:** Default to plan mode for any task touching more than one file. Press `Shift+Tab` twice or type `/plan`. If you've been one-shotting features, change the default.

**Source:** [00:26:32]

### 12. The `#` (hash) trick — push learnings into `claude.md` on the fly

**What:** When Claude makes a mistake, don't just fix it. Type `#` followed by an instruction — Claude will save that instruction into the relevant `claude.md` automatically. This is "regression prevention" — turn every mistake into a permanent rule.

**How to apply:** After any "no, don't do X, do Y" correction, immediately follow up with `# When doing <thing>, always do Y, never X`. Costs 5 seconds, prevents the mistake forever.

**Source:** [00:31:47]

**Why:** This is the closest thing Claude Code has to a "learn from this mistake" button.

### 13. Model selection: Opus for plan/complex, Sonnet for implementation, Haiku for trivial

**What:** Time saved by a smarter model > money saved by a cheaper one that makes mistakes you have to fix.

**How to apply:** Default to Opus for planning and complex tasks, Sonnet as the implementation workhorse, Haiku only for simple bug fixes or trivial tweaks. Use `/model` to switch.

**Source:** [00:30:30]

### 14. Three build workflows — pick by task shape

**What:**
- **General workflow** (Research → Plan → Implement → Test): single feature end-to-end
- **Issue-based**: GitHub issues = source of truth; ask Claude to convert your spec into issues, then work them. Cleaner repo, parallelizable.
- **Multi-agent / multi-Claude with git worktrees**: multiple Claude Code instances working on isolated branches simultaneously. Most powerful, most setup overhead.

**How to apply:** Start with general. Add issue-based once your project has 5+ open features/bugs. Reach for multi-agent only when you're confident in the first two and have 3+ truly independent features.

**Source:** [00:25:26]

### 15. Code is cheap — throw away aggressively

**What:** If a feature isn't working, don't keep patching. Delete it and restart from a checkpoint. Claude Code's session checkpoints + git rewind give you cheap recovery.

**How to apply:** Set a 30-min "stuck timer." If you're still patching the same feature after 30 min of failed fixes, rewind to the last checkpoint and restart with a clearer plan.

**Source:** [00:32:38]

## Skipped

- [00:01:54] Self-intro and channel subscribe ask
- [00:33:24] Maven course pitch with $100 off code "YouTube"
- [00:34:01] Like/subscribe outro

## Quick-apply checklist

- [ ] Write a project spec doc (product reqs + tech stack + milestones) before next project starts
- [ ] Add the "what 3 questions do I need to answer for MVP?" prompt to a slash command
- [ ] Try voice-mode brainstorming for the next fuzzy idea
- [ ] Audit your current `claude.md` — move detail to `architecture.md` and link from `claude.md`
- [ ] Set up the four automated docs (`architecture.md`, `changelog.md`, `project-status.md`, per-feature refs) on next project
- [ ] Run `/fewer-permission-prompts` after a week on any new project
- [ ] Try one Stop hook — even just a Slack notification
- [ ] Default to plan mode for any multi-file task
- [ ] Practice the `#` trick — every correction becomes a permanent rule
- [ ] Use Opus for planning, Sonnet for implementation
- [ ] Try issue-based dev once you have 5+ open features
- [ ] Set a 30-min "stuck timer" — rewind instead of patching past it

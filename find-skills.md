---
name: find-skills
description: Use before creating a new skill, or when a task looks like it might already have a skill for it. Checks the personal Skills library (github.com/UnnayanMishra/Skills) first, then local .claude/skills/, before ever brainstorming or writing a new one from a blank page.
---

# Find Skills

Finds an existing skill for a task before a new one gets written. Two different
things use skills.sh here, don't conflate them:

- **Runtime lookup (Steps 1–2 below):** NEVER checks skills.sh or any third-party
  marketplace. Only the personal `Skills` repo, then local `.claude/skills/`.
- **Authoring a brand-new skill (Step 3):** DOES check skills.sh first, as a source
  to adapt from, so a new skill isn't written from a blank page when good prior art
  exists publicly. What gets saved afterward is still the adapted, repo-specific
  version — not a copy-paste of the public one.

Check sources in this exact order and stop at the first match:

## Step 1: Check the personal Skills library (only external source)

Repo: `https://github.com/UnnayanMishra/Skills`, cloned locally at
`/Users/unnayanmishra/Documents/Skills/knowledge-base/`.

1. Pull the latest before searching, so a stale local clone doesn't hide a skill that
   was added elsewhere: `cd /Users/unnayanmishra/Documents/Skills/knowledge-base && git pull`.
2. Grep/search `skills/` there. In this repo skills are stored **flat** —
   `skills/<task-name>.md`, one file per skill, no per-skill folder — for a file
   matching the task, by topic, not just filename.
3. If found: this file is content only, not an installable skill yet — a project's
   `.claude/skills/` needs the `<name>/SKILL.md` folder shape. Convert it locally
   before using it:
   ```
   mkdir -p .claude/skills/<task-name>
   cp /Users/unnayanmishra/Documents/Skills/knowledge-base/skills/<task-name>.md \
      .claude/skills/<task-name>/SKILL.md
   ```
   (or `~/.claude/skills/<task-name>/SKILL.md` for a global install). Do this
   conversion every time a library skill is used — the flat `.md` in the library is
   never itself the thing that gets loaded as a skill. Do not fall through to the
   steps below once converted and in place.

## Step 2: Check local skills

If nothing matched in the Skills repo, check locally, same as any other repetitive
task:

- `.claude/skills/` (project-local)
- `~/.claude/skills/` (global)

If a match exists here, use it instead of the steps below.

## Step 3: Check skills.sh for prior art, then brainstorm and create

Only if Steps 1 and 2 both come up empty, and the task is non-trivial enough to
justify a skill (not a one-off):

1. **Check skills.sh first, before writing anything from scratch:**
   `npx --yes skills find "<topic>"`, sorted by installs. If a well-installed,
   reputable result exists (prefer 1K+ installs; check source reputation), fetch its
   full content — `npx skills add <owner/repo@skill>` to inspect it, or fetch the raw
   `SKILL.md` from its GitHub repo — and use it as the starting structure to adapt,
   not something to install as-is.
2. **Adapt it to this system's conventions**, don't just copy it in:
   - Strip vendor/tool lock-in that doesn't match this repo (e.g. a specific SaaS
     platform or workflow engine the original assumed).
   - Add the **Requirements to proceed** section (see the template) — the original
     public skill likely has no "ask before assuming" behavior; that's the part that
     makes it usable here instead of just generically plausible-sounding.
   - Reshape it into this repo's `SKILL.md` sections (When to use this /
     Prerequisites / Requirements to proceed / Steps / Validation / Notes).
   - Credit the source at the bottom (repo + skill name) so it's clear what was
     adapted vs. original.
   - If nothing relevant/trustworthy turns up on skills.sh, skip straight to writing
     it from the template.
3. Think the approach through in `docs/brainstorm.md` first if it's still non-obvious
   after adapting (see the "Deep Thinking" rule in CLAUDE.md) — don't jump straight
   to writing steps from the first idea.
4. Start from the shape at
   `/Users/unnayanmishra/Documents/Skills/templates/skills/example-skill/SKILL.md`.
5. Save it to `.claude/skills/<task-name>/SKILL.md` (project-local, default) or
   `~/.claude/skills/<task-name>/SKILL.md` (global, only if clearly reusable across
   projects).
6. If the skill is generic enough to be useful in *other* projects too, also add a
   **flat copy** — `/Users/unnayanmishra/Documents/Skills/knowledge-base/skills/<task-name>.md`,
   not a folder — and push it, so Step 1 finds it next time instead of it being
   rebuilt from scratch elsewhere. (The working copy stays a proper
   `<task-name>/SKILL.md` folder in `.claude/skills/`; only the library copy is
   flattened.)

## Why this order

Checking the personal library first means a skill only ever gets built once, and
every project benefits from it afterward — re-deriving an approach that's already
been solved and saved is exactly what this whole system exists to avoid.

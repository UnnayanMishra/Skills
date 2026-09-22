---
name: find-skills
description: Use before creating a new skill, or when a task looks like it might already have a skill for it. Checks the personal Skills library (github.com/UnnayanMishra/Skills) first, then local .claude/skills/, before ever brainstorming or writing a new one.
---

# Find Skills

Finds an existing skill for a task before a new one gets written from scratch. This
does NOT search the public skills.sh registry or any third-party marketplace — the
only external source is the personal `Skills` repo. Check sources in this exact
order and stop at the first match:

## Step 1: Check the personal Skills library (only external source)

Repo: `https://github.com/UnnayanMishra/Skills`, cloned locally at
`~/Documents/Skills-Library/`.

1. Pull the latest before searching, so a stale local clone doesn't hide a skill that
   was added elsewhere: `cd ~/Documents/Skills-Library && git pull`.
2. Grep/search it for a file matching the task (by topic, not just filename — a
   `.md` there may describe a skill without being named after it exactly).
3. If found: use it as-is. Do not fall through to the steps below.

## Step 2: Check local skills

If nothing matched in the Skills repo, check locally, same as any other repetitive
task:

- `.claude/skills/` (project-local)
- `~/.claude/skills/` (global)

If a match exists here, use it instead of the steps below.

## Step 3: Brainstorm and create one, if the task actually needs it

Only if Steps 1 and 2 both come up empty, and the task is non-trivial enough to
justify a skill (not a one-off):

1. Think it through in `docs/brainstorm.md` first if the approach itself is
   non-obvious (see the "Deep Thinking" rule in CLAUDE.md) — don't jump straight to
   writing steps from the first idea.
2. Copy the shape from
   `/Users/unnayanmishra/Documents/Skills/templates/skills/example-skill/SKILL.md`.
3. Save it to `.claude/skills/<task-name>/SKILL.md` (project-local, default) or
   `~/.claude/skills/<task-name>/SKILL.md` (global, only if clearly reusable across
   projects).
4. If the skill is generic enough to be useful in *other* projects too, also add a
   copy to `~/Documents/Skills-Library/` and push it, so Step 1 finds it next time
   instead of it being rebuilt from scratch elsewhere.

## Why this order

Checking the personal library first means a skill only ever gets built once, and
every project benefits from it afterward — re-deriving an approach that's already
been solved and saved is exactly what this whole system exists to avoid.

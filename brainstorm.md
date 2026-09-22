# Brainstorm Log

Working memory for thinking through non-trivial decisions before committing to an
approach. This is not a polished doc — it's the whiteboard-session reasoning of a
senior engineer / consultant: what was considered, what breaks each option, and why
one was picked.

How this file is used (full rules live in CLAUDE.md, "Deep Thinking -> brainstorm.md"):

1. Before starting fresh thinking on a topic, check below for an existing entry on it.
   If found, state how much still applies before reusing it.
2. If new thinking is needed, add an entry below using the structure template.
3. If a brainstorm leads to a firm decision, promote the outcome into
   `docs/decisions.md` — this file keeps the reasoning trail, decisions.md keeps the
   final call.

---

## Entry template (copy this for each new topic)

```md
## [YYYY-MM-DD] Topic / question being thought through

**Problem:** what's actually being decided or solved, in plain terms.

**What exists already:** relevant prior art — architecture.md, decisions.md, earlier
entries in this file, or the knowledge-base repo. State explicitly whether it applies
as-is, partially, or not at all.

**Options considered:**
1. Option A — how it works, why it's tempting
2. Option B — how it works, why it's tempting
3. (as many as are genuinely plausible — don't pad with strawmen)

**Edge cases / failure modes per option:** what breaks each one, at scale or under
real-world conditions, not just the happy path.

**Trade-offs:** cost, complexity, maintainability, time-to-ship, reversibility.

**Recommendation:** which option, and the one or two reasons that actually tipped it.

**Open questions:** anything still needing a human decision before proceeding.
```

---

<!-- New entries go below this line, most recent first -->

## [2026-09-22] Which skills to seed the library with for common project types

**Problem:** which skills are worth pre-building into this library for Data
Engineering, Lead Data Engineering, Data Science/GenAI, DevOps, and Full Stack
projects, so future projects in these domains don't start from zero.

**What exists already:** only `find-skills.md` (the lookup workflow) and
`example-skill/SKILL.md` (the empty shape). No domain skills yet.

**Options considered (per domain, ranked by leverage — highest-frequency /
highest-cost-when-skipped task, not just "common"):**

1. **Data Engineering** → `design-data-pipeline`. Almost every DE ticket is some
   variant of "move data from A to B correctly." Idempotency, backfills, and schema
   evolution are the recurring failure points, and getting this pattern right once
   pays off on every pipeline after. Runner-ups: `write-dbt-model`,
   `debug-data-quality-issue`.
2. **Lead Data Engineer** → `write-technical-design-doc`. The thing that's actually
   different about the "lead" role isn't more pipelines, it's turning an ambiguous
   ask into a reviewable design before code gets written — that's what scales a
   lead's judgment across a team instead of gatekeeping every PR. Runner-ups:
   `review-data-architecture-pr`, `plan-data-migration`.
3. **Data Science / GenAI** → `design-genai-eval-harness`. GenAI features fail
   silently — no crash, just a wrong or hallucinated answer — so the eval harness
   (accuracy, hallucination rate, latency, cost) has to exist before the feature
   ships, not after a user complains. More foundational than prompt-writing itself.
   Runner-ups: `build-rag-pipeline`, `debug-model-drift`.
4. **DevOps** → `debug-production-incident`. Deployment playbooks are useful, but
   incident response is where a missing process costs the most (extended downtime,
   repeated root causes). A runbook (triage → mitigate → root-cause → postmortem)
   is the highest-stakes recurring task. Runner-ups: `set-up-ci-cd-pipeline`,
   `set-up-monitoring-and-alerting`.
5. **Full Stack** → `ship-a-feature-end-to-end`. The atomic unit of full-stack work
   is a single feature touching API + frontend + tests together, not any one layer
   in isolation — a skill scoped to just "add an API endpoint" misses the wiring and
   test-coverage steps that actually cause bugs. Runner-ups: `db-migration`,
   `set-up-auth-flow`.

**Edge cases / failure modes per option:** a skill written generically (no real repo
paths/commands) is nearly useless per this system's own rule — "Repetitive Tasks ->
Skills" requires real conventions, not generic advice. Pre-seeding skeletons here
without a real project to anchor them risks producing exactly the generic advice the
system is designed to avoid.

**Trade-offs:** writing full skills now (5+ domains) vs. writing thin skeletons that
get filled in with real specifics the first time each is actually used in a project.
Full skills up front cost more now and risk staleness before ever being used;
skeletons cost less now and guarantee accuracy since they're filled in from a real
task.

**Recommendation:** write thin skeletons (structure + guidance comments, no invented
commands/paths) for the 5 top picks now, so `find-skills` has something to match
against — but flag each skeleton's steps as `<!-- TODO: fill in from real repo -->`
and treat the first real use of each as the point where it gets filled in with actual
conventions, not before.

**Open questions:** should the 4 runner-up skills per domain (20 total) also get
skeletons now, or only on demand when a second real occurrence of that task happens?
Leaning toward on-demand per the "2+ times" rule already in CLAUDE.md — only the 5
top picks are common enough to justify seeding ahead of time.
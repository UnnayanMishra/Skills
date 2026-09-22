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
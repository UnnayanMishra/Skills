---
name: design-data-pipeline
description: Use when designing a new data pipeline or ETL/ELT workflow — moving data from a source to a destination with transformation in between. Covers idempotency, backfills, schema evolution, scheduling, monitoring, and data quality checks.
---

# Design Data Pipeline

<!--
Adapted from claude-office-skills/skills@data-pipeline (skills.sh, 4.3K installs,
fetched 2026-09-22). Original was n8n-flavored and vendor-specific (Shopify/Stripe ->
BigQuery/Sheets); vendor lock-in stripped here, and a Requirements-to-proceed section
added — the original had no "ask before assuming" behavior, which is the actual gap
this adaptation closes. See knowledge-base/brainstorm.md, 2026-09-22 entry, for why
idempotency/backfill/schema-evolution were picked as the emphasis over generic ETL
mechanics.
-->

## When to use this

Use when asked to design, build, or review a data pipeline / ETL / ELT workflow —
before writing extraction, transformation, or load code, not after.

## Prerequisites / context to check first

- `docs/architecture.md` — does an existing pipeline/warehouse pattern already exist
  in this repo that this should match, rather than introducing a second pattern?
- `docs/decisions.md` — has a similar pipeline decision already been made and
  recorded (tooling choice, schema convention)?

## Requirements to proceed

A pipeline design is only as good as the assumptions it's built on. These change the
actual architecture, not just the details — if any are missing, STOP and ask rather
than picking a default:

- **Source(s):** what system(s) is the data coming from (database, API, files,
  webhook, SaaS app)?
  - *Ask if missing:* "Where does this data currently live, and how is it accessed
    today — direct DB access, an API, exported files?"
- **Destination:** where must the data land, and does anything downstream expect a
  specific shape?
  - *Ask if missing:* "What's the destination system, and does anything downstream
    (dashboards, another service) expect a specific schema?"
- **Volume & frequency:** how much data per run, how often does the source change?
  - *Ask if missing:* "Roughly how many rows/records per run, and how often does the
    source data actually change?"
- **Latency / freshness SLA:** how stale is the destination data allowed to be?
  - *Ask if missing:* "Is there a hard freshness SLA, or is 'eventually consistent'
    (e.g. nightly) fine?"
- **Idempotency / replay requirement:** can the pipeline safely re-run the same
  window without duplicating data?
  - *Ask if missing:* "If this pipeline fails halfway and gets re-run, is it OK if
    some records get processed twice, or does it need to be exactly-once?"
- **Schema stability:** should the pipeline react to a source schema change by
  failing loudly, adapting automatically, or alerting and continuing?
  - *Ask if missing:* "Does the source schema change often — and if a new or renamed
    field shows up, should the pipeline break loudly or just ignore it?"
- **Failure handling:** on a partial failure, does the rest of the batch still go
  through, or must the whole run fail together?
  - *Ask if missing:* "If one record fails transformation, should the rest of the
    batch still load, or does the whole run need to fail?"

## Steps

1. **Extract/transform/load shape** — decide where transformation happens (source
   query, in-pipeline code, or a downstream modeling layer like dbt) based on where
   it's cheapest and safest to run for this stack.
   <!-- TODO: fill in this repo's actual extraction/orchestration tooling on first real use -->
2. **Idempotency** — design the write step so re-running the same window is safe:
   upsert on a natural/business key, or a full-partition overwrite — never a blind
   append on retry.
3. **Backfill path** — provide an explicit way to re-run historical windows, not
   just the incremental/latest-window path; a pipeline that can only run "forward"
   will eventually need a backfill and won't have one.
4. **Schema evolution** — decide up front, per the Requirements answer above,
   whether new/changed source fields get dropped, passed through, or block the
   pipeline. Don't leave this implicit.
5. **Scheduling** — cadence matching the freshness SLA from Requirements, plus
   explicit dependency ordering if this pipeline depends on another completing
   first.
6. **Monitoring & alerts** — track row count, execution time, error count, and data
   freshness at minimum. Alert on pipeline failure AND on data-quality anomalies
   (row count off by a threshold, unexpected nulls, schema drift) — a pipeline that
   only alerts on hard crashes misses silent data-quality failures.
7. **Data quality checks** — schema validation (required fields, types, allowed
   values), statistical checks (null rate, duplicate rate, value ranges), and any
   business-rule checks specific to this data (e.g. "total equals sum of line
   items").

## Validation

- [ ] Re-running the same window twice produces the same result (idempotency check)
- [ ] A historical window can be backfilled without special-casing code
- [ ] An unexpected/renamed source field doesn't silently corrupt output
- [ ] A failed run triggers an alert — silent staleness is not an acceptable failure mode

## Notes / known pitfalls

- Blind `INSERT`/append on retry is the single most common cause of duplicated data
  in pipelines — design the write step to be idempotent from the start, not bolted
  on after an incident.
- "We'll add monitoring later" is how silent data-freshness failures happen — build
  the freshness/anomaly alert alongside the pipeline, not after.
- Link to a `docs/decisions.md` entry if this pattern was the result of a past
  trial-and-error.

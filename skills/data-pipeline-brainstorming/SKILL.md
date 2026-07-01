---
name: data-pipeline-brainstorming
description: "You MUST use before any pipeline creative work - ETL, new tables, layer changes, Airflow/Databricks/GE work. Explores intent, lineage, incremental/idempotency decisions, and produces a design spec before plans or code."
---

# Data Pipeline Brainstorming

Turn fuzzy pipeline requests into an approved **design spec** before plans, STTM finalization, or code.

<HARD-GATE>
Do NOT invoke `data-pipeline-plans`, write pipeline code, create Airflow DAGs, Databricks jobs, GE suites, or STTM plan-complete fields until the design spec is written and **DesignApproved**.
</HARD-GATE>

## Announce

"I'm using the data-pipeline-brainstorming skill to design this pipeline."

## Checklist

Create a todo per item; complete in order:

1. **Explore context** — repos, existing pipelines, layers, source systems
2. **Confirm architecture mode** — extended Medallion vs other; which layers apply
3. **Clarify scope** — one question at a time: domain, iteration layer stop, sources
4. **Ask DE depth questions** — incremental key, idempotency, SLA, rerun vs skip, schema drift
5. **Propose 2–3 approaches** — trade-offs; recommend one
6. **Present design in sections** (200–300 words each); user validates each section
7. **Write design spec** — copy from `docs/data-engineering/templates/design-spec-template.md` to `docs/data-engineering/specs/YYYY-MM-DD-<domain>-design.md`
8. **Create STTM skeletons** — one YAML per target table in scope; `status: skeleton` only
9. **Close must-resolve items** — cross-layer contracts have **no TBD**
10. **Approval block** — fill `## Approval`; obtain **DesignApproved** (written human quote)

## Layer taxonomy (non-PII)

`bronze` → `silver-cdm` → `silver-ref` / `silver` (star) → `gold-mart` → `gold-app` → `gold-proxy`

Ask which **target layers** this iteration touches. PII layers deferred unless user enables them.

## Required design sections

Use template sections 1–4:

1. Problem & goals (iteration layer stop)
2. Approach comparison
3. Data architecture (lineage Mermaid, table inventory, **cross-layer decisions**)
4. Risks & gates (GE/reconciliation, open questions, must-close checklist)

## STTM at design phase

For each in-scope target table, create:

`docs/data-engineering/sttm/<domain>/<target_table>.yaml`

Fill: metadata, source/target identity, layer, `status: skeleton`. Incremental keys may be TBD if listed in Approval **Allowed table-level TBD**.

## DesignApproved criteria

- `## Approval` block complete with human sign-off quote
- Incremental/watermark, idempotency, layer order, timezone, schema drift — **decided**
- All "must resolve before plan = yes" items closed

## After DesignApproved

Ask: "Ready to write the implementation plan?"

Invoke **`data-pipeline-plans`** — NOT implementation skills.

## One question at a time

Prefer multiple choice. Do not ask five questions in one message.

## Red Flags

| Thought | Reality |
|---------|---------|
| "Simple sync, no design" | ETL needs lineage and incremental decisions |
| "We'll figure out merge keys in code" | Merge keys are design-level unless in Allowed TBD |
| "Skip spec, go to plan" | DesignApproved is required |

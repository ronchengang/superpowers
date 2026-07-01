---
name: data-pipeline-plans
description: Use after DesignApproved to create rolling pipeline plans - 00-overview, one layer sub-plan at a time, full STTM per layer, before execution
---

# Data Pipeline Plans

Write **rolling layer plans** with mandatory STTM, lineage links, and task fields for Databricks + Airflow + GE on Azure.

<HARD-GATE>
Requires **DesignApproved** design spec. Do NOT execute code. Do NOT write layer N+1 sub-plan until layer N **ExecutionSignOff** (use `data-pipeline-execution` for that gate).
</HARD-GATE>

## Announce

"I'm using the data-pipeline-plans skill to create the pipeline plan."

## Prerequisites

- [ ] Design spec exists with **DesignApproved**
- [ ] STTM skeletons exist for in-scope tables

## Document set (per domain)

```
docs/data-engineering/plans/YYYY-MM-DD-<domain>/
├── 00-overview.md          # Thin; derived from design spec
├── 01-bronze.md            # First layer — rolling
├── 02-silver-cdm.md        # After bronze ExecutionSignOff
├── ...
├── 99-gates-signoff.md
└── lineage/
    └── 01-bronze.mmd
```

STTM: `docs/data-engineering/sttm/<domain>/<table>.yaml`

Templates: `docs/data-engineering/templates/`

## Rolling workflow

1. Write **`00-overview.md`** + **first layer sub-plan only** (usually `01-bronze.md`)
2. For each in-scope table in that layer:
   - Complete STTM to **plan_complete** (all template fields — see `sttm-table-template.yaml`)
   - Add **task group** to layer sub-plan
3. Define **layer dependency order** inside sub-plan before task groups
4. Submit for **PlanApproved** — STOP until human written approval
5. After layer **ExecutionSignOff**, write **next** layer sub-plan; update overview TBDs

## Overview rules

- 2–3 pages max; no option comparison essays
- `Derived from:` link to design spec
- Mermaid summary + table inventory with STTM/plan links

## Task group rules

One **task group per target table**. Each group MUST include:

| Field | Required |
|-------|----------|
| Target layer / table | yes |
| STTM link | yes |
| Incremental key / partition / load_type | yes |
| Upstream dependencies | yes |
| File paths (notebook, DAG, GE) | yes |
| Airflow dag_id / task_id | yes |
| Databricks job or notebook | yes |
| GE expectations | yes |
| Reconciliation SQL / KPI | yes |
| Verification commands | yes |
| Lineage update target | yes |
| Design reference | yes |
| Sign-off item id | yes |
| Done definition | yes |

**Missing any field = invalid plan.**

## Micro-steps (inside each task group)

1. Confirm STTM frozen fields match PlanApproved
2. Failing pytest + failing GE
3. Implement PySpark/SQL
4. Dev job pass
5. Update lineage + STTM implementation_detail fields
6. Airflow wiring
7. Commit

For `incremental` / `merge`: include idempotency verification command in plan.

## PlanApproved

Human must reply with explicit approval (e.g. "Plan OK for bronze — start execution") before `data-pipeline-execution`.

Record pending/approved in layer sub-plan header.

## After PlanApproved

Invoke **`data-pipeline-execution`**.

## Amendment

Frozen STTM field changes → Plan Amendment + re-PlanApproved. Cross-layer contract changes → design spec + DesignApproved.

See `docs/data-engineering/templates/sttm-field-mutability.md`.

## Red Flags

| Thought | Reality |
|---------|---------|
| "Write all layer plans upfront" | Rolling: only current layer after previous sign-off |
| "STTM later" | PlanApproved requires full STTM for layer tables |
| "Skip overview" | Overview links contracts and lineage |

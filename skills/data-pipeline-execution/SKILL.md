---
name: data-pipeline-execution
description: Use after PlanApproved to execute one layer sub-plan with hard stops for gates, amendments, and ExecutionSignOff before the next layer plan
---

# Data Pipeline Execution

Execute **one layer sub-plan** with mandatory stops for approvals, quality gates, and sign-off documentation.

<HARD-GATE>
Requires **PlanApproved** for the current layer sub-plan. Do NOT start if plan header shows PlanApproved pending.
</HARD-GATE>

## Announce

"I'm using the data-pipeline-execution skill to execute layer [name]."

## Before first task

1. Read `00-overview.md` and current `0N-<layer>.md`
2. Confirm **PlanApproved** quote recorded in plan header
3. Confirm previous layer **ExecutionSignOff** in `99-gates-signoff.md` (except first layer)
4. Create todos from task groups in dependency order

## Per task group (one table)

Follow micro-steps in the layer plan exactly:

1. STTM frozen field check
2. **Failing** pytest + GE (`test-driven-development` interim skill)
3. Implement
4. Dev Databricks run
5. Update repo lineage (`.mmd` + overview status) + STTM impl fields
6. Airflow integration
7. Commit

### Idempotency (mandatory)

For STTM `load_type` of **incremental** or **merge**:

- Run idempotency test (same fixture/partition twice — row count + primary key set match)
- Record evidence in `99-gates-signoff.md` idempotency table

## Layer completion gates (all required)

Before ExecutionSignOff:

| Gate | Evidence |
|------|----------|
| Airflow | Run id(s) — green |
| Great Expectations | Checkpoint name + pass |
| Reconciliation | SQL/KPI result |
| Runtime lineage | Unity Catalog / OpenLineage link |
| STTM version | git commit hash |
| Idempotency | Table for incremental/merge loads |

Fill **`99-gates-signoff.md`** layer section completely. Template: `docs/data-engineering/templates/gates-signoff-template.md`.

## ExecutionSignOff

**Hard stop.** Ask human for written sign-off. Paste quote into sign-off block.

Only after ExecutionSignOff:

- Invoke **`data-pipeline-plans`** to write the **next** layer sub-plan
- Update overview changelog

## Plan amendment mid-execution

| Change | Action |
|--------|--------|
| `implementation_detail` fields only | Proceed; note in task |
| Frozen STTM / plan fields | Stop; amend plan + STTM; **Plan Amendment** in sign-off; re-PlanApproved |
| Cross-layer contract | Stop; update design spec; **DesignApproved** again |

## When to stop and ask

- Plan step unclear
- GE or reconciliation fails repeatedly
- Missing Airflow/Databricks access
- Frozen field needs change

Do not skip to next layer. Do not write next layer plan without ExecutionSignOff.

## Parallelism

**Same layer, independent tables:** may use `subagent-driven-development` for parallel task groups after dependency order is respected.

**Never parallelize across layers.**

## Phase 1 testing note

Use `test-driven-development` until `data-pipeline-testing` ships. Still require **both** pytest and GE to fail before implementation per plan.

## Red Flags

| Thought | Reality |
|---------|---------|
| "DAG passed, next layer" | GE + reconciliation + sign-off missing |
| "Fix merge key in code" | Frozen — plan amendment |
| "Sign-off verbal only" | Record exact quote in 99-gates-signoff |

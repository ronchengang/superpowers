# STTM Field Mutability

After **PlanApproved** for the layer containing this STTM, agents may only edit fields marked `implementation_detail` without a plan amendment.

## Editable after PlanApproved (`implementation_detail`)

| Field | Notes |
|-------|-------|
| `transform_rule` | Business logic detail |
| `spark_sql_snippet` | Implementation |
| `notes` | Clarifications |
| `ge_expectations[].kwargs` | Threshold tuning only — cannot remove mandatory expectations or change `expectation_type` |
| `airflow_task_id` | Concrete task id/path |
| `databricks_job_or_notebook` | Concrete job or notebook path |

## Frozen after PlanApproved (plan amendment + re-approval required)

| Field | Notes |
|-------|-------|
| `source.*` | Source system/table/column semantics |
| `target.*` / `target_layer` | Target identity |
| `load_semantics.*` | incremental_key, partitions, load_type, merge_keys |
| `orchestration.airflow_dag_id` | DAG identity |
| `lineage.upstream_tables` / `downstream_tables` | Contract edges |
| `quality.ge_expectations[].expectation_type` | Expectation kind |
| `quality.ge_expectations[].mandatory` | Cannot demote mandatory checks |

## Cross-layer contract changes

If a frozen change affects `00-overview.md` cross-layer contracts → update design spec and obtain **DesignApproved** again.

## Plan amendment record

Log in `99-gates-signoff.md` under **Plan Amendment** for the affected layer.

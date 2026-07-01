# Gates & Sign-off: [Domain]

> **Template:** `docs/data-engineering/templates/gates-signoff-template.md`  
> **Path:** `docs/data-engineering/plans/YYYY-MM-DD-<domain>/99-gates-signoff.md`

## DesignApproved

| Field | Value |
|-------|-------|
| Design spec | |
| Approved by | |
| Date | |
| Allowed table TBD | |

---

## Layer: bronze

### Execution results

| Field | Value |
|-------|-------|
| Date | |
| Airflow run id | |
| Databricks run id | |
| GE checkpoint | _name + pass/fail_ |
| Reconciliation | _one-line conclusion_ |

### Evidence

| Type | Link / id |
|------|-----------|
| GE Data Docs / report | |
| Reconciliation SQL / query id | |
| STTM git ref | _commit or file hash_ |
| Runtime lineage (UC / OpenLineage) | |

### Deviations & decisions

_[Plan vs actual; known limits; backfill scope]_

### Idempotency (incremental/merge tables)

| Table | Test command | Result | Run id |
|-------|--------------|--------|--------|
| | | pass/fail | |

### ExecutionSignOff

| Field | Value |
|-------|-------|
| Approved by | |
| Date | |
| Sign-off quote | _paste exact message_ |

### Next layer entry

| Field | Value |
|-------|-------|
| Next layer plan file | `02-silver-cdm.md` |
| PlanApproved status | pending / approved |
| Carryover todos | |

---

## Plan Amendment (layer: _name_)

_Use when frozen STTM/plan fields change mid-layer._

| Field | Value |
|-------|-------|
| Reason | |
| Diff / PR link | |
| Re-PlanApproved quote | |
| Design revision required? | yes / no |

---

<!-- Copy the "Layer: ..." section for each subsequent layer -->

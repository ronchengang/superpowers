# Data Engineering Superpowers Fork — Design Spec

> **Status:** Approved baseline for Phase 1 implementation  
> **Stack:** Databricks, Airflow, Great Expectations, PySpark, SQL, Azure  
> **Harness:** Cursor + Claude Code (primary)

## 1. Problem & goals

**Problem:** Generic Superpowers skills optimize for application code. Data pipelines need cross-layer lineage, STTM, quality gates, idempotency, and documentation at every step—not only notebooks and DAGs.

**Goals:**

- Force fuzzy ETL requirements into explicit design before code
- Enforce layer gates: Airflow → GE → reconciliation → human sign-off
- Produce STTM, lineage, and sign-off artifacts alongside code
- Support end-to-end business-line delivery across extended Medallion layers

**Success criteria (Phase 1):** Agent can run design → overview + first layer plan → execution with hard stops on missing approvals, using templates in `docs/data-engineering/templates/`.

## 2. Approach comparison

| Approach | Summary | Verdict |
|----------|---------|---------|
| Big bang replace all skills | Fast consistency, high test blast radius | Rejected for Phase 1 |
| **Phased replace (chosen)** | Templates + DE skills first; testing/debugging Phase 3 | **Selected** |
| Dual-track generic + DE | Easier upstream merge, agent confusion | Rejected |

**Why phased:** Templates and approval gates must exist before skill checklists are meaningful. Cursor + Claude Code bootstrap can be validated per phase.

## 3. Data architecture decisions

### Layer taxonomy (non-PII)

`bronze` → `silver-cdm` → `silver-ref` / `silver` (star) → `gold-mart` → `gold-app` → `gold-proxy`

Project-specific patterns vary; skills branch on **target layer** after architecture mode is chosen.

### Work unit

**Cross-layer E2E** per business domain—not single-table-only plans.

### Cross-layer contracts (frozen at DesignApproved)

- Incremental / watermark strategy
- Idempotency and rerun scope
- Timezone handling
- Schema drift policy
- Layer execution order

Table-level fields may remain TBD if listed in the design **Approval** block.

### Lineage

- **Design lineage:** repo Mermaid (`00-overview` + `plans/.../lineage/<layer>.mmd`)
- **Runtime lineage:** Databricks Unity Catalog / OpenLineage — links recorded in sign-off

## 4. Risks, gates & open items

### Layer execution gates (each layer)

1. Airflow run success
2. Great Expectations checkpoint pass
3. Business reconciliation (row counts / KPIs)
4. Human ExecutionSignOff

### Plan gate (each layer)

Written **PlanApproved** before any code/DAG/notebook changes for that layer.

### Testing (Phase 1 interim)

Use `test-driven-development` until `data-pipeline-testing` ships. Target state: pytest + GE both red before implementation; mandatory idempotency test for incremental/merge loads with sign-off evidence.

### Phase 1 scope

- Templates, `using-data-superpowers`, `data-pipeline-brainstorming`, `data-pipeline-plans`, `data-pipeline-execution`
- Archive generic workflow skills to `skills/_archived/`
- **Not in Phase 1:** `data-pipeline-testing`, `data-pipeline-debugging`, PII layer branches

### Items closed before plan work

- [x] Document structure (spec vs overview vs layer plans vs STTM)
- [x] Approval gate model
- [x] STTM mutability rules
- [x] Archive strategy for upstream skills

## Approval

| Field | Value |
|-------|-------|
| **Scope snapshot** | Phase 1 DE fork; non-PII layers; Cursor + Claude Code |
| **Allowed TBD at design** | Table-level STTM fields, per-table GE thresholds |
| **Frozen at design** | Cross-layer incremental/idempotency/layer order |
| **Approved by** | _(human partner — fill on review)_ |
| **Date** | _(fill on review)_ |

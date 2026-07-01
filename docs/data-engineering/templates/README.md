# Templates index

Copy these when starting a new pipeline domain.

| Template | Use for |
|----------|---------|
| [design-spec-template.md](./design-spec-template.md) | Brainstorming output → `specs/` |
| [overview-template.md](./overview-template.md) | Plan overview → `plans/<domain>/00-overview.md` |
| [layer-subplan-template.md](./layer-subplan-template.md) | Per-layer plan → `plans/<domain>/0N-<layer>.md` |
| [sttm-table-template.yaml](./sttm-table-template.yaml) | Per-table STTM → `sttm/<domain>/` |
| [sttm-field-mutability.md](./sttm-field-mutability.md) | Frozen vs editable STTM fields |
| [gates-signoff-template.md](./gates-signoff-template.md) | Layer gates → `99-gates-signoff.md` |

## Quick start

1. `data-pipeline-brainstorming` → design spec + STTM skeletons → **DesignApproved**
2. `data-pipeline-plans` → `00-overview` + `01-bronze` → **PlanApproved**
3. `data-pipeline-execution` → execute bronze → **ExecutionSignOff**
4. Repeat plans + execution for next layer

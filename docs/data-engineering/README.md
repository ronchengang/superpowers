# Data Engineering Superpowers (Fork)

This fork replaces generic Superpowers workflow skills with a **Databricks + Airflow + Great Expectations + PySpark + SQL + Azure** pipeline methodology.

## Document layout

```
docs/data-engineering/
├── README.md                 # This file
├── specs/                    # Thick design specs (brainstorming output)
├── plans/<domain>/           # Rolling plans per business domain
│   ├── 00-overview.md
│   ├── 01-bronze.md …
│   ├── 99-gates-signoff.md
│   └── lineage/*.mmd
├── sttm/<domain>/            # Per-table Source-to-Target Mapping (YAML)
└── templates/                # Copy-paste templates for agents and humans
```

## Three approval gates

1. **DesignApproved** — design spec approved; cross-layer contracts frozen (no TBD on incremental/idempotency/layer order)
2. **PlanApproved** — written approval before executing each layer sub-plan
3. **ExecutionSignOff** — Airflow green + GE + reconciliation + human sign-off before writing the next layer plan

## Layer taxonomy (non-PII)

`bronze` → `silver-cdm` → `silver-ref` / `silver` (star schema) → `gold-mart` → `gold-app` → `gold-proxy`

PII layers (`bronze-pii`, `silver-cdm-pii`) are deferred in Phase 1.

## Phase 1 skills (active)

- `using-data-superpowers` — bootstrap
- `data-pipeline-brainstorming` — design spec
- `data-pipeline-plans` — overview + rolling layer plans + STTM
- `data-pipeline-execution` — PlanApproved / ExecutionSignOff enforcement

Testing temporarily uses archived-adjacent `test-driven-development` until `data-pipeline-testing` lands in Phase 3.

## Harness support (target)

Cursor and Claude Code first. Bootstrap: `hooks/session-start` reads `using-data-superpowers`.

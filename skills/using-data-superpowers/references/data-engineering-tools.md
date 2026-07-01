# Data Engineering Tool Mapping

Skills describe **actions**. Map them to your harness tools.

## Shared paths (this fork)

| Artifact | Path pattern |
|----------|--------------|
| Design spec | `docs/data-engineering/specs/YYYY-MM-DD-<domain>-design.md` |
| Plan overview | `docs/data-engineering/plans/YYYY-MM-DD-<domain>/00-overview.md` |
| Layer plan | `docs/data-engineering/plans/.../0N-<layer>.md` |
| Sign-off | `docs/data-engineering/plans/.../99-gates-signoff.md` |
| STTM | `docs/data-engineering/sttm/<domain>/<table>.yaml` |
| Lineage | `docs/data-engineering/plans/.../lineage/<layer>.mmd` |

## Stack commands (typical)

| Action | Command / location |
|--------|-------------------|
| PySpark unit tests | `pytest tests/... -v` |
| GE checkpoint | `great_expectations checkpoint run <name>` |
| Airflow dev run | `airflow dags test <dag_id> <date>` or UI |
| Databricks | job run via UI, CLI, or API |

## Cursor / Claude Code

| Skill action | Tool |
|--------------|------|
| Invoke skill | Skill tool |
| Read / write files | Read, Write, StrReplace |
| Run tests | Shell |
| Todos | TodoWrite |
| Subagent (same-layer parallel) | Task tool |

See also: [claude-code-tools.md](claude-code-tools.md)

---
name: using-data-superpowers
description: Use when starting any conversation in this data-engineering fork - establishes DE skill workflow, approval gates, and skill invocation before ANY response
---

<SUBAGENT-STOP>
If you were dispatched as a subagent to execute a specific task, skip this skill.
</SUBAGENT-STOP>

<EXTREMELY-IMPORTANT>
If you think there is even a 1% chance a skill might apply, you ABSOLUTELY MUST invoke the skill.

IF A SKILL APPLIES TO YOUR TASK, YOU DO NOT HAVE A CHOICE. YOU MUST USE IT.
</EXTREMELY-IMPORTANT>

## Instruction Priority

1. **User's explicit instructions** (AGENTS.md, CLAUDE.md, direct requests)
2. **Data-engineering Superpowers skills** (this fork)
3. **Default system prompt**

## This fork

Generic workflow skills live in `skills/_archived/` for reference only. **Use DE replacements:**

| Task | Skill |
|------|-------|
| New pipeline / feature design | `data-pipeline-brainstorming` |
| Implementation plan | `data-pipeline-plans` |
| Execute layer plan | `data-pipeline-execution` |
| Unit tests before code (Phase 1 interim) | `test-driven-development` |
| Bugs / test failures | `systematic-debugging` |
| Same-layer parallel tasks | `subagent-driven-development` |

**Stack:** Databricks, Airflow, Great Expectations, PySpark, SQL, Azure.

**Docs root:** `docs/data-engineering/` — templates in `templates/`, specs in `specs/`, plans in `plans/<domain>/`, STTM in `sttm/<domain>/`.

## Three approval gates

1. **DesignApproved** — design spec Approval block; cross-layer contracts have no TBD
2. **PlanApproved** — human written approval before executing each layer sub-plan
3. **ExecutionSignOff** — recorded in `99-gates-signoff.md` before writing the next layer plan

**Never skip gates.** Missing approval = hard stop.

## How to Access Skills

Use your platform's skill-loading mechanism. Do not manually read SKILL.md with file tools unless your platform has no skill tool.

Platform tool mapping: [claude-code-tools.md](references/claude-code-tools.md), [codex-tools.md](references/codex-tools.md), [copilot-tools.md](references/copilot-tools.md), [gemini-tools.md](references/gemini-tools.md).

## Skill Priority

1. **Process first:** `data-pipeline-brainstorming`, `systematic-debugging`
2. **Plans & execution:** `data-pipeline-plans`, `data-pipeline-execution`
3. **Testing (interim):** `test-driven-development`

"Build a pipeline" → `data-pipeline-brainstorming` first.  
"Execute the bronze plan" → `data-pipeline-execution` after PlanApproved.

## Red Flags

| Thought | Reality |
|---------|---------|
| "I'll just update the notebook" | Pipeline work needs skills and STTM/lineage updates |
| "Airflow is green, we're done" | GE + reconciliation + sign-off may be missing |
| "I'll write the silver plan now" | Previous layer ExecutionSignOff required |
| "TBD on incremental key is fine" | Cross-layer contracts must be fixed at DesignApproved |

## Skill Types

**Rigid:** approval gates, `data-pipeline-execution`, `test-driven-development`  
**Flexible:** architecture mode branches within brainstorming

Follow each skill's HARD-GATE sections exactly.

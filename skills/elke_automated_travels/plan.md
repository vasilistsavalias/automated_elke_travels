# Main Agent Plan

This plan is the default runbook for the main agent.

## Objective

Transform one noisy evidence folder into a deterministic submission-ready package with reviewer gating.

## Skill Graph

1. `elke-orchestrator` (controller)
2. `elke-intake-noise-triage`
3. `elke-doc-extraction-normalization`
4. `elke-evidence-matching-reconciliation`
5. `elke-package-assembly-quality-gates`
6. `elke-reviewer-audit-gate`
7. `elke-baseline-comparator` (optional controlled trials)
8. `elke-schema-compliance-check` (final mandatory gate)

## System Prompts Location

All subagent prompts are under:

- `skills/system-prompts/`

## Execution Policy

- Prefer sequential baseline first.
- Enable parallel only for independent tasks.
- Keep deterministic final synthesis.
- Always run reviewer gate before final decision.
- Always run schema-compliance gate as final step.

## Artifact Handoff Contract

Final user-facing delivery must contain only:

- `airplane/`
- `food/`
- `hotel/`
- `transport/`
- `other/`
- `expense_register.xlsx`

Strict rule:

- No additional files or folders are allowed in final output.

Internal artifact routing:

- Use `artifacts_root` for all intermediate stage files, queues, and verification reports.
- Reserve `output_root` only for final user-facing delivery.

## Per-File Analysis Requirement

1. Process each image/pdf independently.
2. Do not read all image/pdf contents into one context window.
3. Preferred mode: separate subagent per file; fallback mode: strict sequential per-file worker.
4. Do not write json/jsonl/trace/state artifacts into final output.

## Business Rule: High Non-Travel Shared Expenses

For categories `food`, `groceries`, `drinks`, `other` with `amount >= 50 EUR`:

1. Do not auto-fill a numeric user-share value.
2. Mark share input as required.
3. In spreadsheet output, set share cell text to `ENTER YOUR SHARE`.
4. Style that share cell in bold red.

Travel-core categories are excluded from this rule.

## Strict Validation Step

Before run completion:

1. Validate `reviewer_audit_report.json` against:
   - `skills/elke-reviewer-audit-gate/templates/reviewer_audit_report.schema.json`
2. If comparator is executed, validate `baseline_comparison_report.json` against:
   - `skills/elke-baseline-comparator/templates/baseline_comparison_report.schema.json`
3. Emit `schema_compliance_report.json` via `elke-schema-compliance-check`.
4. Fail run if schema validation fails.

## Best-Practice Basis

This plan follows common production guidance:

1. Keep architecture simple and composable.
2. Use explicit orchestration and specialist handoffs.
3. Add guardrails and human/reviewer checkpoints.
4. Parallelize only independent subtasks.
5. Evaluate and iterate with explicit comparison reports.

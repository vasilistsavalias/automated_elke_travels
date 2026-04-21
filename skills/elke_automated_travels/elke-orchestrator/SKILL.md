---
name: elke-orchestrator
description: "Use when: running the full ELKE reimbursement pipeline from noisy input to submission-ready output, with sequential baseline and optional parallel subagents."
---

# ELKE Orchestrator

## Goal

Run the end-to-end reimbursement workflow with minimum user effort and strong safety.

## System Prompt

- `skills/system-prompts/main-agent-system.md`

## Inputs

- `input_root`: one noisy mixed folder with no pre-categorization.
- `artifacts_root`: internal working folder for intermediate artifacts and audit reports.
- `output_root`: final delivery root only.
- `baseline_root` (optional): used only for controlled-trial comparison.

## Global Contract

The orchestrator must enforce this final delivery structure in `output_root`:

- `airplane/`
- `food/`
- `hotel/`
- `transport/`
- `other/`
- `expense_register.xlsx`

No additional files or folders are allowed in `output_root`.

## Flow

1. Call `elke-intake-noise-triage`.
2. Call `elke-doc-extraction-normalization`.
3. Call `elke-evidence-matching-reconciliation`.
4. Call `elke-package-assembly-quality-gates`.
5. Call `elke-reviewer-audit-gate`.
6. If `baseline_root` provided, call `elke-baseline-comparator`.
7. Call `elke-schema-compliance-check`.

## Skill Connectivity

- Intake produces inventory + triage + duplicate artifacts in `artifacts_root/02_structured/`.
- Extraction consumes intake artifacts and produces extracted + warning artifacts.
- Reconciliation consumes extraction artifacts and produces matching outputs.
- Packaging consumes prior outputs and emits final package + checklist.
- Reviewer consumes all stage outputs and emits pass/fail audit.
- Comparator consumes baseline + output and emits parity verdict.
- Schema validator consumes reviewer/comparator outputs and emits final schema compliance gate.

## Execution Modes

- Default: sequential.
- Preferred: one independent worker/subagent per file for classification/extraction.
- Optional: parallel subagents for independent workloads only.
- Fallback: if subagents unavailable, stay sequential.

Per-file processing guard:

- Never aggregate all image/pdf contents into one monolithic context.
- Do not write json/jsonl/trace/state artifacts into final output.

Parallel-safe examples:

- document-type routing
- per-file extraction
- per-category reconciliation pre-checks

Non-parallel stage:

- final packaging + final verdict synthesis

## Output Contract

- Final categorized folders in `output_root/{airplane,food,hotel,transport,other}`
- Final workbook in `output_root/expense_register.xlsx`

Internal artifact contract:

- Intermediate and verification files must be written under `artifacts_root`, never in `output_root`.

## Best-Practice Notes

- Keep orchestration simple and composable.
- Increase complexity only when measurable quality improves.
- Use explicit handoffs and machine-checkable contracts between skills.
- Always include guardrails and a reviewer checkpoint before final pass.

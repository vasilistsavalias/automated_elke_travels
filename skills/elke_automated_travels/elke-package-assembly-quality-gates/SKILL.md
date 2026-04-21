---
name: elke-package-assembly-quality-gates
description: "Use when: assembling the final reimbursement package, enforcing deterministic structure, and producing reviewer-ready quality gate reports."
---

# ELKE Package Assembly And Quality Gates

## Purpose

Produce a deterministic final package that is ready for ELKE review.

## System Prompt

- `skills/system-prompts/packaging-subagent-system.md`

## Inputs

- `artifacts_root/00_raw_input/*`
- `artifacts_root/02_structured/inventory_manifest.json`
- `artifacts_root/02_structured/duplicates.json`
- `artifacts_root/02_structured/extracted_records.json`
- `artifacts_root/02_structured/reconciliation.json`
- `artifacts_root/02_structured/share_review_queue.json`
- `artifacts_root/03_verification/manual_review_queue.json`

## Required Structure

- airplane
- food
- hotel
- transport
- other
- expense_register.xlsx

No additional files or folders are allowed in final output.

## Quality Gates

- All source files accounted for in manifest
- Duplicate groups documented
- Low-confidence items listed for manual review
- Unmatched items explicitly listed
- Shared-expense prompt rule correctly applied for high-value non-travel expenses
- Final checklist generated

## Required Outputs

Write reviewer notes (path optional but recommended):

- `artifacts_root/03_verification/review_summary.md`

Write final delivery to `output_root/`:

- `airplane/`
- `food/`
- `hotel/`
- `transport/`
- `other/`
- `expense_register.xlsx`

## Excel Rule For Shared Expenses

In `expense_register.xlsx`:

- For non-travel categories (`food`, `groceries`, `drinks`, `other`) with `amount >= 50 EUR`:
  - set `your_share_eur` cell display text to `ENTER YOUR SHARE`
  - style that cell in bold red

## Excel Totals Row Rule

Append one final totals row after all expense rows:

- `record_id`: `TOTAL`
- `category`: `TOTAL`
- `merchant_or_vendor`: `TOTAL SPENT`
- `amount`: Excel formula summing all expense `amount` cells
- `currency`: `EUR`
- `your_share_eur`: Excel formula summing all numeric `your_share_eur` cells

The `your_share_eur` total is expected to remain incomplete until the user replaces any `ENTER YOUR SHARE` cells with numeric personal-share amounts. The formula must still compute the already-known numeric share cells.

## Handoff

Downstream consumer: `elke-reviewer-audit-gate`.

## Output

- package manifest
- verification summary
- manual action checklist
- expense register spreadsheet

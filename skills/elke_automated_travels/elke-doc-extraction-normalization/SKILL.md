---
name: elke-doc-extraction-normalization
description: "Use when: extracting reimbursement-relevant fields from receipts, statements, tickets, bookings, and related documents with confidence scoring."
---

# ELKE Document Extraction And Normalization

## Purpose

Extract normalized fields from heterogeneous evidence.

## System Prompt

- `skills/system-prompts/extraction-subagent-system.md`

## Inputs

- `artifacts_root/02_structured/inventory_manifest.json`
- `artifacts_root/02_structured/triage_mapping.json`
- `artifacts_root/02_structured/duplicates.json`

## Fields

- date_iso
- amount
- currency
- merchant_or_vendor
- reference_id
- document_type
- expense_category
- is_travel_core_expense
- requires_share_input
- share_prompt_text
- extraction_confidence

## Rules

- Prefer explicit values from document text.
- If uncertain, set null plus warning, not guessed values.
- Keep source trace for every extracted row.
- Process one file per extraction pass; do not aggregate all source file contents in one context.
- Do not emit json/jsonl trace/state artifacts in final output.
- Use explicit confidence bands:
  - `>= 0.80`: high
  - `0.50 - 0.79`: medium
  - `< 0.50`: low (must enter manual review queue)
- Shared-expense threshold rule (default `50 EUR`):
  - Apply only to non-travel categories: `food`, `groceries`, `drinks`, `other`.
  - For records with `currency = EUR` and `amount >= 50`, set:
    - `requires_share_input = true`
    - `share_prompt_text = "ENTER YOUR SHARE"`
  - Do not apply this rule to travel-core categories.

## Required Outputs

Write to `artifacts_root/02_structured/`:

- `extracted_records.json`
- `extraction_warnings.json`
- `share_candidates.json`

Write to `artifacts_root/03_verification/`:

- `manual_review_queue.json` (append-safe)

## Handoff

Downstream consumer: `elke-evidence-matching-reconciliation`.

## Output

- extracted records
- low-confidence queue
- extraction warnings
- share candidates

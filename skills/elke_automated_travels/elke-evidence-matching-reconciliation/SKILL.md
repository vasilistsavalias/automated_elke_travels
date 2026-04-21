---
name: elke-evidence-matching-reconciliation
description: "Use when: matching expense evidence to payment proofs and generating explainable confidence-scored reconciliation results."
---

# ELKE Evidence Matching And Reconciliation

## Purpose

Link expenses to payment proofs and expose unresolved gaps.

## System Prompt

- `skills/system-prompts/matching-subagent-system.md`

## Inputs

- `artifacts_root/02_structured/extracted_records.json`
- `artifacts_root/02_structured/extraction_warnings.json`
- `artifacts_root/02_structured/share_candidates.json`
- `artifacts_root/03_verification/manual_review_queue.json`

## Matching Priority

1. Amount equality or near-equality
2. Date proximity
3. Reference id equality
4. Merchant token overlap

## Required Behavior

- Output one reconciliation row per expense.
- Include reasons for match or non-match.
- Never force a low-confidence match.
- Preserve `requires_share_input` and `share_prompt_text` flags from extraction records.

## Shared-Expense Rule

For non-travel categories (`food`, `groceries`, `drinks`, `other`) with `amount >= 50 EUR`:

- Keep `your_share_eur` unresolved.
- Set share prompt marker text to `ENTER YOUR SHARE`.
- Add those rows to `share_review_queue.json` for manual input.

Travel-core categories (`travel`, `flight`, `transport`, `booking`, `hotel`) are excluded from this rule.

## Required Outputs

Write to `artifacts_root/02_structured/`:

- `reconciliation.json`
- `matching_summary.json`
- `share_review_queue.json`

Update `artifacts_root/03_verification/manual_review_queue.json` with unresolved items.

## Handoff

Downstream consumer: `elke-package-assembly-quality-gates`.

## Output

- matched pairs
- unmatched expenses
- unmatched payments
- reconciliation confidence summary
- share review queue

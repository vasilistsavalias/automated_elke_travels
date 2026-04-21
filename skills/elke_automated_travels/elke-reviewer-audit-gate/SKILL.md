---
name: elke-reviewer-audit-gate
description: "Use when: performing independent reviewer checks on all generated artifacts and issuing a PASS/FAIL gate decision with blockers."
---

# ELKE Reviewer Audit Gate

## Purpose

Provide an evaluator pass independent from generator subagents.

## System Prompt

- `skills/system-prompts/reviewer-subagent-system.md`

## Templates

- `skills/elke-reviewer-audit-gate/templates/reviewer_audit_report.schema.json`
- `skills/elke-reviewer-audit-gate/templates/reviewer_checklist.md`

## Inputs

- `artifacts_root/02_structured/*`
- `artifacts_root/03_verification/*`
- `output_root/*`

## Mandatory Checks

1. Required files exist.
2. File counts are internally consistent.
3. Manual review queue exists and is non-empty when warnings/unmatched items exist.
4. Submission manifest and checklist are present.
5. No missing references in trace fields.
6. Shared-expense rule is applied in `expense_register.xlsx`:
   - non-travel (`food`, `groceries`, `drinks`, `other`) rows with `amount >= 50 EUR`
   - `your_share_eur` shown as `ENTER YOUR SHARE`
   - prompt cell style is bold red

## Required Outputs

Write to `artifacts_root/03_verification/`:

- `reviewer_audit_report.json`
- `reviewer_audit_report.md`

`reviewer_audit_report.json` MUST conform to `reviewer_audit_report.schema.json`.

## Decision Rules

- `PASS`: no blockers.
- `FAIL`: one or more blockers that make submission unsafe.
- Always list blockers and recommended fixes.

## Handoff

Optional downstream consumer: `elke-baseline-comparator` for controlled trial verdict.

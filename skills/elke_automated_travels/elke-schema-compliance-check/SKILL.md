---
name: elke-schema-compliance-check
description: "Use when: validating reviewer/comparator JSON outputs against strict schemas and emitting a final compliance PASS/FAIL report."
---

# ELKE Schema Compliance Check

## Purpose

Provide a deterministic final schema gate for machine-readable outputs.

## System Prompt

- `skills/system-prompts/schema-validator-subagent-system.md`

## Templates

- `skills/elke-schema-compliance-check/templates/schema_compliance_report.schema.json`
- `skills/elke-schema-compliance-check/templates/schema_validation_checklist.md`

## Inputs

- `artifacts_root/03_verification/reviewer_audit_report.json`
- `artifacts_root/03_verification/baseline_comparison_report.json` (optional)
- `skills/elke-reviewer-audit-gate/templates/reviewer_audit_report.schema.json`
- `skills/elke-baseline-comparator/templates/baseline_comparison_report.schema.json`

## Validation Rules

1. Reviewer report must exist and conform to reviewer schema.
2. Comparator report is optional unless baseline comparator was run.
3. If comparator report exists, it must conform to comparator schema.
4. Counts and top-level enums must be consistent with schema constraints.

## Required Outputs

Write to `artifacts_root/03_verification/`:

- `schema_compliance_report.json`
- `schema_compliance_report.md`

`schema_compliance_report.json` MUST conform to `schema_compliance_report.schema.json`.

## Final Decision Rule

- `PASS`: all required schema checks are valid.
- `FAIL`: any required schema check fails.

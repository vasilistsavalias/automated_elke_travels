---
name: elke-baseline-comparator
description: "Use when: running a controlled trial and comparing generated outputs against a baseline folder to classify exact match vs structural equivalence."
---

# ELKE Baseline Comparator

## Purpose

Measure whether a run reproduces baseline outputs exactly or only structurally.

## System Prompt

- `skills/system-prompts/comparator-subagent-system.md`

## Templates

- `skills/elke-baseline-comparator/templates/baseline_comparison_report.schema.json`
- `skills/elke-baseline-comparator/templates/comparator_checklist.md`

## Inputs

- `baseline_root`
- `output_root`
- `artifacts_root`

## Comparison Levels

1. Input parity: file count + extension distribution + hash multiset.
2. Contract parity: required directory/file structure.
3. Output parity: normalized and submission artifacts.

## Required Outputs

Write to `artifacts_root/03_verification/`:

- `baseline_comparison_report.json`
- `baseline_comparison_report.md`

`baseline_comparison_report.json` MUST conform to `baseline_comparison_report.schema.json`.

## Verdict Labels

- `exact_same`
- `structurally_equivalent_but_content_divergent`
- `divergent`

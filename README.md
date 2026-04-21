# Universal AI Skills: ELKE Travel Reimbursement Pack

This repository is a **tool-agnostic skills distribution**.  
It provides reusable skill definitions that can guide **any AI agent runtime** (Codex, Claude-based agents, OpenAI Agents SDK pipelines, custom orchestrators, or internal agent frameworks).

The included pack focuses on ELKE-style travel reimbursement processing.

## What A Skill Pack Contains

Each skill pack is implementation guidance, not app code:

- `SKILL.md`: role, inputs, outputs, rules, handoff contracts
- `templates/`: schemas/checklists/report templates used by gates
- `system-prompts/`: role-specific behavior prompts for orchestration/subagents
- `plan.md`: flow and stage connectivity

## Included Skills (ELKE Pack)

- `elke-orchestrator`: top-level sequencing and contract enforcement
- `elke-intake-noise-triage`: inventory, type triage, duplicate grouping
- `elke-doc-extraction-normalization`: normalized field extraction with confidence
- `elke-evidence-matching-reconciliation`: receipt/payment matching and confidence rationale
- `elke-package-assembly-quality-gates`: deterministic final package + register assembly
- `elke-reviewer-audit-gate`: independent reviewer PASS/FAIL gate
- `elke-baseline-comparator`: optional baseline parity checks
- `elke-schema-compliance-check`: schema validation of reviewer/comparator outputs

## Subagent Roles

The workflow is intentionally decomposed so any agent system can run it with role-specialized workers:

- `Orchestrator`: stage ordering, dependency management, final contract enforcement
- `Intake subagent`: reads noisy input and emits structured inventory/triage artifacts
- `Extraction subagent`: converts evidence into normalized records and warning queues
- `Matching subagent`: links expenses to payment proof with explainable confidence
- `Packaging subagent`: creates strict final folder layout + expense workbook
- `Reviewer subagent`: checks completeness, consistency, and blocker/warning state
- `Comparator subagent`: checks current output against a baseline (when provided)
- `Schema validator subagent`: validates gate reports against JSON schemas

## Final Output Contract (ELKE)

The final output folder must contain exactly:

- `airplane/`
- `food/`
- `hotel/`
- `transport/`
- `other/`
- `expense_register.xlsx`

No extra files/folders are allowed in final output.

## Runtime Compatibility (Works With Everything)

This repository is not tied to one vendor.

- Codex-style runtimes: map `skills/` into your local skill discovery path (for example `.codex/skills/...`).
- Other runtimes: import/copy the same `skills/` tree and wire each `SKILL.md` as an agent task spec.
- Custom orchestrators: use `plan.md` plus templates/schemas as machine-checkable stage contracts.

## Repository Structure

```text
.
|-- LICENSE
|-- README.md
|-- .gitignore
`-- skills/
    `-- elke_automated_travels/
        |-- plan.md
        |-- system-prompts/
        |   |-- main-agent-system.md
        |   |-- intake-subagent-system.md
        |   |-- extraction-subagent-system.md
        |   |-- matching-subagent-system.md
        |   |-- packaging-subagent-system.md
        |   |-- reviewer-subagent-system.md
        |   |-- comparator-subagent-system.md
        |   `-- schema-validator-subagent-system.md
        |-- elke-orchestrator/SKILL.md
        |-- elke-intake-noise-triage/SKILL.md
        |-- elke-doc-extraction-normalization/SKILL.md
        |-- elke-evidence-matching-reconciliation/SKILL.md
        |-- elke-package-assembly-quality-gates/SKILL.md
        |-- elke-reviewer-audit-gate/
        |   |-- SKILL.md
        |   `-- templates/
        |       |-- reviewer_checklist.md
        |       `-- reviewer_audit_report.schema.json
        |-- elke-baseline-comparator/
        |   |-- SKILL.md
        |   `-- templates/
        |       |-- comparator_checklist.md
        |       `-- baseline_comparison_report.schema.json
        `-- elke-schema-compliance-check/
            |-- SKILL.md
            `-- templates/
                |-- schema_validation_checklist.md
                `-- schema_compliance_report.schema.json
```

## License

GNU General Public License v3.0. See `LICENSE`.

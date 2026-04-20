# Travel Reimbursement Automation

This repository implements a skills-first workflow that transforms mixed travel evidence (receipts, statements, invoices, tickets, images, PDFs) into a deterministic reimbursement package.

## Output Contract

Final `output_root/` must contain exactly:

- `airplane/`
- `food/`
- `hotel/`
- `transport/`
- `other/`
- `expense_register.xlsx`

No extra files/folders are allowed in final output.

## Workflow Stages

1. Intake + noise triage
2. Extraction + normalization
3. Evidence matching + reconciliation
4. Package assembly
5. Reviewer audit gate
6. Schema compliance gate

Orchestrator and stage skills live in:

- `.codex/skills/elke_automated_travels/`
- `.codex/skills/elke_automated_travels/plan.md`

## Quick Start (Local)

1. Create/activate a venv.
2. Install dependencies used by the local runner:
   `pypdf`, `pdfplumber`, `openpyxl`, `pillow`, `pymupdf`, `jsonschema`
3. Prepare:
   - `input_root/` with raw evidence files
   - `artifacts_root/` for intermediate outputs
   - `output_root/` for final package
4. Run the local pipeline script (current reference implementation):
   `workspace_skg_rhodes/elke_pipeline.py`
5. Validate:
   - reviewer report is PASS
   - schema report is PASS
   - final `output_root/` matches the strict contract

## Spreadsheet Rules

- `proof_status` values: `Both`, `Statement`, `Receipt`, `Group Receipt`
- For non-travel categories with `amount >= 50 EUR`, `your_share_eur` is set to `ENTER YOUR SHARE` (bold red).
- A final `TOTAL` row is appended:
  - `amount` total formula over all expense rows
  - `your_share_eur` total formula (updates as share cells are filled)

## First Publish Checklist

- Ensure no personal input/output artifacts are tracked (see `.gitignore`).
- Verify README links and paths resolve.
- Include a license file.
- Confirm one reproducible local run command is documented.
- Optional but recommended: add `CONTRIBUTING.md` and release tags/changelog policy.

## Repository Layout

```text
project-root/
|-- .codex/
|   `-- skills/
|       `-- elke_automated_travels/
|-- AGENTS.md
|-- README.md
|-- LICENSE
`-- .gitignore
```

## License

This project is licensed under the GNU General Public License v3.0.
See `LICENSE`.

# Reviewer Checklist Template

Use this checklist before issuing PASS/FAIL.

## Contract Checks

- [ ] Required files exist in 02_structured
- [ ] Required files exist in 03_verification
- [ ] Required files exist in 04_submission_ready

## Consistency Checks

- [ ] File totals are internally consistent across manifests
- [ ] Duplicate groups and inventory are coherent
- [ ] Manual review queue is present when unresolved issues exist
- [ ] Trace references point to existing artifacts
- [ ] For non-travel (`food`, `groceries`, `drinks`, `other`) rows with `amount >= 50 EUR`, `expense_register.xlsx` shows `ENTER YOUR SHARE` in bold red and leaves share value unresolved

## Decision

- [ ] PASS (no blockers)
- [ ] FAIL (at least one blocker)

## Blockers

List blocker id, severity, message, and artifact path.

## Recommendations

List concrete fixes needed for resubmission.

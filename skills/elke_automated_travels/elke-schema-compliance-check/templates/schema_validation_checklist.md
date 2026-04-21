# Schema Validation Checklist Template

## Reviewer Schema

- [ ] reviewer_audit_report.json exists
- [ ] reviewer_audit_report.json validates against reviewer schema

## Comparator Schema

- [ ] comparator required status determined
- [ ] if required, baseline_comparison_report.json exists
- [ ] if present, baseline_comparison_report.json validates against comparator schema

## Final

- [ ] schema_compliance_report.json generated
- [ ] schema_compliance_report.md generated
- [ ] overall_status set to PASS/FAIL correctly

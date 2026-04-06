# LunaNet Submission Authoring Guide

Source basis:

- `Tracks/LunaNet/SUBMISSION_TEMPLATE.pdf`

## Purpose

Build a complete, judge-ready submission package with continuously updated status and evidence instead of a last-minute document scramble.

## Required Template Sections

- Team and repository metadata
- Language/tooling stack
- Architecture summary
- Build/run instructions
- Gateway status (0-8)
- Validation checklist results per gateway
- Performance benchmark table
- Interoperability outcomes (optional but recommended)
- Test summary and known limitations

## Continuous Authoring Workflow

1. Pre-fill static sections in month 1.
2. Update gateway status immediately after each review.
3. Attach evidence references at the same time tests are run.
4. Run submission completeness audits every two weeks.

Why this workflow:

- Prevents stale status and missing evidence.
- Keeps submission quality aligned with implementation progress.

## Evidence Rules

- Each pass/fail check must cite an artifact path.
- Benchmark values must include run context and method.
- Interoperability claims must cite partner and vector IDs.

## 6-Person Submission Split

- Engineer A: architecture/spec alignment narrative
- Engineer B: validation matrix and performance narrative
- Engineer C: interoperability and operational narrative
- Developer A: gateway 1-2 evidence curation
- Developer B: gateway 3-4 and docs/examples curation
- Developer C: gateway 5-8 integration and automation evidence curation

## Final 5-Day Hardening

- T-5: feature freeze and reproducibility check
- T-4: full validation + benchmark rerun
- T-3: interoperability rerun + report finalization
- T-2: build/run instructions dry-run on clean environment
- T-1: final submission package audit

[⬆️ Back to Top ⬆️](#lunanet-submission-authoring-guide)

---

[Back Page](../interoperability/LunaNet-Interoperability-Execution-Plan.md) | [Next Page](../README.md)

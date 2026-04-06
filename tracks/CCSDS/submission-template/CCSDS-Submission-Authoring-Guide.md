# CCSDS Submission Authoring Guide

Source basis:

- `Tracks/CCSDS/SUBMISSION_TEMPLATE.pdf`
- Supporting context from deliverables/requirements/interoperability docs

## Purpose

Produce a complete `SUBMISSION.md` package with consistent evidence, minimizing end-of-project assembly risk.

## Required Submission Blocks

- Team information
- Implementation overview and stack
- Architecture summary
- Build/run instructions
- Gateway status (0-10)
- Validation results by gateway
- Performance benchmarks
- Interoperability outcomes
- Test summary and known limitations

## Authoring Strategy

1. Pre-fill static sections now:
      - Team identity, repository, baseline architecture, tooling stack.
2. Update gateway status continuously:
      - Do not wait for final week to fill statuses.
3. Attach evidence pointers per gateway:
      - Link exact test logs and reports.
4. Freeze and audit before submission:
      - Validate every table row has evidence.

Why this strategy:

- Submission templates fail late when teams treat them as a final-document-only artifact.
- Continuous authoring keeps evidence synchronized with implementation reality.

## Evidence Quality Rules

- Every pass/fail claim must reference a reproducible artifact.
- Performance claims must specify hardware, command, and run count.
- Interop claims must specify partner implementation and test case IDs.

## 6-Person Submission Workflow

- Engineer B: validation tables and compliance narrative owner
- Engineer C: interoperability section owner
- Engineer A: architecture and specification alignment owner
- Developer A: gateway 1-2 technical evidence owner
- Developer B: gateway 3-6 and docs tooling evidence owner
- Developer C: integration, benchmarks, and automation evidence owner

## Final 5-Day Checklist

- T-5: status freeze for new features
- T-4: run full conformance + integration suite
- T-3: run interoperability replay and refresh report
- T-2: regenerate benchmark and memory reports
- T-1: final consistency audit of `SUBMISSION.md` against artifacts

[⬆️ Back to Top ⬆️](#ccsds-submission-authoring-guide)

---

[Back Page](../spec/CCSDS-Spec-Implementation-Guide.md) | [Next Page](../../LunaNet/README.md)

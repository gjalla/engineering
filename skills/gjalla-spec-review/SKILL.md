---
name: gjalla-spec-review
description: Full review of a feature spec across architecture alignment, security posture, quality and robustness, user impact, and governance. Use to harden a spec before implementation.
---

# Specification Review Council

Review this spec from multiple expert perspectives:

## Architect Review
- Does the design fit the existing system and respect layer boundaries?
- Are new components placed in the correct layer and ownership hierarchy?
- Do interactions follow established patterns (sync/async/event)?

## Security Review
- Are all new endpoints authenticated and authorized?
- Does the data model handle sensitive data correctly (PII, secrets)?
- Are inputs validated at the surface-area boundary?

## Guardian Review (Quality)
- Is the testing strategy sufficient for the behavioral requirements?
- Are edge cases and error paths covered?
- Do acceptance criteria map to testable assertions?

## Advocate Review (User Impact)
- Does the feature solve the stated problem?
- Are failure modes graceful from the user's perspective?
- Is the feature discoverable and documented?

## Governance Review
- Does the spec comply with all active project rules and conventions?
- Are any rule exceptions or overrides needed?
- Does this change warrant an architecture decision record (ADR)?

## Output
For each lens, report: what passes, what's at risk, and concrete changes required before implementation. Separate **blocking** concerns from **recommendations**.

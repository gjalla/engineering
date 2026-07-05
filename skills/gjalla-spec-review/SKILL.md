---
name: gjalla-spec-review
description: Full review of a plan or spec to be sure there are no surprises, gaps, or mistakes. Use to harden a spec before implementation.
---

# Comprehensive spec-review

The best, most elegant and effective software systems are ones that are well-informed, well-planned, and verifiable. 
The final output is a plan/spec that can act as a reference doc, covering the problem/motivation, technical approach, deltas (what properties of the system will change once implemented), and verification criteria.

Your task is to review this spec from multiple expert perspectives to ensure that this spec meets our expectations and will result in solid implementations once it's in the hands of the engineers.

The bar: the design should be elegant, well-designed, minimal, maintainable, and not overengineered — something the team would be proud to ship. A spec that merely works but fails that bar is not done.

## Architect Review (reference the gjalla master spec where needed)
- Does the design fit the existing system and respect layer boundaries?
- Are new components placed in the correct layer and ownership hierarchy?
- Is it compatible with the broader system (i.e. cross repository integrations, future goals, etc.?)
- Is it overengineered or introducing complication that will be difficult to understand/maintain?
- Do new commands, endpoints, files, or concepts share a trigger, data source, and output channel with something that already exists? Are established patterns followed and existing code/tools reused?

## Security Review
- Do the changes respect data security and privacy boundaries, such as including authentication and authorization on all new endpoints, and ensuring no cross-user/cross-tenant data leakage?
- Does the data model handle sensitive data correctly (PII, secrets)?
- Are inputs validated at the surface-area boundary?
- Are the data flows changing in a way the user should know about, like different types of data flowing to new consumers, etc.?

## Quality Review
- Is the testing strategy sufficient for the behavioral requirements?
- The goal for AI-generated code is typically 100% test coverage, are positive, negative, and edge cases covered? Are appropriate and effective integration tests scope based on the end-goals?
- Do acceptance criteria map to testable assertions?

## User Experience Advocate Review
- Does the feature solve the stated problem?
- Are failure modes graceful from the user's perspective?
- Is the feature discoverable and documented?

## Governance Review
- Does the spec comply with all active project rules and conventions? (stored in gjalla - check there if needed)
- Are any rule exceptions or approvals needed?
- Does this change warrant an architecture decision record (ADR)? (most do not, but ones that introduce significant, new tradeoffs may)

## Output
For each perspective, report: what passes, what's at risk, and concrete changes required before implementation. Separate **blocking** concerns from **recommendations**.

---
name: gjalla-code-review
description: Review a code change (diff or PR) for correctness, security, and quality before merge. Use when reviewing your own or someone else's changes prior to merging.
---

# Code Review

Review the actual change — not the plan. Read the diff and the surrounding code it touches.

## Orient
1. Understand intent: what is this change supposed to do? (PR description, linked spec, commit messages.)
2. Read the diff in full, then open each changed file to see the change in context — a diff hides what it doesn't touch.
3. Identify blast radius: callers of changed functions, data the change reads/writes, public contracts altered.

## Review lenses
Go through each; report findings per lens.

### Correctness
- Does the code do what it claims? Trace the main path and at least one error path.
- Off-by-one, null/empty/boundary inputs, unhandled error returns, swallowed exceptions.
- Concurrency: shared state, races, ordering assumptions, non-atomic read-modify-write.
- Look for no-op or dead code — does it actually change behavior the way the description says?

### Security
- Untrusted input validated and escaped at the boundary (injection, path traversal, SSRF).
- AuthN/AuthZ on new endpoints and data access — can a caller reach data they shouldn't?
- Secrets, tokens, PII: not logged, not embedded in URLs, not returned in responses.
- Dependencies added: trusted, pinned, and actually necessary.

### Quality & maintainability
- Matches existing patterns and conventions in the surrounding code.
- No needless complexity; no logic duplicated from somewhere it already exists.
- Names say what they mean; no leftover TODO/FIXME or debug logging.
- Tests exercise the real code path and would fail if the change were wrong (see gjalla-test-audit).

### Tests & docs
- New behavior has tests for the happy path, error paths, and edge cases from the intent.
- Public-contract or operational changes are documented.

## Output
Group findings by severity:
- **Blocking** — must fix before merge (correctness, security, data loss).
- **Should fix** — quality/maintainability issues worth addressing now.
- **Nit** — optional polish.

For each finding: `file:line`, what's wrong, and a concrete suggested fix. State what you verified and what you could not.

## Principles
- Review the code that's there AND what's missing — the dangerous bug is usually the unhandled case, not the wrong line.
- Be specific: a finding without a location and a fix is noise.
- Approve only what you understand. If you can't tell whether it's correct, say so rather than rubber-stamping.

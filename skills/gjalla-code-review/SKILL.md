---
name: gjalla-code-review
description: Review a code change (diff or PR) for ship-readiness before merge. Use when reviewing your own or someone else's changes prior to committing/merging/etc.
---

# Code Review

Your task is to review the code changes as a team lead with extremely high standards.
You care about: simple, elegantly-designed, maintainable code; code which meets the expectations and verification criteria; code which is well-tested, robust, secure, and production-ready; and code that balances everything you know about the end user, the company, and the feature.

## Approach
1. Understand intent: what is this change supposed to do? (PR description, linked spec, commit messages.)
2. Put yourself in the shoes of multi-faceted reviewers based on what is relevant to this change. For instance, the perspective of a senior software engineer who is skilled at code correctness / edge cases / bugs / dead code, a product manager who is great at user experience and voice of the user, a QA engineer looking for quality and maintainability, a customer success manager responsible for implementation and value delivery, a security analyst looking for data access / privacy / security / vulnerabilities, an architect, etc.. No need to a) use all of these personas or b) use only these personas. Instead, using the context of this change, determine what the most helpful, adversarial, and gap-filling approach would be and take it.
3. From each perspective, review the diff in full, using second order thinking (what collateral, implicit, or second-order implications do these changes have?) and fresh eyes. The quality lenses that always apply: is it minimal, elegant, effective, not overengineered, and maintainable?
4. Based on what you've found, what is the priority of these items within the context of this user/product/company?

## Review until clean

If this change is loop-bound, enter the review loop first: `gjalla loop advance --to review`.

A review with findings is not the end of the process — it's the middle. A non-clean review mandates another full round with fresh eyes after the fixes land, until a round comes back clean (no blocking or must-fix changes; cap at 5 rounds).

- "Fresh eyes" is literal: where your agent supports subagents, spawn a fresh-context reviewer for each round rather than re-reading your own work — self-review after self-fix is grading your own homework. Where you can't, disclose it: add `--evidence review_mode=self` to the marks below.
- Record every round in the ledger so the loop is visible and provable:

```
gjalla ledger mark review-round --round <N> --evidence result=<clean|not-clean>
gjalla ledger mark review-clean            # when a round comes back clean
```

- If you hit the cap without a clean round, do not loop forever and do not quietly proceed: record the final round honestly and surface the unresolved findings to the human.

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

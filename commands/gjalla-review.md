---
description: Review a spec OR a code change against master spec, rules, and production readiness
allowed-tools: Bash(git:*), Read, Grep, Glob
argument-hint: "[optional: path to a spec/design doc to review]"
---

Review against three lenses that matter at merge time:
**(1) does the thing being reviewed respect the master spec, (2) does it comply
with the project's rules, (3) is it operationally ready to ship.** This is a
compliance review, not a blast-radius analysis. For the forward-looking "what
does this change in the spec" view, use `/gjalla-impact`.

This command handles two input modes — detect first, then branch:

- **Spec mode** — reviewing a draft design/spec doc *before* code is written.
  Triggered if the user passed a file path ($1) that's a markdown spec, or if
  there are no code changes and the conversation references a spec.
- **Code mode** — reviewing actual code changes *before* merge. Triggered if
  there's a non-empty `git diff` or the user explicitly asks for a code review.

If ambiguous, ask the user which mode they want.

---

### Step 1 — Identify the input

- **Spec mode:** Read the spec file ($1, or the spec the user references). Note
  the spec's stated goals, non-goals, behavioral requirements, and proposed
  technical approach. If the spec follows the gjalla `.gjalla/changes/{slug}/`
  convention, read all files in that folder.
- **Code mode:** Run `git diff HEAD` for uncommitted work, or `git diff main...HEAD`
  for branch review. Identify all changed files. If there are no changes, tell
  the user and stop.

### Step 2 — Load the master spec
Use `get_architecture_spec`. Identify the architecture elements (containers,
services, components) the input touches. Files in a diff map to elements via
the spec's evidence patterns. For a spec doc, the spec usually names the
elements it intends to change. Drill in with
`get_architecture_element_details(elementId="...")` when boundaries or
contracts are non-obvious.

Note any documented contracts on those elements (APIs, data shapes,
invariants, capability acceptance criteria).

### Step 3 — Load the rules
Use `get_project_rules`. Pull all active ADRs, principles, and invariants.

### Step 4 — Load the production-readiness checklist
Use `get_guidance(intent="production_readiness")` and
`get_guidance(intent="production_checklist")`. This is your ops lens:
observability, error handling, retries/timeouts, idempotency, security
boundaries, runbook updates, rollback strategy, test coverage, performance.

### Step 5 — Conduct the review on three dimensions

**Spec compliance — does the input respect the documented system?**
- **In spec mode:** Is the proposal grounded in the architecture vocabulary?
  Does it name real elements? Does it preserve component boundaries and
  ownership? Does it correctly enumerate the affected capabilities and
  data flows? Are non-goals explicit and consistent with the architecture?
- **In code mode:** Does the diff preserve component boundaries? Does it
  break or evolve a documented capability's acceptance criteria? Does it
  change a data flow or contract that downstream consumers rely on? Are
  new patterns consistent with the spec's existing design language?

**Rules compliance — does the input honor standing decisions?**
- List each affected rule by name. For each: **COMPLIES / VIOLATES / N/A**.
- For violations, quote the rule and:
  - **In spec mode:** point at the spec section/paragraph at fault.
  - **In code mode:** point at the specific line(s) in the diff at fault.
- For gray areas, frame the question that needs answering.

**Production readiness — is it shippable / mergeable?**
- **In spec mode:** Does the spec address observability, error handling,
  rollback, runbooks, and test strategy? Or does it defer those to "later"
  in a way that's likely to mean "never"?
- **In code mode:** Are observability, error handling, and tests actually
  in the diff? Flag missing affordances on new code paths. Flag operational
  drift (runbooks, dashboards, alerts that should be updated alongside the
  change).

### Step 6 — Present findings grouped by severity

- **BLOCKING** — Rule violations, broken contracts, unsafe-to-ship gaps.
- **WARNING** — Spec drift, missing ops affordances, weakly-tested branches,
  unaddressed risks.
- **NOTE** — Consistency suggestions, alternative approaches, future cleanup.

### Step 7 — Verdict

End with one of: **APPROVE / REQUEST CHANGES / NEEDS DISCUSSION**.

In one sentence, name the single most important reason for the verdict.

---

If gjalla MCP tools are not available, fall back to manual review by reading
any architecture docs, ADR folders, runbooks, or DEFINITION_OF_DONE files in
the repo. Flag explicitly that you reviewed without live spec context.

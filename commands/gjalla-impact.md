---
description: Master-spec delta — what changes in the system when this merges
allowed-tools: Bash(git:*), Read, Grep, Glob
---

Describe the **master-spec delta** for the current changes: what will the
project's architecture, capabilities, data flows, and rules look like AFTER
this PR merges, expressed as additions, modifications, and removals on the
existing spec. This is forward-looking — "here is the new shape of the system" —
not a blast-radius report. For compliance review against the current spec,
use `/gjalla-review`.

1. **Get the diff.**
   - `git diff HEAD` for uncommitted work; otherwise `git diff main...HEAD`.
   - If there are no changes, tell the user and stop.

2. **Load the current master spec** in four passes — these are the four
   axes the delta should be expressed in.

   a. **Architecture** — `get_architecture_spec`. Containers, services,
      components, and relationships. Identify which existing elements the
      diff touches and whether any new elements are being introduced. Drill
      into specific elements with `get_architecture_element_details` when
      the diff suggests a non-trivial change to behavior or boundary.

   b. **Capabilities** — `get_project_capabilities`. The user-observable
      behaviors and their acceptance criteria. Identify which capabilities
      this change adds, modifies, or removes. A capability change means a
      contract change.

   c. **Data flows** — `get_data_flows`. The traces showing how data moves
      across the system. Identify which flows are added, rerouted, or
      retired. A new external integration or a changed storage destination
      is always a data-flow delta.

   d. **Rules** — `get_project_rules`. The standing principles, ADRs, and
      invariants. Identify whether this change implies a new rule, an
      amendment to an existing rule, or the retirement of one.

3. **Produce the master-spec delta** in the following structure. Be precise
   — name the specific element / capability / flow / rule by its spec
   identifier. Use the spec's vocabulary, not the diff's.

   **Architecture delta**
   - **Added elements:** [name, type (container/service/component),
     responsibility, parent in the spec hierarchy]
   - **Modified elements:** [name → what about the element changes:
     responsibility, dependencies, ownership boundary, public surface]
   - **Retired elements:** [name → reason for removal]
   - **Changed relationships:** [from → to: new / removed / kind change]

   **Capability delta**
   - **Added capabilities:** [name, category, acceptance criteria, which
     architecture element implements it]
   - **Modified capabilities:** [name → what about the acceptance criteria
     or implementation changes]
   - **Retired capabilities:** [name → reason]

   **Data flow delta**
   - **Added flows:** [from → to, payload shape, trigger]
   - **Modified flows:** [name → what about source / destination / payload
     / trigger changes]
   - **Retired flows:** [name → reason]

   **Rule delta**
   - **Implied new rules:** [proposed rule text, scope, evidence pattern]
   - **Rule amendments:** [existing rule → proposed change, reason]
   - **Retired rules:** [rule → reason it no longer applies]

4. **Confidence + open questions.**
   - For each delta entry, mark confidence: HIGH (clearly implied by the
     diff), MEDIUM (likely but worth confirming), LOW (might be implied,
     ask the author).
   - List any open questions the author should answer before this delta is
     locked in.

5. **End with a one-paragraph "spec after merge" summary.** Plain-English
   description of the system's documented shape post-merge. This is what an
   engineer reading the spec after the PR should expect to see.

If gjalla MCP tools are not available, produce the best-effort delta from
the repo's architecture docs, capability docs, and ADRs. Flag explicitly
that the spec delta is inferred from local docs rather than the live spec.

---
description: Create a commit attestation for your changes
allowed-tools: Bash(git:*, gjalla:*), Read, Grep
---

Create an attestation documenting what changed, why, and which rules were checked.
Attestations create an accountability trail for every commit — especially important
when AI agents are writing code.

1. Check what has changed: `git diff --stat HEAD`
   - If no changes, tell the user there's nothing to attest and stop.

2. Load the master spec using `get_architecture_spec` MCP tool.
   - Identify which architecture elements are affected by the changes.
   - Drill in with `get_architecture_element_details(elementId="...")` when an
     element's behavior or boundary is materially changing.

3. Load rules using `get_project_rules` MCP tool.
   - Identify which rules are scoped to the affected elements.

4. Draft the attestation using the gjalla CLI: `gjalla attest`.
   - This is a two-phase process: first draft, then finalize.
   - The draft covers all eight primitives: architecture, data model, data flows,
     rules, capabilities, API surface, external dependencies, tech stack.
   - (Write APIs route through the CLI; the MCP server is read-mostly today.)

5. For each primitive, assess: did this change affect it?
   - If yes, document what changed and why.
   - If no, mark as `changed: false`.

6. Check rule compliance:
   - For each applicable rule, report: COMPLIANT / NOT APPLICABLE / NEEDS REVIEW.
   - If any rule is violated, flag it explicitly with the reason.

7. Write a summary that explains the *why*, not just the *what*.
   - Bad: "Added rate limiting"
   - Good: "Token-bucket rate limiting on auth endpoints protects against credential-stuffing;
     chose gateway-level enforcement over per-service to avoid duplicating config."

8. Present the attestation to the user for review before finalizing.

9. After user approval, finalize using `prepare_attestation` (phase 2).

If gjalla MCP tools are not available, create a manual attestation by:
- Reading the git diff
- Documenting changes across the eight primitives
- Writing the attestation to `.gjalla/attestations/` in the repo

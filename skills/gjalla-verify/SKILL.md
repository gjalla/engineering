---
name: gjalla-verify
description: Verify an implementation fully and correctly satisfies its spec — a completion review covering requirements, rules, architecture alignment, and test coverage. Use after finishing implementation, before calling it done.
---

# Completion Verification

Verify the implementation against the spec, the project's rules, and the system it lives in.

> This is a **completion review** — did we build everything the spec asked for, correctly and tested? For a line-level quality and security review of the diff itself, use **gjalla-code-review**.

## Checklist
1. **Spec Compliance**: For each behavioral requirement in the spec, confirm it is implemented and tested.
2. **Rule Compliance**: Check the change against your project's rules/conventions and linters. (If you use gjalla, `gjalla rules list` enumerates active rules.)
3. **Architecture Alignment**: New components, interactions, and boundaries match the spec's technical approach.
4. **Test Coverage**: Every acceptance criterion from the spec has a corresponding test.
5. **Deviation Report**: List any deviations from the spec with justification.

## Verification Process
- [ ] All spec requirements mapped to code locations
- [ ] No orphaned code (code not covered by any requirement)
- [ ] Error paths tested
- [ ] Edge cases from spec handled
- [ ] No TODO/FIXME items left unaddressed
- [ ] Performance implications reviewed
- [ ] Security implications reviewed

## Output
Produce a verification report:
- **Confirmed**: Items that pass verification
- **Deviations**: Items that differ from spec (with reason)
- **Gaps**: Items missing from implementation
- **Rule Violations**: Any rule non-compliance found

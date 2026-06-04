---
name: gjalla-spec
description: Create a feature specification with problem statement, goals, technical approach, and testing strategy. Use before implementing any non-trivial feature.
---

# Feature Specification

Create a structured specification before implementation:

## Template
1. **Problem Statement**: What user or system problem does this solve?
2. **Goals**: What must be true when this is done?
3. **Non-Goals**: What is explicitly out of scope?
4. **Behavioral Requirements**: Observable behaviors the feature must exhibit.
5. **Technical Approach**: How will this be built? Which components, services, and data entities are involved?
6. **Data Model Changes**: New entities, attributes, or relationships.
7. **API Changes**: New or modified endpoints, request/response shapes.
8. **Testing Strategy**: Unit, integration, and acceptance test plan.
9. **Dependencies**: What must exist or be deployed first?
10. **Rollback Plan**: How to safely revert if something goes wrong.

## Guidelines
- Reference existing components by their established names so the design grounds in the real system.
- Cross-reference rules or constraints that bound the design.
- Keep specs under 500 lines; split complex features into sub-specs.

> If you use gjalla, fetch current architecture and rules first (`gjalla context show`, `gjalla rules list`) so the spec aligns with the real system state.

---
description: Load architecture context for this repo
allowed-tools: Read, Grep, Glob
argument-hint: "[scope: architecture|rules|capabilities|tech_stack|data_flows|state]"
---

Load and present architecture context for this project. Use this to orient in an
unfamiliar codebase, understand system structure before making changes, or explore
specific aspects of the architecture.

1. Map the requested scope to the right MCP tool:

   | Scope (optional $1)  | MCP tool                                            |
   |----------------------|-----------------------------------------------------|
   | `architecture`       | `get_architecture_spec`                             |
   | `rules`              | `get_project_rules`                                 |
   | `capabilities`       | `get_project_capabilities`                          |
   | `data_flows`         | `get_data_flows`                                    |
   | `tech_stack`         | `get_tech_stack_overview`                           |
   | `state` / no scope   | `get_project_state(metadata_only=true)` for overview |

2. If no scope was provided, lead with `get_project_state(metadata_only=true)` for a
   high-level overview, then suggest which sub-scope to drill into based on what
   the user is likely doing.

3. Present the context in a clear, navigable format:

   **For architecture scope:**
   - Containers, services, components, and their responsibilities
   - Relationships between elements
   - Key design decisions surfaced in the spec

   **For rules scope:**
   - Active ADRs, principles, and invariants
   - Which elements each rule is scoped to
   - Evidence patterns (how rules can be verified)

   **For capabilities scope:**
   - Documented capabilities by category and status
   - Acceptance criteria
   - Which architecture elements implement each capability

   **For tech_stack scope:**
   - Languages, frameworks, and libraries
   - Infrastructure and deployment targets
   - External dependencies

   **For data_flows scope:**
   - How data moves through the system
   - Integration points
   - Data transformation and storage patterns

4. If the user asks about a specific element or wants to drill in, use:
   - `get_architecture_element_details(elementId="...")` for element-level detail
   - `get_project_state(path="rules.adrs")` style drill paths for sub-trees

5. End with suggested next actions:
   - "To review your current changes against this architecture: `/gjalla-review`"
   - "To project the master-spec delta of your changes: `/gjalla-impact`"

If gjalla MCP tools are not available, orient by:
- Reading README, ARCHITECTURE.md, and docs/ in the repo
- Scanning the directory structure for service boundaries
- Checking package.json / pyproject.toml for dependencies

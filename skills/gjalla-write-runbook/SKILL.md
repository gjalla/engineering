---
name: gjalla-write-runbook
description: Write operational procedures for a service or feature using its topology and external dependencies. Use when documenting ops procedures for something you operate.
---

# Write Runbook

Write operational procedures for a service or feature:

## Process
1. **Understand the topology**: Identify the service's components, its external dependencies, and how traffic and data flow through it. (If you use gjalla, `gjalla state show -c architecture` and `gjalla state show -c services` surface this.)
2. **Identify operational scenarios**: What can go wrong? What requires manual intervention?

## Runbook Template
For each procedure:
- **Title**: Clear action-oriented name (e.g., "Restart Payment Service")
- **When to use**: Conditions that trigger this procedure.
- **Prerequisites**: Access, tools, permissions needed.
- **Steps**: Numbered, copy-pasteable commands where possible.
- **Verification**: How to confirm the procedure succeeded.
- **Escalation**: Who to contact if the procedure fails.

## Common Procedures to Document
- Service restart / failover
- Database connection pool exhaustion
- External service outage response
- Cache invalidation
- Log investigation for common errors
- Scaling up/down procedures

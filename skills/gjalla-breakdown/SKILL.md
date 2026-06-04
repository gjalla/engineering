---
name: gjalla-breakdown
description: Break a feature spec into sized tasks grouped by dependency. Use after a spec is written to plan implementation.
---

# Wave-Based Task Breakdown

Break the feature into sized, dependency-ordered task waves:

## Process
1. **Read the spec**: Identify all behavioral requirements and technical changes.
2. **Identify tasks**: Each task should touch 1-2 files and produce a small, reviewable diff.
3. **Map dependencies**: Which tasks must complete before others can start?
4. **Group into waves**: Tasks within a wave can be done in parallel; waves are sequential.

## Task Format
For each task:
- **ID**: W1-T1, W1-T2, W2-T1, etc.
- **Description**: One-line summary of what changes.
- **Files**: Exact file paths that will be modified or created.
- **Depends On**: Task IDs this depends on (within same wave = none).
- **Acceptance**: How to verify this task is done.

## Wave Rules
- Wave 1: Schema/model changes, migrations
- Wave 2: Storage/service layer
- Wave 3: API layer, validation
- Wave 4: Tests
- Wave 5: Documentation, cleanup

Keep the total task count under 20 for a single spec; split larger features into multiple specs.

---
name: gjalla-debug
description: Find the root cause of a bug by reproducing, isolating, and tracing it back to the original trigger — not just patching the symptom. Use when investigating a defect or unexpected behavior.
---

# Systematic Debugging

Resist the urge to guess-and-patch. Find the true root cause first.

## 1. Reproduce
- Get a reliable, minimal reproduction. A bug you can't reproduce, you can't confirm you've fixed.
- Pin down exact inputs, environment, and steps. Note what's intermittent vs deterministic.
- If you can't reproduce it, gather evidence (logs, traces, stack traces, recent changes) until you can form a testable hypothesis.

## 2. Localize
- Read the full error and stack trace — start at the deepest frame in *your* code, not the top.
- Bisect the problem space: confirm where state is still correct and where it first goes wrong. Binary-search the pipeline, the inputs, or the commits (`git bisect`).
- Trace backward from the symptom to the original trigger. The line that throws is rarely the line that's wrong.

## 3. Form and test hypotheses
- State a specific, falsifiable hypothesis: "X is wrong because Y."
- Change ONE thing at a time. Add a targeted log, assertion, or breakpoint to confirm or kill it.
- Let evidence, not intuition, advance you. A failed hypothesis is progress — it removes a suspect.

## 4. Find root cause
- Keep asking "why" until you reach a cause that, if fixed, prevents this whole class of bug — not just this instance.
- Distinguish the trigger (what set it off) from the root cause (why the code allowed it).

## 5. Fix and verify
- Write a failing test that reproduces the bug FIRST, then fix until it passes. This proves the fix and prevents regression.
- Confirm the original reproduction is gone and you haven't broken adjacent behavior.
- Ask where else this same root cause exists, and fix the class — not just the case.

## Anti-patterns
- Patching the symptom (clamping a value, swallowing an error) without understanding why it occurred.
- Changing several things at once so you can't tell what actually fixed it.
- Declaring victory because the error "went away" — confirm *why* it went away.

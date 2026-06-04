# gjalla/engineering

Opinionated agent skills for **trustworthy, production-grade engineering** — spec-driven planning, rigorous review, honest tests, and clean operations. Built from the practices we use to ship verifiable code with agents.

## Install

```bash
# all skills
npx skills add gjalla/engineering

# browse and pick
npx skills add gjalla/engineering --list
```

Each skill is a `SKILL.md` your coding agent loads on demand. Works with Claude Code, Cursor, Codex, and the other agents supported by [`npx skills`](https://github.com/vercel-labs/skills).

## Skills

### Plan
- **gjalla-spec** — write a feature spec (problem, goals, approach, test strategy) before building
- **gjalla-spec-review** — review a spec across architecture, security, quality, user-impact, and governance lenses
- **gjalla-breakdown** — decompose a spec into dependency-ordered task waves

### Review & verify
- **gjalla-code-review** — review a diff/PR for correctness, security, and quality before merge
- **gjalla-verify** — verify an implementation fully satisfies its spec (completion review)
- **gjalla-test-audit** — find tests that give false confidence before they cost you a regression
- **gjalla-debug** — systematic root-cause debugging instead of symptom patching

### Ship & operate
- **gjalla-prepare-commit** — shape clean, atomic, well-described commits and pass pre-commit checks
- **gjalla-write-runbook** — document operational procedures for a service or feature
- **gjalla-conduct-post-mortem** — blameless incident analysis with 5 Whys and action items

Every skill works standalone. A few note optional `gjalla` CLI commands that enrich them with live architecture and rules context — those are enhancements, not requirements.

---

Built by **gjalla** — the architectural memory layer for agentic engineering.

# gjalla/engineering

Opinionated and time-tested agent skills for **trustworthy, production-grade engineering** — spec-driven planning, rigorous review, test auditing to bust false-confidence, and clean operations. Built with engineering best practices in mind, adapted for use by teams shipping with coding agents. Welcome to agentic SDLC!

## Install

```bash
# all skills
npx skills add gjalla/engineering

# browse and pick
npx skills add gjalla/engineering --list
```

Then tell your agent **"set up gjalla"**. The `gjalla-onboard` skill installs the CLI, walks you through one browser sign-in, backfills your last week of agent sessions, and reports what your coding-agent spend bought by task type. Alternatively start from the CLI with `uvx gjalla setup`, which installs these skills for you.

Each skill is a `SKILL.md` your coding agent loads on demand. Works with Claude Code, Cursor, Codex, and the other agents supported by [`npx skills`](https://github.com/vercel-labs/skills).

Every skill works standalone. A few note optional `gjalla` CLI commands that enrich them with live architecture and rules context — those are enhancements, not requirements.

---

Built by **gjalla** — the infrastructure for trustworthy code when building with coding agents.

---
name: gjalla-prepare-commit
description: Shape staged changes into clean, atomic, well-described commits and run pre-commit checks before pushing. Use right before committing or opening a PR.
---

# Prepare Commit

Turn a working change into a commit history that reviewers and future readers can trust.

## 1. Review what you're about to commit
- Read the full staged diff (`git diff --staged`). Commit what you intend — nothing more.
- Remove debug logging, commented-out code, stray files, and unrelated changes.
- Never stage secrets, credentials, tokens, or `.env` values. Confirm none appear in the diff (including inside URLs).

## 2. Make commits atomic
- One logical change per commit. If the diff does two unrelated things, split it (`git add -p`).
- Each commit should build and pass tests on its own where practical.
- Keep refactors separate from behavior changes — mixing them hides the real change in noise.

## 3. Verify before committing
- Run the relevant tests and the linter/formatter; fix failures rather than committing red.
- Confirm the change does what you set out to do (see gjalla-verify for spec completion, gjalla-code-review for a quality pass).

## 4. Write the message
- Subject: imperative and scoped — "Fix race in token refresh", not "fixes stuff".
- Body (when non-trivial): WHY the change exists and any consequence or trade-off a reader needs. The diff already shows the what.
- Reference the issue or spec it satisfies.

## 5. Final checks
- Re-read the diff one last time as if reviewing someone else's work.
- Ensure project conventions and rules are met. (If you use gjalla, re-check `gjalla rules` and attest your change before pushing.)

## Principles
- A commit is a unit of understanding, not a save point. Optimize for the person who reads it in six months.
- If you can't describe the commit in one clear subject line, it's probably doing too much — split it.

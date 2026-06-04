---
name: gjalla-prepare-and-commit
description: Make sure your code is ready to commit, then stage changes into clean, atomic commit. Use before committing.
---

# Make your changes commit-ready

Turn a working change into a committable change that reviewers and future readers can trust.

## 1. Review what you implemented
- Are all verification criteria and expected outcomes met?
- Is the implementation complete (no dummy data or placeholder stubs left)?
- Are all of the gjalla rules met?
- Does this elegantly build on top of / within the existing state/architecture/properties?
- Does it match the plan / gjalla spec?
- Are positive, negative, and edge cases saliently tested?

## 2. Make a commit plan
- One logical change per commit. If the diff does two unrelated things, don't be afraid to split it (`git add -p`).
- Ensure you're committing to the right branch and/or following the expected branching strategy.
- Prepare what you will report to git (description of the lines of code that are changing) and to gjalla (via attestation - properties of the system that have changed, including a reference to the gjalla spec)
- Ensure gjalla git hooks are in place already. If they're not, your attestation and spec will not get sync'd into the master source of truth.

## 3. Commit
- Write your gjalla attestation. `gjalla attest --example` will show you the format, make sure to view the full output (do not run the command and pipe it 2>/dev/null for example)
- Write your commit message and run your commit. Your gjalla attestatation and spec will be sync'd with gjalla as part of the git hooks, no additional work required from you.
- Unless the user has given you blanket permission, you should likely ask the user before pushing the commit.

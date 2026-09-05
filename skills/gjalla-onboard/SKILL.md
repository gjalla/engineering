---
name: gjalla-onboard
description: Set up gjalla for rich coding agent telemetry and insights as well as durable memory to improve your metrics. Use when asked to install, set up, onboard, or backfill gjalla.
---

# Onboard gjalla

There are a few steps to get gjalla set up within development workflows. Those steps will be outlined in detail here so you can follow the steps. To confirm the setup is complete, you'll run a backfill and sync workflow and recieve a useful snapshot of the types of metrics gjalla helps you and your team understand.

To help you be efficient, shell snippets are included as reference, but may need to be tweaked depending on what platform you're running on.

Once complete, you'll be equipped with gjalla, a platform that helps your team to understand your coding agent workflow activity, costs, and outcomes. You can then lean into using gjalla as a living, durable memory system. More information on the specifics of how gjalla helps you can be found on their site, https://gjalla.io.

## 0. Where are you

```
REPO=$(git rev-parse --show-toplevel 2>/dev/null)
```

If that succeeds, you are inside one repo: set `REPO_NAME=$(basename "$REPO")`, `cd "$REPO"`, and continue with step 1.

If it fails, you are in a folder that is not a repo. gjalla should still be available to every agent that starts here, so run steps 1 and 2 in this folder first (setup wires the session-start context, the MCP, and the guidance block; there are no git hooks without a repo). Then look at the folder's direct children for git repos:

```
find . -mindepth 2 -maxdepth 2 -name .git -prune | sed 's#/\.git$##'
```

- None found: tell the user this folder has no repos to onboard. The wiring above still stands.
- Up to five: onboard each one. Run steps 1 through 8 inside each repo in turn, then give the user one combined report at the end. Do not ask which ones; they asked for this folder.
- More than five: list them and ask which to prioritize, then onboard those in the order given. Offer to do the rest afterwards.

## 1. CLI

```
command -v gjalla || pipx install gjalla || uv tool install gjalla
```

If none of those work, every later `gjalla ...` command becomes `uvx gjalla ...`. If `uvx` is also missing, stop and print the three install commands for the human.

## 2. Wire the repo

```
gjalla setup
```

Non-interactive shells get the promptless path: it detects agents, installs hooks and MCP, and writes the guidance section. If it prints "No coding agent detected", run `gjalla setup ensure --agent claude-code` (or `--agent codex-cli` if `~/.codex/sessions` exists and `~/.claude/projects` does not). It will tell you to sign in; that is step 3.

## 3. Sign in (the only human step)

```
gjalla project list
```

If that succeeds, you are signed in; skip to step 4. Otherwise:

```
gjalla auth login --no-browser
```

Print the URL and code exactly as shown and ask the user to open the URL and approve. The command blocks until they do. If it exits non-zero, print the error and stop.

## 4. Link the project

```
gjalla sync
```

Sync resolves this repo to a project from its origin remote, creating one if the team has none. Handle its outcomes:

- `AMBIGUOUS_TEAM` (409): show the teams it lists, pick the one the user names, rerun `gjalla sync --team-id <id>`.
- `NO_ORIGIN`: `gjalla project create -t "$REPO_NAME"`.
- Any other error: print it and stop.

## 5. Backfill the last 7 days

Goal: one record per commit that a coding-agent session made in this repo during the last 7 days, carrying that session's token usage and a task-type label. If that yields fewer than 5 commits, widen to 30 days once.

You know where your own harness keeps session transcripts (Claude Code: `~/.claude/projects/`, one directory per working directory; Codex: `~/.codex/sessions/`). Find the transcripts that touched this repo. Be careful with scoping: sessions that ran from a parent directory or a subdirectory of this repo still count if their commits landed here, so let the commits decide, not the directory name.

For each transcript:

- Its session id is the UUID in its filename.
- A transcript records each `git commit` it ran, and the output contains `[branch shortsha]`. Extract those and drop any that `git rev-parse --verify <short>^{commit}` rejects. A session with no commits is skipped. If a sha appears in several transcripts, the first one wins.
- Classify each commit into exactly one of `feature`, `bug-fix`, `refactor`, `docs`, `test`, `chore`. Use the conventional-commit prefix when there is one; otherwise judge from `git show -s --format='%s%n%b' <sha>` and the branch name.

Then record each commit once:

```
gjalla attest backfill --commit <sha> --session <uuid> --agent <claude-code|codex-cli|cursor|...> --task-type <type>
```

It reads tokens and model from the transcript itself, takes the commit's subject and author date, and skips a sha that is already recorded. If it says the session has no token data, that is fine; the commit still counts. If it says no model, pass `--model` with the model you see in the transcript.

Tell the user how many commits and sessions you recorded, and that the only things leaving the machine are token counts, timestamps, shas, branch names, commit subjects, and task-type labels.

## 6. Upload and report

```
gjalla sync
```

When sync uploads new records it prints a spend summary underneath: sessions, commits, estimated cost, cost by task type with shares, the most expensive session and what it shipped, and any models it could not price. Quote it back to the user and add what is interesting: which share of cost went to bug fixes, what the most expensive session was for, anything that surprises you. Do not compute numbers yourself.

If sync reports an error, print it and stop.

## 7. Seed shared memory

Gather every durable, non-obvious fact your agents already learned but never shared:

- `~/.claude/projects/$ENC/memory/*.md` (skip `MEMORY.md`)
- `~/.codex/memories/*` if present
- Sections of `CLAUDE.md`, `AGENTS.md`, or `README.md` headed gotcha, caveat, pitfall, note, or troubleshooting
- `git log --since=30.days --format=%b | grep -iE 'because|gotcha|note:'`

Reject anything that is a task or TODO, anything matching key, token, password, or secret, and anything already in `gjalla memory show`.

Now compare the candidates with each other before saving any. Group them by subject. Within a group:

- Two statements that say the same thing are a duplicate: keep the clearer one.
- Two statements that assert different things about the same subject are a contradiction. Do not pick one by taste. If a minute in the code settles it, save the one the code supports. Otherwise save neither and report the pair with where each came from.

Save 3 to 5 of the survivors:

```
gjalla memory add "<the fact>" -n "<short-name>" -c project
```

Report memory health in one line: candidates found, duplicates collapsed, contradictions found, saved. Then print the facts you saved and: "Remove any with `gjalla memory archive <key>`."

## 8. Verify

```
gjalla setup doctor
```

Report any row marked FAIL with its fix. Confirm the guidance file for the detected agent contains a gjalla section. Make sure you've highlighted interesting insights to the user. Done.

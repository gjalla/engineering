---
name: gjalla-onboard
description: Set up gjalla for rich coding agent telemetry and insights as well as durable memory to improve your metrics. Use when asked to install, set up, onboard, or backfill gjalla.
---

# Onboard gjalla

There are a few steps to get gjalla set up within development workflows. Those steps will be outlined in detail here so you can follow the steps. To confirm the setup is complete, you'll run a backfill and sync workflow and recieve a useful snapshot of the types of metrics gjalla helps you and your team understand.

To help you be efficient, shell snippets are included as reference, but may need to be tweaked depending on what platform you're running on.

Once complete, you'll be equipped with gjalla, a platform that helps your team to understand your coding agent workflow activity, costs, and outcomes. You can then lean into using gjalla as a living, durable memory system. More information on the specifics of how gjalla helps you can be found on their site, https://gjalla.io.

## 0. Preconditions

```
REPO=$(git rev-parse --show-toplevel) || { echo "Not a git repository. Run this inside the repo you want to onboard."; exit 1; }
REPO_NAME=$(basename "$REPO")
cd "$REPO"
```

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

Goal: one attestation record per commit that a coding-agent session made in this repo during the last 7 days, carrying that session's token usage and a task-type label. If that yields fewer than 5 commits, widen to 30 days once.

You know where your own harness keeps session transcripts (Claude Code: `~/.claude/projects/`, one directory per working directory; Codex: `~/.codex/sessions/`). Find the transcripts that touched this repo. Be careful with scoping: sessions that ran from a parent directory or a subdirectory of this repo still count if their commits landed here, so let the commits decide, not the directory name.

Key commands:

- Session token totals, from the CLI, given a transcript's session id (the UUID in its filename):
  ```
  gjalla session context --agent <claude-code|codex-cli> --session <uuid> --cwd "$REPO"
  ```
  Use `facts.tokens` (input, output, cache_read, cache_creation, input_uncached). If `facts` is null, skip that session.
- Model: the transcript records `"model":"..."` on each request; take the most frequent.
- Commits: a transcript records each `git commit` it ran, and the output contains `[branch shortsha]`. Extract those, expand with `git rev-parse --verify <short>^{commit}`, and drop any that no longer exist. A session with no commits is skipped. If a sha appears in several transcripts, the first one wins. Skip any sha that already has `.gjalla/.platform/attestations/<sha>.yaml`.
- Commit facts: `git show -s --format='%aI%n%s%n%b' <sha>` for the author date, subject, and body.

Classify each commit into exactly one of `feature`, `bug-fix`, `refactor`, `docs`, `test`, `chore`. Use the conventional-commit prefix when there is one; otherwise judge from the subject, body, and branch name.

Write `.gjalla/.platform/attestations/<sha>.yaml`:

```yaml
source: backfill
timestamp: "<author date, %aI>"
agent: <claude-code|codex-cli|devin|cursor-cli|opencode|...>
agent_provider: <anthropic|openai|cognition|cursor|...>
agent_model: "<model>"
agent_session_id: "<uuid>"
provenance:
  type: ad-hoc
  ref: backfill
task_type: <one of the six>
summary: "<commit subject>"
telemetry:
  version: 2
  session_id: "<uuid>"
  source: backfill
  agent_runtime: <claude-code|codex-cli|devin|cursor-cli|opencode|...>
  tokens:
    input: <n>
    output: <n>
    cache_read: <n>
    cache_creation: <n>
    input_uncached: <n>
```

Append one line to `.gjalla/.platform/log.jsonl` for each sha not already present in that file. Append only; never rewrite it.

```json
{"commit":"<sha>","branch":"<branch>","timestamp":"<author date>","agent":"<agent>","summary":"<subject>","synced":false,"agent_session_id":"<uuid>","telemetry":{"version":2,"session_id":"<uuid>","source":"backfill","agent_runtime":"<agent>","tokens":{"input":0,"output":0,"cache_read":0,"cache_creation":0,"input_uncached":0}}}
```

Tell the user how many commits and sessions you recorded, and that the only things leaving the machine are token counts, timestamps, shas, branch names, commit subjects, and task-type labels.

## 6. Upload

```
gjalla sync
grep -c '"synced": false' .gjalla/.platform/log.jsonl
```

The count should be 0. If sync fails, print the error and stop.

## 7. Report

```
KEY=${GJALLA_API_KEY:-$(grep -E '^api_key:' ~/.gjalla/config.yaml | awk '{print $2}')}
URL=${GJALLA_API_URL:-$(grep -E '^api_url:' ~/.gjalla/config.yaml | awk '{print $2}')}
URL=${URL:-https://gjalla.io}
curl -s -H "x-api-key: $KEY" "$URL/api/agent/projects/$PROJECT_ID/attestations/stats?days=30"
```

Show the user:
- Spend by task type: commits, sessions, tokens, estimated cost. Say which share of cost went to bug fixes.
- The most expensive session: estimated cost, task types, and the commit subjects it shipped.
- Totals, and how many records were backfilled.
- If `pricing.unpriced_models` is non-empty, say cost is token-only for those models.

Do not compute numbers yourself, but instead your role is to understand and give any interesting insights to the user.

## 8. Seed shared memory

Look for durable, non-obvious facts your agents already learned but never shared:

- `~/.claude/projects/$ENC/memory/*.md` (skip `MEMORY.md`)
- `~/.codex/memories/*` if present
- Sections of `CLAUDE.md`, `AGENTS.md`, or `README.md` headed gotcha, caveat, pitfall, note, or troubleshooting
- `git log --since=30.days --format=%b | grep -iE 'because|gotcha|note:'`

Pick 3 to 5. Reject anything that is a task or TODO, anything matching key, token, password, or secret, and anything already in `gjalla memory show`. Save each:

```
gjalla memory add "<the fact>" -n "<short-name>" -c project
```

Print the facts you saved and: "Remove any with `gjalla memory archive <key>`."

## 9. Verify

```
gjalla setup doctor
```

Report any row marked FAIL with its fix. Confirm the guidance file for the detected agent contains a gjalla section. Make sure you've highlighted interesting insights to the user. Done.

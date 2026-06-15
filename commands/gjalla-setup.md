---
description: Initialize gjalla in this repo
allowed-tools: Bash(gjalla:*), Read, Write
---

Initialize gjalla in the current repository. Run the setup flow to connect this repo to a gjalla project.

1. Check if gjalla CLI is installed: `gjalla --version`
   - If not installed, tell the user: "gjalla CLI is required. Install with: pip install gjalla"
   - Stop and wait for the user to install.

2. Check current status: `gjalla status`
   - If already configured, report the current project connection and cache status.
   - Ask if the user wants to reconfigure or just sync.

3. If not configured, run: `gjalla setup`
   - This walks through project selection/creation and API key configuration.
   - Follow the interactive prompts.

4. After setup, sync the local cache: `gjalla sync`

5. If the project was just created via CLI (not connected via GitHub App), run `gjalla state bootstrap`
   to generate initial architecture context from the local codebase. This analyzes the repo and creates
   gjallastate/gjallamap/gjallarules locally so MCP tools return architecture data immediately.
   For GitHub App-connected projects, cloud analysis triggers automatically — bootstrap is not needed.

6. Verify everything works by querying the live MCP: call `get_project_state(metadata_only=true)`
   to confirm the connection and show project size.

7. Report to the user:
   - Project name and ID
   - Number of architecture elements, rules, and capabilities loaded
   - Available MCP tools
   - Suggest next steps: "Try /gjalla-context to explore your architecture, or /gjalla-review to review your current changes."

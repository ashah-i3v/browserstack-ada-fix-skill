# Bitbucket MCP -- Setup, Auto-Configure & Start

Fully automated procedure for ensuring the Bitbucket MCP server is
configured, running, and reachable before the GitFlow phase begins.

**Guiding principle:** Fix it automatically. Only ask the user when
credentials are genuinely missing or all recovery attempts fail.

---

## Step 1: Locate existing configuration

Check both locations in order. Use whichever is found first:

1. **Project-level:** `<project-root>/.vscode/mcp.json`
2. **User-level:** VS Code user settings (`settings.json`) under `"mcp.servers"`

Look for any key matching `bitbucket` (case-insensitive) inside
the `servers` object.

- **Found:** Jump to [Step 3: Verify server is responding](#step-3-verify-server-is-responding).
- **Not found:** Continue to Step 2.

---

## Step 2: Auto-configure (server not configured)

Log: `[Setup] Bitbucket MCP is not configured. Setting it up now...`

### 2a -- Check Python / uvx availability

The recommended Bitbucket MCP package (`bitbucket-mcp-py`) runs via
`uvx`. Verify the toolchain:

```
uvx --version
```

| Result | Action |
|--------|--------|
| `uvx` found | Log: `[Setup] uvx is available.` Proceed to 2b. |
| `uvx` not found | Try installing it: run `pip install uv` (or `pip3 install uv`). If pip is also missing, try `python -m ensurepip && pip install uv`. |

After install attempt, re-check `uvx --version`.

- Success: Log: `[Setup] Installed uv/uvx successfully.`
- Still missing: Log: `[Setup] Could not install uvx. Please install it manually: pip install uv`
  **Stop** and wait for user, then re-check.

### 2b -- Collect credentials

Check if the required values already exist as environment variables:

| Variable | Shell command (PowerShell) | Shell command (bash) |
|----------|---------------------------|----------------------|
| `BITBUCKET_USERNAME` | `echo $env:BITBUCKET_USERNAME` | `echo $BITBUCKET_USERNAME` |
| `BITBUCKET_TOKEN` | `echo $env:BITBUCKET_TOKEN` | `echo $BITBUCKET_TOKEN` |
| `BITBUCKET_WORKSPACE` | `echo $env:BITBUCKET_WORKSPACE` | `echo $BITBUCKET_WORKSPACE` |

- **All three set in environment:**
  Log: `[Setup] Found all Bitbucket credentials in environment variables. Proceeding automatically.`
  Use `${env:BITBUCKET_USERNAME}`, `${env:BITBUCKET_TOKEN}`, `${env:BITBUCKET_WORKSPACE}` as the values in the config (do **not** embed the literal values). Skip prompting the user entirely and continue directly to 2c.

- **Partially set or none set:**
  Log only the *missing* variable names (never log values):

  ```
  [Setup] Bitbucket MCP requires the following credentials (missing values marked):
    - BITBUCKET_USERNAME  (your Atlassian account email)       [missing]
    - BITBUCKET_TOKEN     (Bitbucket App Password / API token) [missing]
    - BITBUCKET_WORKSPACE (workspace slug, e.g. "myteam")      [set]
  Please provide the missing values.
  ```

  **Stop** and wait for user input before continuing.

### 2c -- Write configuration to mcp.json

Read the target `.vscode/mcp.json` (project-level first, then user-level settings.json).
If neither file exists, create project-level `.vscode/mcp.json`.

**Merge rule:** Only add or update the `bitbucket-mcp` key inside
`servers`. Preserve every other existing server entry unchanged.

When credentials came from environment variables (Step 2b), write `${env:…}` references so credentials are never hardcoded in the file:

```json
{
  "servers": {
    "bitbucket-mcp": {
      "command": "uvx",
      "args": ["--from", "bitbucket-mcp-py", "bitbucket-mcp"],
      "env": {
        "BITBUCKET_USERNAME": "${env:BITBUCKET_USERNAME}",
        "BITBUCKET_TOKEN": "${env:BITBUCKET_TOKEN}",
        "BITBUCKET_WORKSPACE": "${env:BITBUCKET_WORKSPACE}"
      }
    }
  }
}
```

When credentials were supplied directly by the user (not from env vars), embed the provided values literally.

Log: `[Setup] Bitbucket MCP configuration written to <path-to-mcp.json>.`

### 2d -- Pre-warm the package

Run the package once to ensure it downloads and caches:

```
uvx --from bitbucket-mcp-py bitbucket-mcp --help
```

- Success: Log: `[Setup] bitbucket-mcp-py package cached and ready.`
- Failure: Log: `[Setup] Package install failed: <error>. Retrying...`
  Retry once. If still failing, log the error and **stop**.

Continue to Step 3.

---

## Step 3: Verify server is responding

Log: `[Setup] Verifying Bitbucket MCP server is reachable...`

Call a lightweight Bitbucket MCP tool to confirm the server is alive.
Try in this order (stop at first success):

1. `list_repositories` with `page_size: 1` (bitbucket-mcp-py tools)
2. `pr_list` with any known repo (Node.js bitbucket tools)

### 3a -- Server responds

Log: `[Setup] Bitbucket MCP server is active and responding.`

Done -- proceed with the workflow.

### 3b -- Server does not respond

Log: `[Setup] Bitbucket MCP server is not responding. Attempting auto-recovery...`

Follow this recovery sequence in order. Stop as soon as one succeeds.

#### Recovery 1: Reload MCP servers via VS Code command

Log: `[Setup] Asking VS Code to reload MCP servers...`

Instruct the user:

```
[Setup] Please reload MCP servers:
  Ctrl+Shift+P -> "MCP: List Servers" -> restart the bitbucket server
  (or: Ctrl+Shift+P -> "Developer: Reload Window")
```

**Wait** for user confirmation, then re-test (repeat Step 3 verification).

- Success: Log: `[Setup] Bitbucket MCP server recovered after reload.`
- Failure: Continue to Recovery 2.

#### Recovery 2: Re-validate configuration

Log: `[Setup] Checking configuration for errors...`

Re-read `.vscode/mcp.json` and validate:

| Check | Fix |
|-------|-----|
| `command` is not `uvx` | Correct it to `"uvx"` |
| `args` are wrong | Set to `["--from", "bitbucket-mcp-py", "bitbucket-mcp"]` |
| `env` keys misspelled | Fix key names |
| `env` values empty / placeholder | Prompt user for real values |

If any corrections were made:
Log: `[Setup] Fixed configuration errors in mcp.json. Please reload VS Code.`
**Wait** for reload, then re-test.

- Success: Log: `[Setup] Bitbucket MCP server recovered after config fix.`
- Failure: Continue to Recovery 3.

#### Recovery 3: Reinstall package

Log: `[Setup] Reinstalling bitbucket-mcp-py...`

```
uvx --reinstall --from bitbucket-mcp-py bitbucket-mcp --help
```

Then ask user to reload VS Code. Re-test.

- Success: Log: `[Setup] Bitbucket MCP server recovered after reinstall.`
- Failure: Continue to final fallback.

#### Final fallback

Log:

```
[Setup] All auto-recovery attempts failed.
  Bitbucket MCP is not available. The GitFlow phase will still:
  - Create the branch
  - Commit and push changes
  But the pull request must be created manually in Bitbucket.
```

Mark Bitbucket MCP as unavailable and let the GitFlow phase handle
graceful degradation (branch + push still work via git CLI).

---

## Security rules

- **Never** hardcode or log real credentials in chat output.
- **Never** log token or password values -- only confirm they are set.
- If the API returns 401/403, ask the user to regenerate their
  App Password with the required scopes:
  `repository:read`, `repository:write`, `pullrequest:read`,
  `pullrequest:write`.

---

## Available Bitbucket MCP tools

Once the server is running, the following tools are available for the
GitFlow phase. Prefer whichever set is loaded by the active server.

### bitbucket-mcp (Python -- `bitbucket-mcp-py`)

| Tool | Purpose |
|------|---------|
| `create_pull_request` | Create a new PR |
| `get_pull_request` | Get PR details |
| `get_pull_requests` | List PRs in a repo |
| `list_repositories` | List repos in workspace |
| `get_pull_request_diff` | Get PR diff |
| `merge_pull_request` | Merge a PR |

### bitbucket (Node.js -- custom server)

| Tool | Purpose |
|------|---------|
| `pr_create` | Create a new PR |
| `pr_get` | Get PR details |
| `pr_list` | List PRs |
| `pr_merge` | Merge a PR |
| `pr_comment_add` | Add PR comment |
| `branch_create` | Create a branch |
| `commit_files` | Commit files via API |

### Tool selection logic

1. Try `create_pull_request` first (bitbucket-mcp-py).
2. If unavailable, fall back to `pr_create` (Node.js server).
3. If neither responds, log the failure and tell the user to create
   the PR manually (the branch is already pushed).

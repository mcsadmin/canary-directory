# Canary Preflight Master Prompt — Improvement Notes

**Date:** 13 March 2026
**Author:** Dil (captured by Canary Preflight Agent)
**Purpose:** Lessons learned from the first preflight run, to be incorporated into the next version of the prompt.

---

## Issue 1 — GitHub CLI Not Installed

The prompt assumes `gh` is available. On a fresh Mac it is not. The prompt should open with an explicit check:

```bash
gh --version
```

If this returns "command not found", the user should be directed to install it before proceeding:

```bash
brew install gh
```

The `brew install gh` command should be the first instruction in Area 1, not assumed.

---

## Issue 2 — Gitbook Repository Not Visible in Dropdown

After granting the Gitbook GitHub app access to a newly created repository, the repository may not appear in the Gitbook space configuration dropdown. The prompt should warn the user of this and provide three remedies in order:

1. **Use the search box** within the dropdown — type the repository name directly. This is the most reliable fix.
2. **Click "Load more..."** in the dropdown — this sometimes surfaces repositories not shown in the initial list, but is unreliable.
3. **Hard refresh the page** (`Cmd + Shift + R`) — forces Gitbook to re-fetch the repository list from GitHub.

The search box approach (option 1) is the most reliable and should be tried first.

---

## Issue 3 — Railway Custom Domain Cannot Be Configured Before First Deployment

The prompt instructs the user to configure the custom domain (`canary.commoner.services`) as part of the Railway application service setup. This is not possible until Railway has made at least one deployment attempt, because the "Generate Domain" button must be clicked first, and Railway will not generate a domain for a service that has never deployed.

The prompt should be updated to:

1. Instruct the user to set the `DATABASE_URL` environment variable on the application service.
2. Explicitly state that the custom domain configuration is a **pending item** to be completed after the first code push.
3. Remove the instruction to configure the custom domain from the preflight sequence entirely, and instead add it as a named pending item in the Preflight Report template.

---

## Issue 4 — Cursor Linear MCP Configuration Is Outdated

The Linear MCP server is no longer distributed as a local npm package. It is now a centrally hosted remote server authenticated via OAuth. The `mcp.json` configuration in the prompt must be updated.

**Before running the MCP configuration**, the user must have Node.js installed. The prompt should include a check:

```bash
node --version
```

If this returns "command not found", install it:

```bash
brew install node
```

Then find the full path to `npx` (required because Cursor launches with a restricted PATH that may not include Homebrew binaries):

```bash
which npx
```

The correct `mcp.json` configuration is:

```json
{
  "mcpServers": {
    "linear": {
      "command": "/path/from/which/npx",
      "args": ["-y", "mcp-remote", "https://mcp.linear.app/mcp"]
    }
  }
}
```

Replace `/path/from/which/npx` with the output of `which npx` (typically `/opt/homebrew/bin/npx` on Apple Silicon Macs). No API key is required — authentication is handled via OAuth in the browser when the server first connects.

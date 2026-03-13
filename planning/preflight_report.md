# Canary Preflight Report
**Date:** 13 March 2026
**Completed by:** Dil

## Confirmed Items
| Area | Item | Status | Notes |
|---|---|---|---|
| GitHub | Repository created and initialised | ✓ | |
| GitHub | Folder structure in place | ✓ | |
| GitHub | Briefing documents committed | ✓ | |
| GitHub | SUMMARY.md initialised and in sync | ✓ | |
| Gitbook | Space created and connected | ✓ | URL: https://app.gitbook.com/o/XaMlLD6UZw2ACBWlWinj/s/Z3pvygdUBm01bDL820zl/ |
| Gitbook | Initial sync confirmed | ✓ | |
| Railway | PostgreSQL provisioned | ✓ | |
| Railway | Application service configured | ✓ | DNS: pending |
| Cursor | Linear MCP integration active | ✓ | |
| Cursor | GitHub push confirmed | ✓ | |
| Linear | Canary project created | ✓ | |
| Coaching agent | Task created and briefed | ✓ | |

## Issues Encountered and Resolved
- **Gitbook repository access:** The Gitbook GitHub app initially lacked permission to see the new `canary-directoryesolved by updating the app's repository access settings in GitHub to include `canary-directory`.
- **Cursor Linear MCP setup:** The Linear MCP integration in Cursor failed initially due to two issues: (1 ) Node.js was not installed on the local machine, and (2) the MCP server configuration in `mcp.json` was pointing to an outdated, non-existent npm package. This was resolved by installing Node.js via Homebrew and updating `mcp.json` to use the correct remote MCP server URL (`https://mcp.linear.app/mcp` ) as specified in the latest Linear documentation.

## Pending Items
- **Railway DNS configuration:** The custom domain for the Railway application service (`canary.commoner.services`) cannot be fully configured until the first deployment is made. This is noted as a pending item to be completed during the first canary coding session.
- **Tom's preflight items:** Tom needs to (1) activate his Cursor Pro licence, (2) connect his Cursor instance to the Linear MCP, and (3) accept the invite to the Linear workspace. These are pending his availability but are required before he can participate in coding sessions.

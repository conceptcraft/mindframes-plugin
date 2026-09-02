---
name: connecting-mindframes
description: Connects this plugin to a Mindframes workspace. Use when the plugin was just installed, when a presentation tool reports that authorization is required, or when the user asks to sign in to or switch Mindframes workspaces.
---

# Connecting Mindframes

The plugin talks to one remote MCP server over OAuth. There is nothing to
install locally and no API key to paste.

1. The server is `https://www.mindframes.app/mcp/presentations`, already
   declared in this plugin's `.mcp.json`. Do not ask the user for a URL.
2. On first use the host opens a Mindframes sign-in and consent page. Consent
   asks for exactly two scopes, `presentations:read` and
   `presentations:write`. Nothing else is requested, and this server cannot
   reach videos, brands, agents, or media.
3. A Mindframes account is required. If the user has none, send them to
   https://www.mindframes.app to sign up, then retry.
4. After consent, call `list_workspaces` once and confirm which workspace to
   use when the account has more than one. Pass that `teamId` on every later
   call.

## When something fails

- A tool answering that authorization is required means the connection was
  never completed or was revoked. Re-run the host's connect flow; do not
  retry the tool in a loop.
- `workspace_not_found` means the `teamId` is wrong or the account lost access
  to it. Call `list_workspaces` again rather than guessing an id.
- Report a tool's typed error to the user as a failure. Never present an empty
  result as if the operation succeeded.

Generating a presentation spends the workspace's Mindframes credits. Say so
before the first generation in a session, not after.

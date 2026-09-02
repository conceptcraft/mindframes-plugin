# Mindframes Presentations — Claude & Codex plugin

Create, find, share and inspect [Mindframes](https://www.mindframes.app)
presentations from Claude Code, Cowork and Codex.

## What it does

The plugin connects to one remote MCP server over OAuth and exposes a
deliberately narrow presentation surface:

| Tool | Purpose |
| --- | --- |
| `list_workspaces` | Pick the workspace every other call is scoped to |
| `generate_presentation` | Start a deck; returns a `runId` |
| `get_generation_status` | Poll that run until it reaches a terminal state |
| `list_presentations` | Browse existing decks with stable ordering |
| `get_presentation` | Read one deck, including slide detail |
| `update_visibility` | Share or unshare a deck |
| `get_templates` | List the workspace's slide templates |
| `get_generation_limits` | Check remaining generation capacity |

The bundled skill teaches the workflow — resolve the workspace, start once
with a stable idempotency key, poll to terminal, and only then report success.

## Install

```
/plugin install mindframes
```

Then connect: run `/mcp`, pick **mindframes**, and approve the consent
screen. The bundled `connecting-mindframes` skill walks Claude through it and
through what to do when authorization expires or a workspace is wrong.

## Scopes

Consent requests exactly two scopes, `presentations:read` and
`presentations:write`. This endpoint is presentation-only by construction — it
registers eight tools and cannot reach videos, brands, agents, media or
billing, whatever the account itself is entitled to.

## Costs

Generating a presentation spends Mindframes credits from the selected
workspace. Reading, listing and sharing are free.

## Not included yet

File export (PDF/PPTX) is intentionally absent from this release. It will be
added once export artifacts are served from private, expiring storage.

## Source

The plugin is built from the Mindframes application repository and mirrored
here. File issues at https://www.mindframes.app.

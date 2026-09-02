---
name: creating-presentations
description: Creates, finds, and shares Mindframes presentations through MCP. Use when the user asks for a presentation, deck, or slides, or wants to manage an existing Mindframes presentation.
---

# Mindframes presentations

Use the presentation tools exposed by the Mindframes MCP server. The workflow
below uses their server-registered names; let the host apply its own MCP
namespace.

1. If `teamId` is unknown, call `list_workspaces` and use the selected workspace for every later call.
2. Resolve missing topic or slide-count requirements. Call `get_generation_limits` only when limits or available capacity matter.
3. Call `generate_presentation` once with a stable idempotency key. It returns a `runId`; pass that value to `get_generation_status` as its `jobId` argument, wait `nextPollAfterSeconds`, and repeat until terminal.
4. Claim success only after terminal completion. Then call `get_presentation` when slide details are needed.
5. Use `list_presentations` or `get_presentation` for existing decks. Call `update_visibility` only when sharing was requested; return the deck's own link rather than claiming a file download, which this server does not expose.

Surface typed tool errors as failures; never substitute an empty result. Retry
non-terminal transport errors with the same key or `runId`. After a terminal
failure, start a new attempt with a new key only when the user asks.

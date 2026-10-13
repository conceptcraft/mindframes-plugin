---
name: creating-presentations
description: Creates, finds, edits, and exports Mindframes presentations through MCP. Use for decks, slides, speaker notes, or managing an existing Mindframes presentation.
---

# Mindframes presentations

Use the presentation tools exposed by the Mindframes MCP server. The workflow
below uses their server-registered names; let the host apply its own MCP
namespace.

1. If `teamId` is unknown, call `list_workspaces` and use the selected workspace for every later call.
2. Use `list_presentations` and `get_presentation` to select and read an existing deck. Resolve missing topic or slide-count requirements for creation; call `get_generation_limits` when limits or available capacity matter.
3. For creation, call `generate_presentation` with a stable idempotency key. Follow its human approval URL, then retry the exact input and key with the browser-issued receipt. Poll `get_generation_status` with the returned `runId` as `jobId`, waiting `nextPollAfterSeconds` until terminal.
4. For slide-content edits, read the current deck and call `edit_presentation` with its exact version slug and requested targets. Follow the same approval-and-exact-retry flow, then poll `get_presentation_edit_status` using `jobId`. Claim a saved edit only after completion and reread `get_presentation`.
5. For speaker notes, use free `set_speaker_notes` to place supplied text verbatim or clear it. Use paid `generate_speaker_notes` for writing or rewriting; to improve existing notes pass `mode: regenerate` and `rewriteExisting: true`, with the user's instructions and selected slide IDs. Poll `get_speaker_notes_status` using `jobId`, then reread authorized saved notes through `get_presentation`. Slide-content editing does not change speaker notes; missing or withheld notes are not proof of a saved rewrite.
6. Call `update_visibility` only when sharing was requested and return the deck's own link. For an explicitly requested downloadable file, call `export_presentation` with HTML, PDF, or PPTX format and return its short-lived download URL.
7. Claim success only after the operation reports its completed saved result; acceptance or a `runId` alone is not completion.

Surface typed tool errors as failures; never substitute an empty result. Retry
non-terminal transport errors with the same key or `runId`. After a terminal
failure, start a new attempt with a new key only when the user asks.

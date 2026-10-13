# Mindframes Presentations — Claude & Codex plugin

Create, find, edit, export and share [Mindframes](https://www.mindframes.app)
presentations from Claude (chat, Cowork and Claude Code) and Codex.
Mindframes is a product of ConceptCraft AI.

## Install

From the Claude plugin directory, or in Claude Code:

```
/plugin install mindframes --marketplace conceptcraft/mindframes-plugin
```

Then connect: run `/mcp`, pick **mindframes**, and approve the Mindframes
sign-in and consent screen. The bundled `connecting-mindframes` skill walks
Claude through it, and through what to do when authorization expires or a
workspace is wrong. You need a Mindframes account; sign up at
https://www.mindframes.app.

## What the plugin contains and does

The plugin contains two skills and one remote MCP server declaration. It runs
no code on your machine and installs no packages.

- **MCP server:** `https://www.mindframes.app/mcp/presentations`, reached over
  HTTPS with OAuth. Consent requests exactly two scopes,
  `presentations:read` and `presentations:write`. This endpoint is
  presentation-only: it cannot reach your videos, brands, agents, media or
  billing.
- **`creating-presentations` skill:** teaches Claude the workflow — pick the
  workspace, start work once with a stable idempotency key, wait for the human
  approval it asks for, poll until the run finishes, and only then report
  success.
- **`connecting-mindframes` skill:** handles sign-in, workspace selection and
  authorization errors.

The plugin sends data only to the Mindframes server above: the prompts,
presentation content and choices you ask Claude to pass to these tools.

## Tools

| Tool | What it does |
| --- | --- |
| `list_workspaces` | Lists the workspaces you can use; every other call is scoped to one |
| `list_presentations` | Lists presentations in the workspace |
| `get_presentation` | Reads one presentation, its slides and its view and edit links |
| `get_templates` | Lists slide design templates |
| `get_generation_limits` | Shows your generation limits and usage |
| `generate_presentation` | Starts creating a presentation, after your approval in the browser |
| `get_generation_status` | Reports progress until the presentation is ready |
| `continue_presentation_approval` | Delivers a request you already approved |
| `edit_presentation` | Edits a saved presentation's slides, after your approval (available on request) |
| `get_presentation_edit_status` | Reports progress of an edit |
| `generate_speaker_notes` | Writes or rewrites speaker notes |
| `get_speaker_notes_status` | Reports progress of speaker notes |
| `set_speaker_notes` | Saves speaker notes you supply, word for word (free) |
| `export_presentation` | Returns a short-lived download link for an HTML, PDF or PowerPoint file |
| `update_visibility` | Sets who can open a presentation: you, your team, or anyone with the link |
| `open_mindframes_presentations` | Opens the Mindframes presentation viewer inside a host that supports MCP Apps |
| `create_presentation_studio_launch` | Prepares read-only Studio access for a presentation or a run in progress |

## Example prompts

- "Create a 6-slide presentation on our Q3 product launch for the sales team."
- "List my five most recent Mindframes presentations."
- "Write speaker notes for the launch presentation."
- "Export the launch presentation as a PowerPoint file."

## Costs

Creating presentations, editing slides and writing speaker notes use
Mindframes credits from the selected workspace, and you approve each new
presentation and edit before it starts. Reading, listing, setting your own
speaker notes, exporting and sharing are free.

## Availability

Editing slides by asking is available on request: contact us to turn it on for
your workspace. Everything else works with any Mindframes account.

## Privacy and support

- Privacy policy: https://www.mindframes.app/en/privacy
- Terms of service: https://www.mindframes.app/en/terms
- Support: support@conceptcraft.ai

## License

Proprietary. Copyright (c) 2026 ConceptCraft AI. All rights reserved. See
[LICENSE](LICENSE).

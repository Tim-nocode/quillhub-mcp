<p align="center">
  <img src="logo.png" width="120" alt="QuillHub logo">
</p>

<h1 align="center">QuillHub MCP Server</h1>

<p align="center">
  Your meeting transcripts, readable by Claude, Cursor and any other MCP client.<br>
  Search past calls, read who said what, pull decisions and action items, transcribe new files and links.
</p>

<p align="center">
  <a href="https://quillhub.ai/en/help/mcp-claude-cursor?utm_source=github&utm_medium=directory&utm_campaign=mcp-showcase">Setup guide</a> ·
  <a href="https://quillhub.ai/en/docs/mcp?utm_source=github&utm_medium=directory&utm_campaign=mcp-showcase">Docs</a> ·
  <a href="https://quillhub.ai/en?utm_source=github&utm_medium=directory&utm_campaign=mcp-showcase">quillhub.ai</a> ·
  <a href="https://quillhub.ai/en/desktop?utm_source=github&utm_medium=directory&utm_campaign=mcp-showcase">Desktop app</a>
</p>

---

**Endpoint:** `https://mcp.quillhub.ai/mcp` (Streamable HTTP, hosted)
**Auth:** OAuth 2.1 with PKCE (interactive clients) or API key `qai_live_...` (scripts, CI)
**Source:** this repository documents the hosted server; the server itself is operated by QuillHub. There is nothing to install or self-host.

## What it is

[QuillHub](https://quillhub.ai/en?utm_source=github&utm_medium=directory&utm_campaign=mcp-showcase) records meetings from your desktop without a bot in the call (Zoom, Meet, Teams, phone, in person) and transcribes uploaded files and YouTube links in 98+ languages. Every recording comes back split by speaker with timestamps, chapters, a summary, decisions and action items.

The MCP server gives your AI assistant direct access to that archive. Ask Claude "what did we agree with the client last Tuesday", "what has Anna said about pricing this month", or "transcribe this file and draft the follow-up", and it calls the right tools itself.

## Quick start

Step-by-step guide with screenshots: [QuillHub + Claude/Cursor via MCP](https://quillhub.ai/en/help/mcp-claude-cursor?utm_source=github&utm_medium=directory&utm_campaign=mcp-showcase).

### Claude Desktop, Cursor, VS Code and other OAuth-capable clients

Point the client at the URL with no `Authorization` header. The client opens a browser, you sign in to QuillHub, done.

```json
{
  "mcpServers": {
    "quillhub": {
      "url": "https://mcp.quillhub.ai/mcp"
    }
  }
}
```

Claude.ai / Claude Desktop: **Settings → Connectors → Add custom connector**, paste `https://mcp.quillhub.ai/mcp`.

Claude Code:

```bash
claude mcp add --transport http quillhub https://mcp.quillhub.ai/mcp
```

### API key (headless, CI, clients without OAuth)

Get a key in the [Developers dashboard](https://quillhub.ai/en/developers?utm_source=github&utm_medium=directory&utm_campaign=mcp-showcase) (issued automatically at sign-up), then use the `mcp-remote` bridge:

```json
{
  "mcpServers": {
    "quillhub": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://mcp.quillhub.ai/mcp",
        "--header",
        "Authorization:Bearer qai_live_YOUR_KEY"
      ]
    }
  }
}
```

Write `Authorization:Bearer ...` with no space after the colon: some `mcp-remote` versions split arguments on spaces.

## Tools

| Tool | What it does |
|---|---|
| `search_meeting_transcripts` | Full-text search across meetings in a workspace: titles, summaries, decisions, action items and transcript text. Morphological matching in English and Russian. |
| `list_transcriptions` | Paginated list of recordings with summary, key theses, decisions, action items, participants, project. Filter by text, date range, workspace, project. |
| `get_transcription` | One recording in the format you need: `summary`, `text`, `segments`, `dialog` (who said what), `paragraphs`, `chapters`, `subtitles`. Supports time windows for long calls. |
| `get_person_speech` | Timestamped quotes from one person across every meeting, with filters by date and project. |
| `find_subjects` | Find tracked subjects (people, deals, projects, clients, candidates, custom types) with their AI-maintained note, open commitments and mention counts. |
| `get_subject_page` | Full page for one subject: living note, open commitments, every meeting mention with quotes and speakers, unresolved questions. |
| `get_state_timeline` | How a subject's note changed over time: revisions with diffs and the recording that caused each change. |
| `list_open_questions` | Contradictions and ambiguities the AI found across meetings (e.g. two different budget figures) that need a human decision. |
| `get_project_memory` | AI-maintained briefing for a project or workspace: purpose, glossary, key people, activity digest. |
| `list_subject_types` | Subject types available in a project and the fields each one tracks. |
| `list_workspaces` | Your workspaces and the projects inside them, so the agent can read teammates' shared recordings. |
| `create_transcription` | Transcribe a URL (YouTube, direct file link) or an inline base64 file up to 25 MB. |
| `wait_for_transcription` | Wait for a job to finish (up to 300 s per call), no polling loop needed. |
| `cancel_transcription` | Cancel a queued or running job. |
| `get_account` | Account, balance and subscription. Cheap liveness check. |
| `get_developer_docs` | QuillHub developer docs as markdown, for agents writing integration code. |

## Example prompts

- "Find every call where we discussed the migration deadline and list the decisions."
- "What did Maria commit to in last week's meetings? Quote her."
- "Transcribe https://youtu.be/... and give me the chapters with timestamps."
- "Brief me on the Acme project before tomorrow's call."
- "When did we first hear about the budget cut, and what changed after the last meeting?"

## Limits and pricing

- Connecting MCP is free. Transcription uses your normal QuillHub balance: 60 free minutes on sign-up, no card. Plans at [quillhub.ai/en/pricing](https://quillhub.ai/en/pricing?utm_source=github&utm_medium=directory&utm_campaign=mcp-showcase).
- Inline base64 uploads are capped at 25 MB; for larger files pass a URL. Files up to 10 hours / 5 GB via the app.
- No webhooks over MCP. Use the REST API webhooks for push delivery.

## Security and privacy

- The server is stateless and forwards your token to the QuillHub REST API; it stores no recordings and no secrets.
- Recordings are encrypted at rest and in transit, never used to train models, and can be deleted at any time.
- Don't commit real API keys into config files. Rotate keys in the Developers dashboard.

## Also available

- REST API with webhooks: [quillhub.ai/en/docs](https://quillhub.ai/en/docs?utm_source=github&utm_medium=directory&utm_campaign=mcp-showcase)
- Desktop app for macOS 13+ and Windows 10 (2004+): [quillhub.ai/en/desktop](https://quillhub.ai/en/desktop?utm_source=github&utm_medium=directory&utm_campaign=mcp-showcase)

## Support

Open an issue in this repository or write to tim.nocode@gmail.com.

QuillHub is built by Axevia Labs LLC.

## License

The MIT license covers the files in this repository (documentation and config examples). The hosted server at `mcp.quillhub.ai` is a proprietary service.

# Installing the QuillHub MCP server (instructions for AI agents)

QuillHub is a hosted remote MCP server. There is nothing to clone, build or run locally.

- Endpoint: `https://mcp.quillhub.ai/mcp`
- Transport: Streamable HTTP
- Auth: OAuth 2.1 (browser sign-in) or an API key `qai_live_...`

## Option A: OAuth (preferred when the client supports remote servers with OAuth)

Add this to the client's MCP settings:

```json
{
  "mcpServers": {
    "quillhub": {
      "url": "https://mcp.quillhub.ai/mcp"
    }
  }
}
```

On first use the client opens a browser; the user signs in to QuillHub (free account, 60 free transcription minutes, no card).

## Option B: API key through mcp-remote (Cline, older clients)

1. Ask the user for their QuillHub API key. It is shown at https://quillhub.ai/en/developers after sign-in and starts with `qai_live_`.
2. Add this to the MCP settings file (for Cline: `cline_mcp_settings.json`), replacing the placeholder:

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

Write `Authorization:Bearer` with no space after the colon.

3. Requires Node.js 18+ for `npx`.

## Verify

Call the `get_account` tool. It returns the user id, balance and subscription. Then try `list_transcriptions` or `search_meeting_transcripts`.

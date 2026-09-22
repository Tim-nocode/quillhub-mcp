---
name: quillhub
displayName: QuillHub
description: Search meeting transcripts, read who said what, pull decisions and action items, and transcribe files and links.
keywords: ["meeting", "meetings", "transcript", "transcription", "call notes", "action items", "decisions", "quillhub"]
author: QuillHub
---

## Onboarding

The `quillhub` MCP server is hosted at `https://mcp.quillhub.ai/mcp`. On first use the client opens a browser and the user signs in to QuillHub with OAuth. A free account includes 60 transcription minutes, no card.

Verify the connection by calling `get_account`.

## Which tool to use

| Task | Tool |
|---|---|
| Find meetings about a topic, person or decision | `search_meeting_transcripts` |
| Browse recent recordings with summaries, decisions, action items | `list_transcriptions` |
| Read one recording | `get_transcription` with `format`: `summary` first, then `dialog` or `segments` with `from_seconds`/`to_seconds` |
| What one person said across meetings | `get_person_speech` |
| Status of a deal, project or client | `find_subjects`, then `get_subject_page` |
| How something changed over time | `get_state_timeline` |
| Contradictions to resolve | `list_open_questions` |
| Briefing on a project | `get_project_memory` |
| Workspace and project ids | `list_workspaces` |
| Transcribe a new file or link | `create_transcription`, then `wait_for_transcription` |

## Reading long recordings

Fetch `summary` and read its `toc`, pick the chapter, then fetch `dialog` for that time window plus one neighbouring chapter on each side.

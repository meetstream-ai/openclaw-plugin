# MeetStream for OpenClaw

Use MeetStream meeting bots from [OpenClaw](https://github.com/openclaw/openclaw): send a bot to a Zoom, Google Meet or Microsoft Teams call, then get the recording, the transcript and the meeting data.

## Install

The MeetStream skills and the hosted MeetStream MCP server for OpenClaw come from the [MeetStream plugin](https://github.com/meetstream-ai/claude-plugin). This repository holds no install files. Run:

```bash
git clone https://github.com/meetstream-ai/claude-plugin.git ~/.meetstream-plugin
~/.meetstream-plugin/setup --host openclaw
```

`setup` installs the MeetStream skills into OpenClaw and adds the hosted MCP server `https://mcp.meetstream.ai/mcp` with OAuth sign-in. You can run it again safely.

## Sign in

```bash
openclaw mcp login meetstream
```

A browser opens `app.meetstream.ai`. Sign in (or create an account) and click **Allow**. MeetStream creates an API key for this connection. You do not copy the key.

## Check that it works

Ask OpenClaw to call the MeetStream tool `list_bots`. A list of bots, or an empty list, means that MeetStream is connected.

## Use it

- "Send a bot to <meeting link> and take notes."
- "Get the notes from my last meeting."

The bot records audio only, unless you ask for video. MeetStream transcription adds $0.10 for each hour of bot time. MeetStream bills from prepaid credit: if the credit is low, add credit at <https://app.meetstream.ai/billing>.

## More

- Install steps for other agents: [INSTALL_FOR_AGENTS.md](https://github.com/meetstream-ai/claude-plugin/blob/main/INSTALL_FOR_AGENTS.md)
- Hosted MCP server: [meetstream-mcp](https://github.com/meetstream-ai/meetstream-mcp)
- Docs: <https://docs.meetstream.ai>
- Website: <https://meetstream.ai>

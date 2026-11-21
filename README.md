# MeetStream for OpenClaw

Use MeetStream meeting bots from [OpenClaw](https://github.com/openclaw/openclaw): send a bot to a Zoom, Google Meet or Microsoft Teams call, then get the recording, the transcript and the meeting data.

## Two ways to integrate

1. **Bot API: meeting bots and recording** (start here). A bot joins the call and records it. You get recordings, transcripts, participants, chat and webhooks after the call, or live data during the call. The install below gives OpenClaw this path.
2. **Agents API: a voice agent in the meeting** (API name MIA). Use it only when you want an agent that talks in the call. See [Voice agents](#voice-agents).

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

## Voice agents

A voice agent listens and talks in the call. Set it up in the MeetStream dashboard or with the API, in this order:

| Setup | What you do | Price per meeting hour |
|---|---|---|
| a. MeetStream's inbuilt keys (default) | Pick the modules (speech-to-text, LLM, voice, or just the LLM) and the models. No provider account. Inbuilt keys cover OpenAI and Deepgram today. | Bot price + $0.25 agent infrastructure + the per-hour price of the models you pick. The dashboard agent editor shows the rate before the bot runs. |
| b. Your own provider keys | Save your keys (OpenAI, Anthropic, Deepgram, ElevenLabs and others) in the dashboard and pick any supported model. | Bot price + $0.25 agent infrastructure. Your provider bills you for model usage. |
| c. Your own agent: OpenClaw | OpenClaw on your computer writes what the voice agent says, on your own model subscription. Run the [MeetStream Agent Kit](https://github.com/meetstream-ai/agent-kit) with `--agent openclaw` (text only: no meeting tools) and set `model.provider: "local_agent"` on the agent. | Bot price + $0.25 agent infrastructure. MeetStream charges no model tokens for OpenClaw's replies. |

The bot price is $0.35 per recording hour ($0.25 at volume): <https://meetstream.ai/pricing>. Inbuilt keys are not on for every account yet. If the API answers "MeetStream-managed keys are not available for … Use your own key.", use your own keys (setup b).

Setup c is never the default. Know this first: what people say in the meeting reaches an agent that can access your computer and terminal, and they can try to give it instructions (prompt injection). Your computer, OpenClaw and the Agent Kit bridge must stay on for the whole call. Use the lowest access level (`readonly`, the default) and keep the activation code on. Save the bridge's pairing token under **Integrations > Models > Local agent**, then click **Test connection** in the agent editor. Steps: [MIA Screen Sharing and Custom Agents](https://docs.meetstream.ai/guides/features/mia-screen-sharing-and-custom-agents).

## Latest docs

- Two ways to integrate: <https://docs.meetstream.ai/guides/introduction/integration-paths>
- First bot: <https://docs.meetstream.ai/guides/get-started/create-your-first-bot>
- Hosted MeetStream MCP server: <https://docs.meetstream.ai/build-with-ai/meetstream-mcp-server>
- Voice agent pricing and keys: <https://docs.meetstream.ai/guides/mia/pricing-and-keys>
- Your own agent (`local_agent`): <https://docs.meetstream.ai/guides/features/mia-screen-sharing-and-custom-agents>

For the latest details, fetch the page with `.md` appended, read <https://docs.meetstream.ai/llms.txt>, or connect the docs MCP server at <https://docs.meetstream.ai/_mcp/server>.

## More

- Install steps for other agents: [INSTALL_FOR_AGENTS.md](https://github.com/meetstream-ai/claude-plugin/blob/main/INSTALL_FOR_AGENTS.md)
- Hosted MCP server: [meetstream-mcp](https://github.com/meetstream-ai/meetstream-mcp)
- Docs: <https://docs.meetstream.ai>
- Website: <https://meetstream.ai>

# MeetStream for OpenClaw

Use MeetStream meeting bots from [OpenClaw](https://github.com/openclaw/openclaw): send a bot to a Zoom, Google Meet or Microsoft Teams call, then get the recording, the transcript and the meeting data.

## Three ways to integrate

1. **Recording only (Bot API)** (start here). A bot joins the call and records it. You get recordings, transcripts, participants, chat and webhooks after the call, or live data during the call. The install below gives OpenClaw this flow.
2. **Agents in meetings, your own stack.** Try this first when you want an agent that talks in the call: 2a a bridge server with your own speech-to-text, LLM and voice, or 2b your own agent, for example OpenClaw, as the brain. See [Voice agents](#voice-agents).
3. **Managed agent infrastructure (Agents API).** MeetStream hosts the voice agent (API name MIA). See [Voice agents](#voice-agents).

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

A voice agent listens and talks in the call. After recording only, there are three flows, in this order:

| Flow | What you do | Price per meeting hour |
|---|---|---|
| 2a. Bridge server (your own stack) | The bot sends live meeting audio to your server (`live_audio_required`), and your server sends audio, chat and images back on the control socket (`socket_connection_url`). You run your own speech-to-text, LLM and voice, hosted or local. See [Bridge server architecture](https://docs.meetstream.ai/guides/websockets/bridge-server-architecture) and the [MeetStream Bridge](https://github.com/meetstream-ai/meetstream-agent) starter. | Bot price only. No agent infrastructure, no model tokens. |
| 2b. Your own agent as the brain: OpenClaw | OpenClaw on your computer writes what the voice agent says, on your own model subscription. Run the [MeetStream Agent Kit](https://github.com/meetstream-ai/agent-kit) with `--agent openclaw` (text only: no meeting tools) and set `model.provider: "local_agent"` on the agent. | Bot price + $0.25 agent infrastructure. MeetStream charges no model tokens for OpenClaw's replies. |
| 3. Managed agent infrastructure (Agents API, MIA) | MeetStream hosts the agent. 3a: MeetStream's inbuilt keys (OpenAI and Deepgram today). 3b: your own provider keys (OpenAI, Anthropic, Deepgram, ElevenLabs and others). | Bot price + $0.25 agent infrastructure + the per-hour price of the models you pick. The dashboard agent editor shows the rate before the bot runs. With 3b your provider bills you for model usage. |

The bot price is $0.35 per recording hour ($0.25 at volume): <https://meetstream.ai/pricing>. Want no infrastructure work? Use flow 3. Inbuilt keys are not on for every account yet. If the API answers "MeetStream-managed keys are not available for … Use your own key.", use your own keys (3b) and do not retry. A flow 3 bot takes only `agent_config_id`; do not add the bridge sockets to it.

Flow 2b is never the default. Know this first: what people say in the meeting reaches an agent that can access your computer and terminal, and they can try to give it instructions (prompt injection). Your computer, OpenClaw and the Agent Kit bridge must stay on for the whole call. Use the lowest access level (`readonly`, the default) and keep the activation code on. Save the bridge's pairing token under **Integrations > Models > Local agent**, then click **Test connection** in the agent editor. Steps: [MIA Screen Sharing and Custom Agents](https://docs.meetstream.ai/guides/features/mia-screen-sharing-and-custom-agents).

## Latest docs

- Integration paths: <https://docs.meetstream.ai/guides/introduction/integration-paths>
- Bridge server (flow 2a): <https://docs.meetstream.ai/guides/websockets/bridge-server-architecture>
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

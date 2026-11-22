# MeetStream for OpenClaw

Use MeetStream meeting bots from [OpenClaw](https://github.com/openclaw/openclaw): send a bot to a Zoom, Google Meet or Microsoft Teams call, then get the recording, the transcript and the meeting data.

## Build tracks

1. **Recording bot** (start here). A bot joins the call, records it and transcribes it. 1a: recordings, transcripts, participants, chat and webhooks after the call. 1b: live data during the call. The install below gives OpenClaw this track.
2. **Bridge server (your own stack, local).** Try this first when you want an agent that talks in the call: your server runs your own speech-to-text, LLM and text-to-speech, free local models included. See [Voice agents](#voice-agents).
3. **Agents API infra (hosted by MeetStream).** MeetStream hosts the voice agent (API name MIA): 3a MeetStream's own, default keys, or 3c your own keys. 3b (bring your own agent infra, LiveKit) and 3d (local agent: your own agent CLI, for example OpenClaw, as the brain) are coming soon. See [Voice agents](#voice-agents).

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

A voice agent listens and talks in the call. After the recording bot, use these tracks, in this order:

| Track | What you do | Price per meeting hour |
|---|---|---|
| 2. Bridge server (your own stack, local) | The bot sends live meeting audio to your server (`live_audio_required.websocket_url`), and your server sends audio, chat and images back on the control socket (`socket_connection_url.websocket_url`). You run your own speech-to-text, LLM and text-to-speech; free local models work (for example faster-whisper or Parakeet, a model in Ollama, Piper or Kokoro; check each model's license before commercial use). On a laptop, use a local tunnel (ngrok, cloudflared). See [Bridge server architecture](https://docs.meetstream.ai/guides/websockets/bridge-server-architecture) and the [MeetStream Bridge](https://github.com/meetstream-ai/meetstream-agent) starter. | Bot price only. No agent infrastructure, no model tokens. |
| 3. Agents API infra (MIA, hosted by MeetStream) | MeetStream hosts the agent. 3a: MeetStream's own, default keys. Inbuilt keys work in pipeline mode only: Deepgram (speech-to-text) and OpenAI (LLM and voice). Realtime mode runs on OpenAI and needs your own OpenAI key. 3c: your own provider keys (OpenAI, Anthropic, Deepgram, ElevenLabs and others). | Bot price + $0.25 agent infrastructure + the per-hour price of the models you pick. The dashboard agent editor shows the rate before the bot runs. With 3c your provider bills you for model usage. |
| 3b. Bring your own agent infra (LiveKit) | Coming soon. Not available yet. | |
| 3d. Local agent: OpenClaw as the brain | Coming soon. Not available in production yet. OpenClaw on your computer will write what the voice agent says, through the [MeetStream Agent Kit](https://github.com/meetstream-ai/agent-kit) (a preview) and `model.provider: "local_agent"`. | |

The bot price is $0.35 per recording hour ($0.25 at volume): <https://meetstream.ai/pricing>. Want no infrastructure work? Use track 3a or 3c. Inbuilt keys are not on for every account yet. If the API answers "MeetStream-managed keys are not available for … Use your own key.", use your own keys (3c) and do not retry. A hosted agent bot (track 3) takes only `agent_config_id`; do not add the bridge sockets to it.

OpenClaw as the brain of the voice agent (track 3d) is coming soon. When it is available, what people say in the meeting will reach an agent that can access your computer and terminal, and they can try to give it instructions (prompt injection): use the lowest access level.

## Latest docs

- Integration paths: <https://docs.meetstream.ai/guides/introduction/integration-paths>
- Bridge server (track 2): <https://docs.meetstream.ai/guides/websockets/bridge-server-architecture>
- First bot: <https://docs.meetstream.ai/guides/get-started/create-your-first-bot>
- Hosted MeetStream MCP server: <https://docs.meetstream.ai/build-with-ai/meetstream-mcp-server>
- Voice agent pricing and keys: <https://docs.meetstream.ai/guides/mia/pricing-and-keys>
- Local agent (`local_agent`, track 3d, coming soon): <https://docs.meetstream.ai/guides/features/mia-screen-sharing-and-custom-agents>

For the latest details, fetch the page with `.md` appended, read <https://docs.meetstream.ai/llms.txt>, or connect the docs MCP server at <https://docs.meetstream.ai/_mcp/server>.

## More

- Install steps for other agents: [INSTALL_FOR_AGENTS.md](https://github.com/meetstream-ai/claude-plugin/blob/main/INSTALL_FOR_AGENTS.md)
- Hosted MCP server: [meetstream-mcp](https://github.com/meetstream-ai/meetstream-mcp)
- Docs: <https://docs.meetstream.ai>
- Website: <https://meetstream.ai>

# Gonka AI Drop: setup notes

[Gonka AI Drop](https://aidrop.gnk.space) lists the inference brokers of the Gonka network, a decentralized GPU network. Each broker gives new users free welcome tokens for open-weight models through an OpenAI-compatible API.

| Broker | Welcome offer |
|---|---|
| Gonka DAHL | 100M tokens |
| Gonka API | up to 100M tokens |
| Gonka Router | $20 in credits |
| Gonka GG | 1M tokens |

Models: DeepSeek V4-Flash (up to 400K context), GLM-5.3-Flash and MiniMax M2.7. These are model tokens, not a cryptocurrency, and no wallet is needed.

> Disclosure: this repo is maintained by people affiliated with Gonka.

## Claim your tokens

1. Open https://aidrop.gnk.space and pick a broker.
2. Say hi in that broker's Discord channel (linked on its card).
3. Sign up on the broker's site and create an API key.

## Connection values

Gonka DAHL, checked on 8 Oct 2026:

| Setting | Value |
|---|---|
| Base URL | `https://inference.dahl.global/v1` |
| API key | your DAHL key |
| Models | `MiniMaxAI/MiniMax-M2.7`, `deepseek-ai/DeepSeek-V4-Flash-0731`, `zai-org/GLM-5.3-Flash` |

- New DAHL keys start at 0 tokens. Allocate tokens from your account pool to the key at https://inference.dahl.global/account before the first request, or you'll get `402 available tokens exhausted`.
- Model IDs change over time. The live list is public: `curl https://inference.dahl.global/v1/models`.
- For the other brokers, use the base URL from their docs, linked on their AI Drop card.

## Setups

### Python (openai SDK)

```python
from openai import OpenAI

client = OpenAI(base_url="https://inference.dahl.global/v1", api_key="YOUR_KEY")
reply = client.chat.completions.create(
    model="MiniMaxAI/MiniMax-M2.7",
    messages=[{"role": "user", "content": "Hello!"}],
)
print(reply.choices[0].message.content)
```

### curl

```bash
curl https://inference.dahl.global/v1/chat/completions \
  -H "Authorization: Bearer $DAHL_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"model": "MiniMaxAI/MiniMax-M2.7", "messages": [{"role": "user", "content": "Hello!"}]}'
```

### JanitorAI and SillyTavern

See [guides/roleplay.md](guides/roleplay.md).

### LibreChat

Add under `endpoints.custom` in `librechat.yaml`, with `DAHL_API_KEY` in `.env`:

```yaml
    - name: "Gonka DAHL"
      apiKey: "${DAHL_API_KEY}"
      baseURL: "https://inference.dahl.global/v1"
      models:
        default: ["MiniMaxAI/MiniMax-M2.7"]
        fetch: true
      titleConvo: true
      titleModel: "current_model"
```

### Codex CLI

From DAHL's docs. Use a custom provider id, not the built-in `openai` one:

```toml
model = "MiniMaxAI/MiniMax-M2.7"
model_provider = "dahl"

[model_providers.dahl]
name = "Dahl"
base_url = "https://inference.dahl.global/v1"
env_key = "DAHL_API_KEY"
requires_openai_auth = false
```

### Other tools

Most coding and agent tools (Cline, Roo Code, Kilo Code, opencode, n8n and others) have an "OpenAI-compatible" provider option. Enter the base URL, your API key and a model ID from the table above.

## Notes

- MiniMax M2.7 starts its replies with its reasoning between `<think>` and `</think>`.
- When a model is busy, DAHL returns `429` with `model_concurrency`. Retry after the `Retry-After` delay or switch models.

# Free DeepSeek V4-Flash, GLM-5.3 and MiniMax M2.7 for JanitorAI and SillyTavern

Gonka AI Drop gives free welcome tokens for DeepSeek V4-Flash (up to 400K context), GLM-5.3-Flash and MiniMax M2.7: up to 100M tokens from Gonka DAHL, 1M from Gonka GG.

> Disclosure: I'm affiliated with Gonka.

## 1. Get a key

1. Go to https://aidrop.gnk.space and pick **Gonka DAHL** (100M tokens) or **Gonka GG** (1M). Both accept requests straight from the JanitorAI website.
2. Say hi in that broker's Discord channel, then sign up on its site and copy your API key.
3. DAHL only: new keys start at 0 tokens. At https://inference.dahl.global/account, allocate tokens from your pool to the key.

## 2. JanitorAI

API Settings → **Proxy**:

| Field | Gonka DAHL | Gonka GG |
|---|---|---|
| Model | `MiniMaxAI/MiniMax-M2.7` | `MiniMaxAI/MiniMax-M2.7` |
| Proxy URL | `https://inference.dahl.global/v1/chat/completions` | `https://api.proxy.gonka.gg/v1/chat/completions` |
| API key | your DAHL key | your GG key |

You can also use `deepseek-ai/DeepSeek-V4-Flash-0731` or `zai-org/GLM-5.3-Flash` as the model.

## 3. SillyTavern

API → **Chat Completion** → source **Custom (OpenAI-compatible)**:

- Custom endpoint: `https://inference.dahl.global/v1` (or `https://api.proxy.gonka.gg/v1`)
- API key: your key
- Model: `MiniMaxAI/MiniMax-M2.7`

## Tips

- MiniMax M2.7 starts each reply with its reasoning between `<think>` and `</think>`.
- If a model is busy you'll get a `model_concurrency` error. Wait a few seconds or switch models.

These are model tokens, not a cryptocurrency, and no wallet is needed. Start at https://aidrop.gnk.space

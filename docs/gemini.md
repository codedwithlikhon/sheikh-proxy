# Gemini API & How Sheikh Maps to It

This file describes the Gemini LLM side, how Sheikh interacts with it, and nuance for proxy mapping.

## Gemini Overview

- Gemini supports **OpenAI compatibility** mode: you can use OpenAI libraries with adjusted `base_url` and your Gemini key. :contentReference[oaicite:0]{index=0}  
- Endpoints include:
  - `generateContent` (REST)  
  - `streamGenerateContent` (SSE streaming)  
  - `embedContent` (embeddings)  
  - `Live API` (WebSocket-based, audio / multimodal) :contentReference[oaicite:1]{index=1}  
- Gemini’s “thinking” capability: 2.5 models use internal reasoning / “thinking budgets” for stepwise planning. :contentReference[oaicite:2]{index=2}  
- Gemini supports structured output, JSON constraints, function-calling, multimodal inputs (text + images) :contentReference[oaicite:3]{index=3}  

## How Sheikh Proxy Maps to Gemini

| Sheikh Alias     | Gemini Model         | Notes |
|------------------|------------------------|-------|
| sheikh-1.5-md     | gemini-2.5-flash        | balanced cost / performance |
| sheikh-1.5-lg     | gemini-2.5-pro          | more compute, better reasoning |
| sheikh-1.0-md     | gemini-2.5-flash-lite   | fastest / lightweight model |

### Upstream Call Structure

Sheikh will convert:

```json
{
  "model": "gemini-2.5-flash",
  "contents": [
    { "role": "user", "parts": [ { "text": ... } ] },
    ...
  ],
  "stream": true,
  "thinkingBudget": 0 / X
}
```

to/from OpenAI-style request/response.

Example (Python with OpenAI lib via compatibility mode):

```python
from openai import OpenAI
client = OpenAI(api_key="GEMINI_KEY",
                base_url="https://generativelanguage.googleapis.com/v1beta/openai/")
resp = client.chat.completions.create(model="gemini-2.5-flash", messages=[...])
```

([Google AI for Developers][1])

### Prompt Engineering with Gemini

* Use reasoning when needed, but limit verbosity
* Use schema enforcement (JSON blocks) and delimiters
* Use thinkingBudget parameter to fine-tune reasoning depth
* Wrap system + guard rails to prevent hallucination / injection

### Caveats & Notes

* Always keep API key secure (server-side only). ([Google AI for Developers][2])
* Streaming responses from Gemini follow SSE / JSON objects with `candidates`, `parts`, etc. ([Google AI for Developers][3])
* For vision / document inputs (PDF, images), Gemini supports inline data. ([Google AI for Developers][4])

---

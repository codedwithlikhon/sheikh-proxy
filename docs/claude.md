# Claude / Other Provider Specification & Integration Notes

After Gemini support, Sheikh Proxy can also wrap **Claude** or other LLM providers. This file describes how.

## Claude Overview

- Claude has proprietary API endpoints (Anthropic, etc.).  
- Supports chat, structured output, function calling, etc.  
- May have different rate limits, error models, streaming semantics.

## Adapter Strategy

- Write a Claude adapter that takes Sheikh’s unified request (OpenAI-style) and converts to Claude’s API format.
- Normalize Claude’s streaming / incremental response into OpenAI chunk format.
- Map Claude error types to OpenAI-style error objects.
- Optionally provide fallback between Gemini and Claude.

## Prompt Divergence

Because Claude may respond differently:

- You may need different **prompt templates / wrappers** when directing to Claude vs Gemini.
- Use **variant prompt sets** per provider, and let `run-variants` pick best per context.

## Provider Tags & Metadata

In your `/v1/models` listing, you may include tags like:

- `via: gemini`
- `via: claude`

But avoid exposing provider names in UI branding.

## Example Integration Flow

1. Client calls `/v1/chat` with `model="sheikh-claude-1"`
2. Proxy sees alias → maps to Claude adapter
3. Claude adapter receives messages (already wrapped) → sends to Claude API
4. Proxy normalizes and streams back to client

## Prompt Engineering Differences

Claude may accept different guard rails, personality styles, or prompts. Keep variant prompt templates per provider.

## Testing & Validation

- Run same user prompt via Gemini and via Claude adapters and compare consistency.
- Use `run-variants` to pick best prompt version per provider.
- Verify structured output, JSON shapes, error handling in both.

---

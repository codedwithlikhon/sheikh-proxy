# Sheikh Proxy Blueprint

## Purpose & Vision
Sheikh Proxy is an OpenAI-compatible API wrapper over underlying LLMs (starting with Gemini, later Claude or others). It allows clients to use standard OpenAI-style endpoints while we handle aliasing, prompt engineering, fallback, streaming, tools, and UI/landing.

## Core Components

- **API / Proxy backend**  
  Handles requests at `/v1/*`, does alias mapping, prompt wrapping, upstream calls, error normalization, streaming, retries.

- **Provider Adapters**  
  One adapter per LLM provider (Gemini, Claude, etc.), abstracting conversion to/from upstream format.

- **Prompt Engineering Layer**  
  Wraps user messages with system prompts / guard rails, uses structured delimiters, few-shot, CoT summaries.

- **Agent / Tool Layer**  
  Supports built-in tools: Bash, code execution, file edits, web fetch, web search, memory. LLM prompts may trigger tools.

- **UI / Dashboard / Landing**  
  Next.js or React frontend showing status, available models, docs, code snippets, branding.

- **Edge / Gateway**  
  NGINX or reverse proxy for TLS, strict SSL, session tickets, upstream validation.

- **Deployment / Orchestration**  
  Docker / Compose, environment config for keys, alias maps, secrets, scaling.

## Data Flow

```

Client (OpenAI SDK / curl)
│
▼
Sheikh Proxy API
├── Authenticate & validate
├── Prompt wrapper / variant logic
├── Alias → upstream model
├── Provider adapter call (streaming/non-streaming)
├── Streaming normalization / chunking
├── Tool orchestration (if needed)
├── Logging, metrics, caching, fallback
▼
Upstream LLM (Gemini, Claude)

```

## Alias Mapping Example

| Sheikh Alias     | Upstream Model         |
|------------------|--------------------------|
| sheikh-1.5-md     | gemini-2.5-flash          |
| sheikh-1.5-lg     | gemini-2.5-pro            |
| sheikh-1.0-md     | gemini-2.5-flash-lite     |

## Error & Fallback Strategy

- On upstream failure (timeout, 5xx): automatically retry with fallback model or another provider.
- Map upstream errors into OpenAI-style error JSON (`{"error":{"message":..., "type":...}}`).
- Provide consistent status codes (400, 401, 429, 500, etc.)

## Observability & Monitoring

- Log request/response times, token counts, error rates
- Expose health endpoint `/` or `/health`
- UI dashboard pings these endpoints for status indication
- Use metrics engine (Prometheus, etc.)

## Delivery Principles

Execution quality is as important as architecture. Follow the [delivery excellence principles](delivery-principles.md) for planning, automated testing, code reviews, and CI/CD so features are production-ready from the first iteration.

---

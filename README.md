# Sheikh Proxy

Sheikh Proxy is an OpenAI-compatible API wrapper that unifies access to multiple LLM providers. The project aims to provide a consistent developer experience, prompt-engineering guardrails, robust tooling, and observability so that clients can reuse existing OpenAI SDK integrations while benefiting from best-of-breed upstream models.

## Project Overview

- **Proxy API** at `/v1/*` translating OpenAI-style calls into upstream provider requests (starting with Google Gemini and later Anthropic Claude or others).
- **Provider adapters** encapsulate provider-specific payload transforms, streaming semantics, and error handling.
- **Prompt engineering layer** manages system prompts, guard rails, few-shot context, and variant evaluation.
- **Agent & tool framework** enables tool calls (shell, code execution, file editing, web fetch/search, memory) orchestrated by the proxy.
- **UI/Dashboard** surfaces status, model availability, documentation, and onboarding snippets.
- **Edge gateway & deployment** rely on hardened TLS, containerization, and infrastructure automation.

## Documentation

| Topic | Description |
| --- | --- |
| [Blueprint](docs/blueprint.md) | High-level vision, components, and data flow for the proxy. |
| [Agent Integration](docs/agents.md) | Tooling model and orchestration workflow for Sheikh Agent. |
| [Gemini Mapping](docs/gemini.md) | How OpenAI-compatible requests translate to Google Gemini APIs. |
| [Claude Integration](docs/claude.md) | Considerations for wrapping Anthropic Claude and other providers. |
| [Docker Operations](docs/docker.md) | Launching interactive shells and running commands in detached containers. |
| [Deployment Config Files](docs/deployment-configs.md) | Overview of Netlify, Cloudflare Workers, Render, and Vercel config manifests. |
| [Delivery Principles](docs/delivery-principles.md) | Engineering practices for planning, testing, and shipping Sheikh Proxy reliably. |

These documents capture the current architectural direction and delivery approach. Future updates will add API specifications, deployment instructions, and implementation details as the proxy evolves.

## Getting Started

Implementation work has not yet begun. To contribute:

1. Review the blueprint and provider integration notes in `docs/`.
2. Outline tasks for building the proxy backend, adapters, and supporting services.
3. Open issues or design proposals to discuss technical decisions before coding.

---

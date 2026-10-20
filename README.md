# Vesper Personal AI

A memory-grounded assistant with Recall, Plan, and Explain skills. Production inference calls Nebius Token Factory using NVIDIA Nemotron.

- Optional VESPER_KB_URL reuses Vesper's existing POST /v1/kb/search durable-memory endpoint; without it, two fictional sample memories are used.
- Optional VESPER_ROUTER_URL calls Vesper POST /v1/router/route for planning metadata only. It does not select or proxy the provider.
- Real inference posts to https://api.tokenfactory.nebius.com/v1/chat/completions using nvidia/nvidia-nemotron-3-nano-30b-a3b.
- DEMO_MODE=mock provides a clearly labeled non-qualifying preview.

This targets Personal AI because it combines persistent memory with reusable personal skills. Coding and Agentic Engineering is a weaker fit; code retrieval and generation are not implemented.

## Run locally

Requires Node.js 20 or newer. No package installation is needed.

    cp .env.example .env
    # Set NEBIUS_API_KEY in the shell or a private server secret manager.
    node server.mjs

Open http://localhost:3000. To preview the UI without model access, run DEMO_MODE=mock node server.mjs. Mock mode makes no provider call and cannot qualify.

## Configuration

- NEBIUS_API_KEY: server-only Token Factory credential. Never expose it to browser code or commit it.
- VESPER_KB_URL: optional base URL for the existing Vesper KB service.
- VESPER_ROUTER_URL: optional base URL for Vesper's router. Its decision is metadata, not the model-provider route.
- DEMO_MODE=mock: enables a clearly labeled preview without a key.
- PORT: listen port (default 3000).

The user's question and any retrieved memory snippets are sent to Nebius. Avoid sensitive information and never point a public demo at private Vesper memory; use fictional sample context. The app does not write prompts or responses to disk. It limits input to 2,000 characters and requests to 12 per minute per source IP in this single-process demo.

Tests use Node's built-in test runner with mocked fetch and make no network calls:

    npm test

## Deployment checklist

- Set NEBIUS_API_KEY using the host's server-side secret manager.
- Use HTTPS and leave DEMO_MODE unset for the qualifying run.
- Use only fictional memory in the public demo.
- Add a stricter ingress rate limit or access code before exposing a billable key.
- Verify /health and a real call, then capture evidence with the credential redacted.

## License and origin

This standalone MIT submission slice contains newly written code and docs only; it does not copy files from the private Vesper monorepo. See DEVELOPMENT.md.

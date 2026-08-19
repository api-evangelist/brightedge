---
name: Connect an agent to the BrightEdge MCP server
description: Attach a BrightEdge customer account to an MCP-capable agent so it can query BrightEdge SEO/AEO data live, using the provider's hosted remote MCP server and its OAuth 2.0 flow.
api: mcp/brightedge-mcp.yml
transport: http-streamable
endpoint: https://mcp2.brightedge.com/mcp
operations: []
source: https://www.brightedge.com/brightedge-mcp
generated: '2026-08-13'
method: generated
---

# Connect an agent to the BrightEdge MCP server

BrightEdge runs a hosted, remote MCP server. There is nothing to install: an MCP client
POSTs to an HTTPS endpoint. This skill is the connection contract, not a tool reference —
BrightEdge does not publish the tool names, and `tools/list` is OAuth-gated, so the agent
discovers the tool set itself after authenticating.

## Endpoints

| Transport | URL | Use when |
|---|---|---|
| HTTP / streamable | `https://mcp2.brightedge.com/mcp` | the client supports standard HTTP MCP transport (ChatGPT, Claude, Gemini Enterprise, Microsoft Copilot, n8n, Relevance AI) |
| SSE | `https://mcp2-sse.brightedge.com/sse` | the client requires Server-Sent Events (Manus, Lovable, OpenClaw) |
| OpenAI plugin | `https://mcp.brightedge.com/marketplace/v2` | using the pre-built BrightEdge plugin from the OpenAI plugin repository |

Prefer `mcp2.brightedge.com/mcp`. BrightEdge states the marketplace plugin lags, because
updates to it require OpenAI review; a custom connector against `mcp2` always sees the
current tool set.

## Prerequisites

1. An active BrightEdge account with valid login credentials. Every BrightEdge customer
   is eligible; there is no separate MCP SKU.
2. An OAuth **Client ID** (and, for Copilot Studio / Gemini Enterprise / Relevance AI, a
   **Client Secret**). These are **not self-serve** — request them from your BrightEdge
   Customer Success Manager or `integrations@brightedge.com`.
3. For Copilot Studio, the redirect URL your platform generates must be sent to BrightEdge
   to be allowlisted before the connector will authenticate.

## Authenticate

OAuth 2.0 authorization code with PKCE (`S256`), fronted by Auth0. Discovery is anonymous,
so a client can bootstrap without credentials:

- `GET https://mcp2.brightedge.com/.well-known/oauth-protected-resource` → the resource and
  its authorization server (RFC 9728)
- `GET https://mcp2.brightedge.com/.well-known/oauth-authorization-server` → endpoints and
  PKCE support (RFC 8414)

Endpoints:

- authorize: `https://mcp2.brightedge.com/authorize`
- token: `https://mcp2.brightedge.com/token`
- scope to request: `openid profile email`

Some platforms are documented against the Auth0 tenant directly
(`https://mrkt-0365.us.auth0.com/authorize` / `/oauth/token`) — use whichever form your
client's setup guide names. The login screen is BrightEdge-branded but hosted on auth0.com;
that is expected, not a phishing signal.

An unauthenticated call is rejected cleanly, which is the check to run first:

```
POST https://mcp2.brightedge.com/mcp
Accept: application/json, text/event-stream
{"jsonrpc":"2.0","id":1,"method":"tools/list"}

401  WWW-Authenticate: Bearer error="invalid_token",
     resource_metadata="https://mcp2.brightedge.com/.well-known/oauth-protected-resource/mcp"
```

## Discover tools

After the OAuth flow completes, call `tools/list` with the bearer token. Do not hard-code a
tool list from any third-party source — BrightEdge publishes only the data domains the
server covers:

DataCube X · AI HyperCube · Keyword Reporting · Share of Voice · Analytics Reporting ·
Google Search Console · AI Catalyst · Recommendations · AI Agent Insights

## Operating rules

- **The server is read-only.** BrightEdge states MCP tools retrieve data only and never
  modify the account. Do not construct a workflow that expects a write; the write surface
  lives on the REST Platform API v5.0 (`api.brightedge.com`), under separate credentials.
- **Data is live.** Results are pulled from the account at query time, so two identical
  prompts on different days legitimately differ. Timestamp any output you persist.
- **Name the connector, not the protocol.** Per BrightEdge's own guidance, agents invoke
  the tools reliably when a prompt says "Using BrightEdge…" and often fail to route on the
  word "MCP".
- **Expect truncated tool listings on some hosts.** BrightEdge documents that asking for a
  full tool list can return only a handful of tools on Gemini Enterprise (an output-length
  limit, not a connection fault). Ask for the tools relevant to a task instead.
- **No published rate limits.** Neither surface documents a quota, a 429, or a
  `Retry-After` header (`rate-limits/brightedge-rate-limits.yml`). Back off conservatively
  on any error and do not assume a retry budget.

## Where to go for the write side

Anything that changes state — creating keyword groups, managing users, submitting bulk
export jobs — is REST, not MCP. See `skills/brightedge-bulk-export.md`,
`skills/brightedge-pull-keyword-rankings.md` and `authentication/brightedge-authentication.yml`
for the key/basic/session auth that surface uses.

# Authentication

The server at `https://ai2human.app/api/mcp` accepts two kinds of bearer credential. Anything else gets `401 Unauthorized`.

| Credential | Best for | How you get it |
|---|---|---|
| **OAuth access token** | Chat apps and interactive clients (Claude, ChatGPT, Cursor, Claude Code) | Sign in through the browser when the client connects |
| **API key** | Agents, scripts and clients without OAuth | Create one in **Dashboard → Settings → AI agents (MCP)** |

Both require an account on a paid plan (Basic, Pro or Ultimate). The plan is checked on every call, so a key or connection pauses automatically if the plan ends.

## OAuth 2.1

The server follows the MCP authorization specification.

| | |
|---|---|
| Issuer | `https://ai2human.app` |
| Authorization endpoint | `https://ai2human.app/oauth/authorize` |
| Token endpoint | `https://ai2human.app/api/oauth/token` |
| Registration endpoint | `https://ai2human.app/api/oauth/register` (dynamic client registration) |
| Grant types | `authorization_code`, `refresh_token` |
| PKCE | Required, method `S256` |
| Client authentication | None (public clients) |
| Scope | `mcp` |

Discovery documents:

- Protected resource: <https://ai2human.app/.well-known/oauth-protected-resource/api/mcp>
- Authorization server: <https://ai2human.app/.well-known/oauth-authorization-server>

An unauthenticated request returns `401` with a `WWW-Authenticate: Bearer` header whose `resource_metadata` parameter points at the protected-resource document, so compliant clients start the flow on their own.

### Token lifetimes

| Token | Lifetime |
|---|---|
| Authorization code | 5 minutes |
| Access token | 1 hour, renewed automatically with the refresh token |
| Connection | 90 days from the day it was approved, then the user approves again |

An account can hold up to 20 connected apps. When a new one would exceed that, the oldest is dropped.

## API keys

1. Sign in and open **Dashboard → Settings**.
2. Under **AI agents (MCP)**, name a key and click **Create key**.
3. Copy the key straight away. It is shown once and cannot be read again.
4. Send it on every request:

```
Authorization: Bearer YOUR_API_KEY
```

Keys start with `a2h_sk_`. An account can hold up to 5 keys and revoke any of them from the same page. Keep keys out of source code and shared chats.

### Raw request

```bash
curl -X POST https://ai2human.app/api/mcp \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"get_usage","arguments":{}}}'
```

## Scope of access

A key or connection can call only the three documented tools. It cannot read the password, billing details or account settings. Session cookies are not accepted by the MCP endpoint, and MCP tokens are not accepted anywhere else on the site.

## Rate limits

- 120 requests per minute per IP address to `/api/mcp`.
- 30 tool calls per minute per account, across all of its keys and connections.

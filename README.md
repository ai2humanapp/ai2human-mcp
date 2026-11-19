<div align="center">

<img src="assets/logo.png" alt="AI2Human" width="220" />

# AI2Human MCP Server

**Humanize AI-written text and detect AI writing, straight from Claude, ChatGPT, Cursor and any other MCP client.**

[![MCP](https://img.shields.io/badge/MCP-Streamable%20HTTP-24c5dc)](https://modelcontextprotocol.io)
[![Auth](https://img.shields.io/badge/auth-OAuth%202.1%20%2B%20PKCE-0f1822)](docs/authentication.md)
[![Hosted](https://img.shields.io/badge/hosted-remote%20server-0f1822)](https://ai2human.app/mcp)
[![License](https://img.shields.io/badge/docs%20license-MIT-blue)](LICENSE)

[Website](https://ai2human.app) · [Setup guide](https://ai2human.app/mcp) · [Pricing](https://ai2human.app/pricing) · [Tools](docs/tools.md) · [Support](https://ai2human.app/contact)

</div>

---

AI2Human is a hosted [Model Context Protocol](https://modelcontextprotocol.io) server. Connect it once and your AI agent can rewrite machine-sounding text so it reads as natural human writing, check how AI-like a passage looks, and report your remaining allowance. There is nothing to install or run: you add one URL.

```
https://ai2human.app/api/mcp
```

| | |
|---|---|
| **Transport** | Streamable HTTP |
| **Sign-in** | OAuth 2.1 with PKCE and dynamic client registration, or an API key |
| **Access** | Paid plans: Basic, Pro and Ultimate |
| **Hosting** | Remote, hosted by AI2Human. No local process |

> **About this repository.** It holds the public documentation, the MCP Registry metadata (`server.json`) and an issue tracker for the hosted server. The service itself is proprietary and is not open source.

## Tools

| Tool | What it does | Cost |
|---|---|---|
| [`humanize_text`](docs/tools.md#humanize_text) | Rewrites AI-generated text as natural human writing while keeping meaning, facts, figures and citations. Eight styles. | 1 word per input word |
| [`detect_ai`](docs/tools.md#detect_ai) | Estimates how much of a text reads as AI-generated, with a per-sentence breakdown. | 1 detector scan |
| [`get_usage`](docs/tools.md#get_usage) | Shows your plan, words used and remaining, the per-call word limit and detector scans used. Read-only. | Free |

Every tool carries MCP annotations (`readOnlyHint`, `destructiveHint`, `idempotentHint`, `openWorldHint`). None of them deletes or changes anything beyond your own usage counters. Full parameters, outputs and examples are in [docs/tools.md](docs/tools.md).

## Quick start

1. Sign in to an [AI2Human](https://ai2human.app) account on a paid plan ([pricing](https://ai2human.app/pricing)).
2. Add the server URL to your AI app. Per-app steps are below and in [docs/clients.md](docs/clients.md).
3. Approve the connection when AI2Human asks, or paste an API key.
4. Ask your agent: *"Humanize this paragraph with AI2Human."*

### Claude (claude.ai and desktop)

Settings → Connectors → **Add custom connector** → paste `https://ai2human.app/api/mcp` → **Connect** → approve on the ai2human.app page. No API key needed.

### Claude Code

```bash
claude mcp add --transport http ai2human https://ai2human.app/api/mcp
```

Then run `/mcp` inside Claude Code and choose `ai2human` to sign in through the browser. To use an API key instead:

```bash
claude mcp add --transport http ai2human https://ai2human.app/api/mcp \
  --header "Authorization: Bearer YOUR_API_KEY"
```

### Cursor, VS Code and other clients

Add this to the client's MCP config (for Cursor, `.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "ai2human": {
      "url": "https://ai2human.app/api/mcp",
      "headers": { "Authorization": "Bearer YOUR_API_KEY" }
    }
  }
}
```

Clients that support OAuth can leave out `headers` and sign in through the browser. Clients that only support local (stdio) servers can use the [`mcp-remote`](https://www.npmjs.com/package/mcp-remote) bridge with the same URL.

### ChatGPT

In ChatGPT settings, open connectors, add a custom connector (MCP server), paste the URL and choose OAuth. Custom connectors depend on your ChatGPT plan and may need developer mode switched on.

## Example

> **You:** Check this paragraph with AI2Human. If it reads as AI, humanize it in an academic style and tell me the new score.

The agent calls `get_usage`, then `detect_ai`, then `humanize_text` with `style: "academic"`, and returns the rewritten text with a line such as:

```
[AI2Human <engine>] Words charged: 214. Words remaining: 49786. Our detector scores the result 94% human.
```

## Plans and limits

MCP uses the same allowance as the website. There is no separate MCP price. Figures below are current at the time of writing; the [pricing page](https://ai2human.app/pricing) is the source of truth.

| Plan | Words per month | Words per call |
|---|---:|---:|
| Basic | 15,000 | 600 |
| Pro | 50,000 | 2,000 |
| Ultimate | 100,000 | 5,000 |

Each account can make 30 tool calls per minute. To humanize text longer than the per-call limit, split it at paragraph breaks and call `humanize_text` once per part. If a call fails, no words are charged.

## Security and privacy

- A key or connection can call only the three tools above. It cannot read your password, billing details or account settings.
- API keys and OAuth tokens are stored only as hashes. A key is shown once, when you create it.
- Connected apps sign in with OAuth 2.1 and PKCE. Access tokens last one hour and renew automatically. A connection expires 90 days after approval and must then be approved again.
- You can disconnect an app or revoke a key at any time in **Dashboard → Settings**, and it stops working immediately.
- Text sent through MCP is handled like text entered on the website. See the [privacy policy](https://ai2human.app/privacy).

To report a vulnerability, see [SECURITY.md](SECURITY.md).

## Discovery metadata

For tooling that discovers servers automatically:

- OAuth protected resource: <https://ai2human.app/.well-known/oauth-protected-resource/api/mcp>
- OAuth authorization server: <https://ai2human.app/.well-known/oauth-authorization-server>
- MCP Registry entry: [`server.json`](server.json)

## Troubleshooting

See [docs/troubleshooting.md](docs/troubleshooting.md) for connection problems and every error code.

## Support

- Setup guide and live docs: <https://ai2human.app/mcp>
- Questions or account issues: <https://ai2human.app/contact>
- Bugs and feature requests for the MCP server: open an [issue](../../issues)

## License

The documentation and configuration in this repository are released under the [MIT License](LICENSE). The hosted AI2Human service is proprietary and covered by the [Terms of Service](https://ai2human.app/terms).

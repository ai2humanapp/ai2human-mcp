# Connecting your client

Server URL for every client:

```
https://ai2human.app/api/mcp
```

Use `https` exactly as shown, with no trailing slash. You need an AI2Human account on a paid plan. See [authentication.md](authentication.md) for how sign-in works.

## Claude (claude.ai and desktop)

1. Open Claude and go to **Settings → Connectors**.
2. Choose **Add custom connector**.
3. Paste the server URL and continue.
4. Click **Connect**. A page on ai2human.app opens: sign in if asked, then click **Approve**.

No API key is needed. Claude signs in through your AI2Human account.

## Claude Code

Sign in through the browser:

```bash
claude mcp add --transport http ai2human https://ai2human.app/api/mcp
```

Run `/mcp` inside Claude Code and choose `ai2human` to sign in.

Or use an API key:

```bash
claude mcp add --transport http ai2human https://ai2human.app/api/mcp \
  --header "Authorization: Bearer YOUR_API_KEY"
```

## ChatGPT

1. In ChatGPT settings, open the connectors (apps) section and add a custom connector or MCP server.
2. Paste the server URL and choose **OAuth** as the sign-in method.
3. A page on ai2human.app opens: sign in if asked, then click **Approve**.

Custom connectors in ChatGPT depend on your ChatGPT plan and may need developer mode switched on.

## Cursor

Add this to `.cursor/mcp.json`:

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

## VS Code and other MCP clients

Use the client's remote (HTTP) server option with the URL above. Clients that support OAuth can omit the `Authorization` header and sign in through the browser. Clients that only support local (stdio) servers can use the [`mcp-remote`](https://www.npmjs.com/package/mcp-remote) bridge:

```json
{
  "mcpServers": {
    "ai2human": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://ai2human.app/api/mcp"]
    }
  }
}
```

## Verify the connection

Ask your agent to call `get_usage`. It is free and read-only, and it returns your plan and remaining words if the connection works.

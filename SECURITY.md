# Security policy

## Reporting a vulnerability

Please do not open a public issue for a security problem.

Report it privately through <https://ai2human.app/contact> and write "Security report" in the subject. Include:

- what you found and where (endpoint, tool or flow),
- steps to reproduce,
- the impact you think it has.

Please give us a reasonable time to fix a problem before disclosing it, and do not access data that is not yours.

## Scope

In scope: the MCP endpoint `https://ai2human.app/api/mcp`, its OAuth flow and discovery documents, and API key handling.

Out of scope: denial-of-service testing, social engineering, and findings that need a compromised device or account.

## How credentials are handled

- API keys and OAuth tokens are stored only as hashes.
- OAuth uses PKCE (`S256`). Access tokens last one hour.
- Connections and keys can be revoked at any time in Dashboard → Settings and stop working immediately.

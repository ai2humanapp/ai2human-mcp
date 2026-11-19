# Troubleshooting

## Connection problems

**The app says it cannot connect.** Check that the URL is exactly `https://ai2human.app/api/mcp`, with `https` and no trailing slash.

**The approval page says the request is not valid.** Remove the connector in your AI app and add it again.

**A key stopped working.** It was revoked, or the account was removed. Create a new key in **Dashboard → Settings → AI agents (MCP)**.

**Everything returns `401`.** The key or token is missing, expired or revoked. OAuth clients refresh or sign in again automatically; with an API key, check the `Authorization: Bearer ...` header.

**The tools stopped working after a plan change.** MCP is available on Basic, Pro and Ultimate. If the plan ends, keys and connections pause until the account is on a paid plan again.

## Error codes

A failed tool call returns an error result whose text starts with a code, then a sentence saying what to do.

| Code | Meaning |
|---|---|
| `WORDS_PER_REQUEST_EXCEEDED` | The text is longer than the plan's words-per-call limit. Split it and call once per part. |
| `USER_LIMIT_EXCEEDED` | The account does not have enough words left this month for this text. |
| `PLAN_REQUIRED` | The account is on the Free plan. MCP needs Basic, Pro or Ultimate. |
| `SCAN_LIMIT_REACHED` | The month's AI detector scans are used. |
| `TEXT_TOO_SHORT` | `detect_ai` needs at least 20 characters. |
| `RATE_LIMITED` | More than 30 tool calls in a minute. Wait and retry. |
| `TIMEOUT`, `ENGINE_FAILED`, `DETECTOR_FAILED` | Temporary failure. No words were charged. Retry shortly. |
| `USAGE_CHECK_FAILED` | The usage service could not be reached. Retry shortly. |
| `UNAUTHORIZED` | The key was revoked or the app was disconnected. Connect again. |

## Still stuck?

Contact us at <https://ai2human.app/contact> and include the error code. For bugs in the MCP server itself, open an [issue](../../../issues/new/choose).

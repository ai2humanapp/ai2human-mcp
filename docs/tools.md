# Tools reference

The AI2Human MCP server exposes three tools. Each call runs as the account behind the API key or OAuth connection that made it, and the plan limits are enforced on every call.

| Tool | Title | Read-only | Changes data |
|---|---|:---:|---|
| [`humanize_text`](#humanize_text) | Humanize text | No | Uses your monthly words |
| [`detect_ai`](#detect_ai) | Detect AI text | No | Uses one monthly detector scan |
| [`get_usage`](#get_usage) | Get plan and usage | Yes | Nothing |

None of the tools is destructive (`destructiveHint: false`) and none reaches outside AI2Human (`openWorldHint: false`).

## `humanize_text`

Rewrites AI-generated text so it reads as natural human writing, keeping the meaning, facts, figures and citations.

### Parameters

| Name | Type | Required | Description |
|---|---|:---:|---|
| `text` | string | Yes | The text to rewrite. Up to your plan's words-per-call limit. |
| `style` | string | No | One of `standard` (default), `academic`, `formal`, `simple`, `informal`, `expand`, `shorten`, `email`. |

### Output

Two text blocks: the rewritten text, then one status line, for example:

```
[AI2Human <engine>] Words charged: 214. Words remaining: 49786. Our detector scores the result 94% human.
```

### Behaviour

- Charges one word per input word from the monthly allowance, then from any extra word packs.
- Words are charged when the call starts and refunded in full if it fails.
- A call can take up to 90 seconds. Allow up to 120 seconds before treating it as failed.
- Text longer than the plan's words-per-call limit is rejected with `WORDS_PER_REQUEST_EXCEEDED`. Split it at paragraph breaks and call once per part.

## `detect_ai`

Estimates how much of a text reads as AI-generated.

### Parameters

| Name | Type | Required | Description |
|---|---|:---:|---|
| `text` | string | Yes | The text to analyse. At least 20 characters; longer text gives a more reliable result. Up to about 10,000 words. |

### Output

A JSON document containing `aiPercentage`, `humanPercentage`, `wordCount`, the scan counters, and `sentences`, a list where each item has a score and a level.

### Behaviour

Uses one of the account's monthly AI detector scans per call.

## `get_usage`

Shows the account's plan, humanizer words used and remaining this month, the words-per-call limit, and AI detector scans used.

### Parameters

None.

### Behaviour

Free and read-only. Agents should call it before humanizing long text so they can respect `wordsPerRequest` and `wordsRemaining`.

## Notes for AI agents

- Call `get_usage` first and read `wordsPerRequest` and `wordsRemaining`.
- Never send more than `wordsPerRequest` words in one `humanize_text` call. Split on paragraph breaks.
- Send the text exactly as the user wrote it. Do not summarise or pre-edit it.
- Return the humanized text to the user unchanged. Rewriting it again undoes the result.
- On `USER_LIMIT_EXCEEDED` or `SCAN_LIMIT_REACHED`, stop and tell the user. Retrying will not help.

## Errors

A failed tool call returns an error result whose text starts with a code, followed by a sentence saying what to do. See [troubleshooting.md](troubleshooting.md#error-codes) for every code.

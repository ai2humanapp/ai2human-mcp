# Changelog

All notable changes to the AI2Human MCP server are recorded here. The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [1.0.1] - 2026-11-19

### Added

- Server icon (`https://ai2human.app/icon.png`) in the MCP Registry metadata.

## [1.0.0] - 2026-11-19

### Added

- Remote MCP server at `https://ai2human.app/api/mcp` over Streamable HTTP.
- Tools: `humanize_text`, `detect_ai` and `get_usage`, each with MCP annotations.
- Sign-in with OAuth 2.1 (PKCE, dynamic client registration) and with API keys.
- OAuth discovery documents under `/.well-known/`.
- Public documentation and MCP Registry metadata (`server.json`).

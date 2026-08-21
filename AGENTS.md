# Grok-MCP — Base44 dev environment

## What this is
An MCP (Model Context Protocol) server for xAI's Grok API. It is a **tool server**, not a web
app — it normally runs over stdio and is consumed by an MCP client (Claude Desktop, etc.).

## How it runs here
- `docker-compose.base44.yml` runs the server from the cloned source (bind-mounted) using the
  official `uv` image, so edits are reflected after a container restart.
- To make it previewable in a browser, `main()` reads `MCP_TRANSPORT` (defaults to `stdio`,
  preserving original behavior). In compose it is set to `streamable-http` on port 3000.
- A minimal status page is served at `/` (lists registered tools + key status). The MCP
  endpoint itself is at `/mcp`.
- There is no live-reload watcher; restart the service after code edits:
  `docker compose -f docker-compose.base44.yml restart grok-mcp`, then `reload_preview`.

## Secrets
- `XAI_API_KEY` — xAI API key (https://console.x.ai). Required for the tools to actually call
  Grok; the server boots without it. Delivered via `/run/base44/app.env`; placeholder lives in
  `.env.base44-defaults` (first env_file, so the real secret always wins).

## Verify
- `curl -sf -H "Host: external.example" http://localhost:3000/` returns the status HTML.
- `docker compose -f docker-compose.base44.yml logs grok-mcp` shows "Started Grok MCP server".

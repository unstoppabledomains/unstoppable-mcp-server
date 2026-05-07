# Unstoppable Domains MCP Server — Agent Install Reference

This file is for LLM agents and automated tooling. It contains machine-readable install instructions and authentication details for the Unstoppable Domains MCP server.

## Server

```
name: unstoppable-domains
url: https://api.unstoppabledomains.com/mcp/v1/
transport: http (Streamable HTTP)
protocol: MCP (Model Context Protocol)
auth: OAuth 2.0 or API key (Bearer token)
```

## Quick Install Commands

### Claude Code (recommended)

```bash
# Add to current project
claude mcp add --transport http unstoppable-domains https://api.unstoppabledomains.com/mcp/v1

# Add globally (all projects)
claude mcp add --transport http --scope user unstoppable-domains https://api.unstoppabledomains.com/mcp/v1
```

After adding, run `/mcp` inside a Claude Code session and follow the OAuth browser flow to authenticate.

### Claude Desktop (JSON config)

File: `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS)
File: `%APPDATA%\Claude\claude_desktop_config.json` (Windows)

```json
{
  "mcpServers": {
    "unstoppable-domains": {
      "command": "npx",
      "args": ["mcp-remote", "https://api.unstoppabledomains.com/mcp/v1"]
    }
  }
}
```

Requires Node.js. Restart Claude Desktop after saving.

### Generic MCP config (.mcp.json)

```json
{
  "unstoppable-domains": {
    "type": "http",
    "url": "https://api.unstoppabledomains.com/mcp/v1/"
  }
}
```

### API key auth (no OAuth flow)

```json
{
  "unstoppable-domains": {
    "type": "http",
    "url": "https://api.unstoppabledomains.com/mcp/v1/",
    "headers": {
      "Authorization": "Bearer ${UD_MCP_API_KEY}"
    }
  }
}
```

Set `UD_MCP_API_KEY` to a key from [Account Settings → Advanced → MCP API Key](https://unstoppabledomains.com/account/settings?tab=advanced). Keys use the `ud_mcp_*` format.

## Authentication Details

| Method | Format | How to get |
|--------|--------|------------|
| OAuth 2.0 | Bearer `<oauth_token>` | Authorize via browser when prompted by your AI tool |
| API Key | Bearer `ud_mcp_*` | Account Settings → Advanced → MCP API Key |

### OAuth Scopes

| Scope | Description |
|-------|-------------|
| `domains:search` | Search domains, check availability |
| `portfolio:read` | View domains, DNS records, offers |
| `portfolio:write` | Manage DNS, listings, messages |
| `cart:read` | View cart and payment methods |
| `cart:write` | Add/remove cart items |
| `checkout` | Complete purchases |

## Tools Summary

60 tools available. Categories:
- Domain search and TLD listing
- Portfolio and domain detail lookup
- ICANN contact management
- Shopping cart (5 add-to-cart variants by marketplace source)
- Payment methods and checkout
- Marketplace listings and offer management
- Domain conversations (leads/messaging)
- DNS records (CRUD + bulk)
- DNS nameservers (custom and default)
- DNS hosting (redirects, listing pages)
- Domain operations (pending ops, auto-renewal)
- Domain management (tags, flags, push/transfer)
- Backorders
- AI landing pages (generate, status, download, upload, remove)
- DNS presets (save, apply, delete)
- Session handoff (authenticated URL generation)

Full tool reference: [tools.md](tools.md)
Full manifest with annotations: [server.json](server.json)

## Limits

- Bulk operations: max 50 domains per call
- Domain search: max 10 queries, max 5 TLDs per request
- Expireds browse: up to 500 results per call
- Portfolio list: page size up to 500
- Prices are in USD cents (5000 = $50.00)
- DNS changes are async — use `ud_domain_pending_operations` to track propagation

## Support

- Docs: https://docs.unstoppabledomains.com/user-api/mcp-server
- Support: https://support.unstoppabledomains.com
- Email: support@unstoppabledomains.com

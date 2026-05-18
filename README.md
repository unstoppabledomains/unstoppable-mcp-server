<p align="center">
  <img src="images/icon.png" alt="Unstoppable Domains" width="160" height="160" />
</p>

# Unstoppable Domains MCP Server

> Search, register, and manage domain names through natural conversation with AI assistants.

[![MCP](https://img.shields.io/badge/protocol-MCP-blue)](https://modelcontextprotocol.io)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![Tools](https://img.shields.io/badge/tools-60-orange)](tools.md)
[![Auth](https://img.shields.io/badge/auth-OAuth%202.0-purple)](https://unstoppabledomains.com)

The Unstoppable Domains MCP server lets you access the [User API](https://docs.unstoppabledomains.com/user-api/overview) through natural conversation inside ChatGPT, Claude, Cline, or any MCP-compatible agent.

**Endpoint:** `https://api.unstoppabledomains.com/mcp/v1`

---

## Quick Start

### Claude Code

```bash
claude mcp add --transport http unstoppable-domains https://api.unstoppabledomains.com/mcp/v1
```

Then run `/mcp` inside a session and follow the OAuth flow to authenticate.

To make it available across all projects:

```bash
claude mcp add --transport http --scope user unstoppable-domains https://api.unstoppabledomains.com/mcp/v1
```

### Claude Desktop (free plan)

Add to `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS) or `%APPDATA%\Claude\claude_desktop_config.json` (Windows):

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

Restart Claude Desktop and look for the hammer icon in the chat input.

### Claude Desktop / Claude.ai (paid plans)

1. Open **Settings → Connectors → Add custom connector**
2. Paste: `https://api.unstoppabledomains.com/mcp/v1`
3. Click **Add** and authorize with your Unstoppable Domains account

### ChatGPT

1. Open [ChatGPT Connector Settings](https://chatgpt.com/#settings/Connectors/Advanced) and enable **Developer Mode (beta)**
2. Create a new connector with URL: `https://api.unstoppabledomains.com/mcp/v1`
3. Authorize with your Unstoppable Domains account

Or use the pre-built [Unstoppable Domains GPT](https://chatgpt.com/g/g-698a7d3768448191a7177d7f3f22a130-unstoppable-domains) (core features, no MCP config needed).

### Cline / Cursor / other MCP clients

```json
{
  "unstoppable-domains": {
    "type": "http",
    "url": "https://api.unstoppabledomains.com/mcp/v1/"
  }
}
```

---

## What You Can Do

Ask your AI assistant things like:

- *"Is 'mycoolstartup.com' available? What about .ai and .io?"*
- *"Register acme.com and set up Google Workspace DNS"*
- *"List my domains expiring in the next 90 days"*
- *"Create a marketplace listing for mydomain.com at $5,000"*
- *"Show me all DNS records for example.com and add an MX record"*
- *"Generate an AI landing page for parkeddomain.com"*

---

## Authentication

### OAuth 2.0 (Recommended)

When prompted by your AI tool, sign in with your Unstoppable Domains account. OAuth grants only the scopes you approve and can be revoked anytime from [Account Settings](https://unstoppabledomains.com/account/settings?tab=advanced).

| Scope | Access |
|-------|--------|
| `domains:search` | Search domains, check availability |
| `portfolio:read` | View your domains, DNS records, offers |
| `portfolio:write` | Manage DNS, create listings, send messages |
| `cart:read` | View cart and payment methods |
| `cart:write` | Add/remove cart items |
| `checkout` | Complete purchases |

### API Key (Advanced)

Generate a key at [Account Settings → Advanced → MCP API Key](https://unstoppabledomains.com/account/settings?tab=advanced). Keys use the `ud_mcp_*` format.

```bash
export UD_MCP_API_KEY="ud_mcp_your_key_here"
```

Use as a Bearer token: `Authorization: Bearer ud_mcp_your_key_here`

> **Note:** API keys grant full access to all tools. Use OAuth for scoped access.

---

## Tools

60 tools across 16 categories. See [tools.md](tools.md) for the full reference and [tool-annotations.reference.json](tool-annotations.reference.json) for the MCP `ToolAnnotation` reference.

| Category | Tools |
|----------|-------|
| [Domain Search](#domain-search) | `ud_domains_search`, `ud_tld_list`, `ud_expireds_list`, `ud_knowledge_base_search` |
| [Portfolio](#portfolio-management) | `ud_portfolio_list`, `ud_domain_get` |
| [ICANN Contacts](#icann-contacts) | `ud_contacts_list`, `ud_contact_create` |
| [Cart](#cart-management) | `ud_cart_get`, `ud_cart_add_*` (5 types), `ud_cart_remove` |
| [Payment & Checkout](#payment--checkout) | `ud_cart_get_payment_methods`, `ud_cart_checkout`, `ud_cart_get_url`, `ud_cart_add_payment_method_url` |
| [Marketplace Listings](#marketplace-listings) | `ud_listing_create/update/cancel`, `ud_offers_list`, `ud_offer_respond` |
| [Conversations](#domain-conversations) | `ud_leads_list/get`, `ud_lead_messages_list/send` |
| [DNS Records](#dns-records) | `ud_dns_records_list`, `ud_dns_record_add/update/remove`, `ud_dns_records_remove_all` |
| [DNS Nameservers](#dns-nameservers) | `ud_dns_nameservers_list/set_custom/set_default` |
| [DNS Hosting](#dns-hosting) | `ud_dns_hosting_list/add/remove` |
| [Domain Operations](#domain-operations) | `ud_domain_pending_operations`, `ud_domain_auto_renewal_update` |
| [Domain Management](#domain-management) | `ud_domain_tags_add/remove`, `ud_domain_flags_update`, `ud_domain_push` |
| [Backorders](#backorders) | `ud_backorder_create/cancel`, `ud_backorders_list` |
| [AI Landing Pages](#ai-landing-pages) | `ud_domain_generate/status/download/upload/remove_lander`, `ud_ai_credits_get`, `ud_cart_add_ai_credits` |
| [DNS Presets](#dns-presets) | `ud_presets_list/save/delete/apply` |
| [Session Handoff](#session-handoff) | `ud_authenticated_url_get` |

---

## API Reference

| Item | Value |
|------|-------|
| Endpoint | `https://api.unstoppabledomains.com/mcp/v1/` |
| OpenAPI Spec | `https://api.unstoppabledomains.com/mcp/v1/openapi.json` |
| Transport | Streamable HTTP (MCP protocol) |
| Auth | `Authorization: Bearer <oauth_token_or_api_key>` |
| Docs | [docs.unstoppabledomains.com/user-api](https://docs.unstoppabledomains.com/user-api/overview) |

---

## Privacy & Data Practices

**Data collected:** When you use this MCP server, requests are processed by Unstoppable Domains servers. This includes domain names you search, portfolio data you query, DNS records you read or modify, and any purchase or marketplace actions you initiate.

**Data storage:** Unstoppable Domains stores account data, domain registrations, DNS records, and transaction history as required to provide the service. Conversation content from your AI assistant is not stored by Unstoppable Domains — only the API requests your assistant generates are received.

**Data sharing:** Unstoppable Domains does not sell personal data. Registration data for ICANN TLDs (.com, .net, .org, etc.) is shared with the relevant registry per ICANN policy. Payment processing is handled by third-party processors under their own privacy policies.

**Data retention:** Account and domain data is retained for the duration of your account and as required by law. You may request deletion of your account data via [support@unstoppabledomains.com](mailto:support@unstoppabledomains.com).

**Full privacy policy:** [unstoppabledomains.com/privacy-policy](https://unstoppabledomains.com/privacy-policy)

---

## Support

- **Documentation:** [docs.unstoppabledomains.com](https://docs.unstoppabledomains.com/user-api/mcp-server)
- **Support:** [support.unstoppabledomains.com](https://support.unstoppabledomains.com)
- **Email:** [support@unstoppabledomains.com](mailto:support@unstoppabledomains.com)
- **Issues:** [github.com/unstoppabledomains/unstoppable-mcp-server/issues](https://github.com/unstoppabledomains/unstoppable-mcp-server/issues)

---

## License

[MIT](LICENSE) © Unstoppable Domains

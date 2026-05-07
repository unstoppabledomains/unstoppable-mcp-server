# Unstoppable Domains — Complete Tool Reference

All 60 tools available via the MCP server, CLI, and API.

> **Tool annotations** — each tool lists its behavioral hints:
> - `[read-only]` — never modifies state, safe to run without confirmation
> - `[destructive]` — irreversible or high-consequence action
> - `[open-world]` — affects external systems (DNS, marketplace, payments)

---

## Domain Search

| Tool | Hints | Description |
|------|-------|-------------|
| `ud_domains_search` | read-only, open-world | Search for domain availability and pricing across TLDs. Up to 10 queries and 5 TLDs per request (defaults: com, net, org, ai, io). Returns `marketplace.source` and `marketplace.status` to determine which cart tool to use. |
| `ud_tld_list` | read-only | List all available ICANN TLDs supported by the registrar. Call before searching specific TLDs to verify support. |
| `ud_expireds_list` | read-only, open-world | Browse pending-delete / expireds marketplace. Domains spend 5 days in `COMING_SOON`, then drop to `AVAILABLE_BACKORDER`. Filter by status, TLD, label length, watchlist count, backorder count. `limit` up to 500/call. |
| `ud_knowledge_base_search` | read-only | Search the Knowledge Base for help articles by keyword. Returns articles ranked by relevance with titles, snippets, and direct links. No authentication required. |

---

## Portfolio Management

| Tool | Hints | Description |
|------|-------|-------------|
| `ud_portfolio_list` | read-only | List domains with filtering and sorting. Use for portfolio-wide scans, rankings, aggregates. Returns per-domain `offersCount`, `leadsCount`, `watchlistCount`, listing price/status, `listing.views`. `pageSize` up to 500. |
| `ud_domain_get` | read-only | Comprehensive domain info for explicitly named domains (up to 50). Returns lifecycle (renewal, expiration, auto-renewal), flags, DNS config, marketplace metrics, tags, pending operations. |

---

## ICANN Contacts

| Tool | Hints | Description |
|------|-------|-------------|
| `ud_contacts_list` | read-only | List ICANN contacts for DNS domain registration. Check before creating new contacts. |
| `ud_contact_create` | open-world | Create ICANN contact for DNS domain registration. Required before checkout for .com, .org, .net, etc. Suggest user's account email for instant verification. |

---

## Cart Management

| Tool | Hints | Description |
|------|-------|-------------|
| `ud_cart_get` | read-only | Get current shopping cart contents with pricing breakdown. |
| `ud_cart_add_domain_registration` | open-world | Add domains for fresh registration. For `marketplace.source = "unstoppable_domains"` with `marketplace.status = "available"`. Max 50 domains. |
| `ud_cart_add_domain_listed` | open-world | Add UD marketplace domains to cart. For `marketplace.source = "unstoppable_domains"` with `marketplace.status = "registered-listed-for-sale"`. Supports Lease-to-Own. Max 50. |
| `ud_cart_add_domain_afternic` | open-world | Add Afternic marketplace domains to cart. For `marketplace.source = "afternic"`. Max 50. |
| `ud_cart_add_domain_sedo` | open-world | Add Sedo marketplace domains to cart. For `marketplace.source = "sedo"`. Max 50. |
| `ud_cart_add_domain_renewal` | open-world | Add domain renewals to cart. 1–10 years per domain. Max 50. |
| `ud_cart_remove` | — | Remove items from shopping cart. |

---

## Payment & Checkout

| Tool | Hints | Description |
|------|-------|-------------|
| `ud_cart_get_payment_methods` | read-only | Get saved credit cards and account balance for checkout. |
| `ud_cart_add_payment_method_url` | open-world | Get an authenticated magic link URL to add a new payment method (60s, single-use). |
| `ud_cart_checkout` | **destructive**, open-world | Complete checkout. Requires ICANN contact for DNS domains. Uses account balance by default; pass `paymentMethodId` if balance insufficient. |
| `ud_cart_get_url` | read-only, open-world | Generate authenticated magic link checkout URL for browser purchase (60s, single-use). |

---

## Marketplace Listings

| Tool | Hints | Description |
|------|-------|-------------|
| `ud_listing_create` | open-world | Create marketplace listings (max 50). Set price, offer settings, validity, LTO options. Response may include signing magic link. |
| `ud_listing_update` | open-world | Update existing marketplace listings (max 50). May require re-signing. |
| `ud_listing_cancel` | **destructive**, open-world | Cancel marketplace listings (max 50). |
| `ud_offers_list` | read-only | List incoming offers on domains. Filter by status and domain. |
| `ud_offer_respond` | **destructive**, open-world | Accept or reject offers (max 50). Acceptance may require signing. |

---

## Domain Conversations

| Tool | Hints | Description |
|------|-------|-------------|
| `ud_leads_list` | read-only | List conversation threads. Filter by domain. `skipEmpty: true` (default) hides empty threads. |
| `ud_lead_get` | open-world | Get or create a conversation for a domain. Returns existing or creates new. |
| `ud_lead_messages_list` | read-only | Get messages in a conversation (newest-first, cursor pagination). |
| `ud_lead_message_send` | open-world | Send a message in a conversation (max 1000 chars). |

---

## DNS Records

| Tool | Hints | Description |
|------|-------|-------------|
| `ud_dns_records_list` | read-only | List all DNS records for a domain. Filter by type and subName. |
| `ud_dns_record_add` | open-world | Add DNS records (max 50). Supports A, AAAA, CNAME, MX, TXT, NS, SRV, CAA. `upsertMode`: append, replace, disallowed (default). Supports `applyToAllDomainsInPortfolio`. |
| `ud_dns_record_update` | open-world | Update existing DNS record by record ID. Max 50. |
| `ud_dns_record_remove` | **destructive**, open-world | Remove a specific DNS record by ID. Max 50. |
| `ud_dns_records_remove_all` | **destructive**, open-world | Remove ALL DNS records from a domain. **Requires `confirmDeleteAll: true`.** Supports `applyToAllDomainsInPortfolio`. |

---

## DNS Nameservers

| Tool | Hints | Description |
|------|-------|-------------|
| `ud_dns_nameservers_list` | read-only | List nameservers for a domain. Shows UD default vs custom status and DNSSEC info. |
| `ud_dns_nameservers_set_custom` | open-world | Set custom external nameservers (2–12 hostnames). Optional DNSSEC DS records. **Disables** DNS record management. Supports `applyToAllDomainsInPortfolio`. |
| `ud_dns_nameservers_set_default` | **destructive**, open-world | Switch back to UD default nameservers. **Re-enables** DNS record management. Supports `applyToAllDomainsInPortfolio`. |

---

## DNS Hosting

| Tool | Hints | Description |
|------|-------|-------------|
| `ud_dns_hosting_list` | read-only | List hosting configs for a domain (listing pages, redirects). |
| `ud_dns_hosting_add` | open-world | Add hosting config: `LISTING_PAGE`, `REDIRECT_301`, `REDIRECT_302`. `forceCompatibility: true` auto-switches nameservers. Supports `applyToAllDomainsInPortfolio`. |
| `ud_dns_hosting_remove` | **destructive**, open-world | Remove hosting config. `deleteAll: true` with `confirmDeleteAll: true` for all. Supports `applyToAllDomainsInPortfolio`. |

---

## Domain Operations

| Tool | Hints | Description |
|------|-------|-------------|
| `ud_domain_pending_operations` | read-only | List pending operations across domains (up to 50). Use after DNS changes to track propagation. |
| `ud_domain_auto_renewal_update` | — | Enable or disable auto-renewal for ICANN domains. Supports `applyToAllDomainsInPortfolio`. |

---

## Domain Management

| Tool | Hints | Description |
|------|-------|-------------|
| `ud_domain_tags_add` | — | Add tags (up to 10 tags, 20 chars each). Auto-creates new tags. Supports `applyToAllDomainsInPortfolio`. |
| `ud_domain_tags_remove` | — | Remove tags. Idempotent — skips unapplied tags. Supports `applyToAllDomainsInPortfolio`. |
| `ud_domain_flags_update` | **destructive** | Update WHOIS privacy and transfer lock flags. Supports `applyToAllDomainsInPortfolio`. |
| `ud_domain_push` | **destructive**, open-world | Transfer domains to another user by account ID. **Requires MFA (6-digit OTP).** Max 50. |

---

## Backorders

| Tool | Hints | Description |
|------|-------|-------------|
| `ud_backorder_create` | open-world | Create backorders for expiring domains. Requires ICANN contact + availability timestamp. Max 50. |
| `ud_backorders_list` | read-only | List user's backorders with status filtering, search, pagination. |
| `ud_backorder_cancel` | **destructive** | Cancel pending backorders. Returns refund information. |

---

## AI Landing Pages

| Tool | Hints | Description |
|------|-------|-------------|
| `ud_ai_credits_get` | read-only | Check AI credit balance and view available packs. New accounts get 5 free credits. |
| `ud_cart_add_ai_credits` | open-world | Add AI credit pack to cart by `productCode` or `tierSize`. |
| `ud_domain_generate_lander` | open-world | Generate AI-powered landing pages. 1 credit/domain. Custom instructions supported. Async. Supports `applyToAllDomainsInPortfolio`. |
| `ud_domain_lander_status` | read-only | Check generation status: pending, generating, processing, hosted, failed, none. Max 50. |
| `ud_domain_download_lander` | read-only | Download lander content. Returns `htmlContent` (single-page) or `zipContent` (base64 zip). |
| `ud_domain_upload_lander` | open-world | Upload custom lander (HTML or base64 zip). **Requires Domainer Club.** Max 1MB. Supports `applyToAllDomainsInPortfolio`. |
| `ud_domain_remove_lander` | **destructive**, open-world | Remove AI landing page. Supports `applyToAllDomainsInPortfolio`. |

---

## DNS Presets

| Tool | Hints | Description |
|------|-------|-------------|
| `ud_presets_list` | read-only | List saved DNS presets and hardcoded provider presets. |
| `ud_presets_save` | — | Create or update a preset (nameserver, DNS record, forwarding). Upserts by name. |
| `ud_presets_delete` | **destructive** | Delete a saved preset by type and name. |
| `ud_presets_apply` | open-world | Apply a preset to domains. Supports bulk and portfolio-wide. |

---

## Session Handoff

| Tool | Hints | Description |
|------|-------|-------------|
| `ud_authenticated_url_get` | read-only, open-world | Generate one-time authenticated URL for browser redirect. Valid 60 seconds, single-use. |

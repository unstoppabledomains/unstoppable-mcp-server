# Known Issues & Open Items

This is a seed repo. Items below need engineering, legal, or org-admin attention before submitting to any MCP directory.

## Repo location

- [ ] **Transfer to `unstoppabledomains` org.** Currently at `gm-ud/unstoppable-mcp-server`. Submission forms (Anthropic, OpenAI, Cline, glama) expect the canonical org-owned URL.
- [ ] Update `repository.url` in `server.json` after transfer (currently points at `unstoppabledomains/mcp-server` which doesn't exist yet).

## Tool annotations (engineering work)

The MCP spec's `readOnlyHint`, `destructiveHint`, `openWorldHint`, and `title` annotations are returned by the **live MCP server** in its `tools/list` response — they are NOT part of the registry's `server.json`. The registry just points clients at the URL.

- [ ] **Audit `tool-annotations.reference.json`** against actual server-side tool implementations. The annotations in that file were authored by reading tool descriptions, not the source — every entry needs a sanity check from someone with engineering access. Mismatched annotations are the #1 OpenAI rejection cause.
- [ ] **Apply annotations to the live server's `tools/list` response.** Until this is done, the live server returns tools without hints, which means MCP clients can't make informed approval/auto-run decisions.
- [ ] Verify the live tool list matches `tools.md` (60 tools). The dev-docs OpenAPI spec has fewer tools — the OpenAPI spec appears outdated.

## OpenAI ChatGPT Apps blockers (server-side)

- [ ] **Add Content-Security-Policy header to `api.unstoppabledomains.com/mcp/v1/`.** Confirmed missing on POST. OpenAI requires CSP for all submitted MCP servers.
- [ ] **Confirm the project uses global (not EU) data residency** in OpenAI's project settings. EU residency projects cannot submit to ChatGPT Apps.
- [ ] **Complete individual or business verification** in the OpenAI Platform Dashboard (publishers under unverified names are auto-rejected).

## Logo

- [x] Resized to 400×400 (Anthropic requirement). Source images were 279×270; current versions in `images/` are padded to 400×400 (white pad on light, black pad on dark).
- [ ] **Optional:** commission a proper 400×400 logo. The current padded versions are functional but not designed at native 400×400 — the icon sits in a 279×270 region with surrounding pad.

## Test account & screenshots (submission-time)

- [ ] **Provision a test account** (not a fresh sign-up) with sample data: a few owned domains, payment method on file, no MFA on inaccessible numbers. Required by both Anthropic and OpenAI reviewers.
- [ ] **Capture 2–3 screenshots** of the MCP in action — domain search, cart/checkout, DNS edit. Required for both directories.
- [ ] **Document test prompts and expected responses** for the OpenAI submission form (this is reviewed for accuracy).
- [ ] **Write justifications** for each `destructiveHint` and `openWorldHint` tool annotation (OpenAI requires this at submission time).

## Privacy / legal

- [ ] **Legal review of the Privacy & Data Practices section in `README.md`.** The text was synthesized from generic best practices; UD legal should verify accuracy and completeness. Anthropic specifically requires data-collection details in the repo README, not just a privacy policy URL.
- [ ] **Confirm domain checkout flow complies with OpenAI's commerce policy** — all checkout must redirect to UD's own domain. Needs product/legal sign-off.

## npm package (Official MCP Registry)

- [ ] **Publish `@unstoppabledomains/mcp-server`** as an npm package. Required by the Official MCP Registry's `mcp-publisher` CLI flow and by Cline's marketplace.
- [ ] DNS TXT verification on `unstoppabledomains.com` to claim the `com.unstoppabledomains/*` namespace in the registry.

## mcpmarket.com slug

- [ ] **Verify `unstoppable-domains` slug availability** at mcpmarket.com. The default `domain-1` slug is taken by Dynadot. The replacement slug returned a 403 — could be available or claimed-but-unpublished, hasn't been confirmed.

## OAuth flow

- [x] **OAuth URLs verified.** `api.unstoppabledomains.com/.well-known/oauth-authorization-server` returns the correct discovery doc with `authorization_endpoint`, `token_endpoint`, `registration_endpoint`, and `revocation_endpoint`. RFC 9728 protected-resource metadata also implemented at `/.well-known/oauth-protected-resource`.

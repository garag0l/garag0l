# Release drafts: mcp-issuer-check v0.1.0

Repo: https://github.com/garag0l/mcp-issuer-check

---

## Blog post

**Title:** Your MCP server picks where your login goes. Do you know where that is?

When you add a remote MCP server to Claude, Cursor or VS Code, the server tells your client which OAuth authorization server to log you in with. Your client then sends the authorization code, the PKCE verifier and sometimes a client secret or refresh token there.

In 2026 both official MCP SDKs fixed bugs where a malicious MCP server could point clients at an attacker's authorization server, or present a real provider's `issuer` from metadata it controlled (GHSA-6qxp-vccf-f47h, GHSA-qx49-fqc8-xw99). The fixes bind *stored* credentials to an issuer. The first sign-in still trusts whatever the server advertises. The Cloud Security Alliance recommends an allowlist that maps each MCP server to the authorization servers it may name, but there was no tool for it.

**mcp-issuer-check** builds that allowlist from your existing configs:

```bash
pipx install git+https://github.com/garag0l/mcp-issuer-check
mcp-issuer-check          # finds remote servers in Claude Code/Desktop, Cursor, VS Code, Windsurf, Gemini CLI, Codex
mcp-issuer-check pin      # approve what you see
mcp-issuer-check          # later: PIN-CHANGED if a server starts sending you somewhere else
```

It follows the same discovery steps as a real MCP client (RFC 9728, then RFC 8414/OIDC) and checks that:
- the `issuer` matches the authorization server URL,
- the metadata describes the resource you configured,
- everything uses HTTPS,
- no authorization server points at an internal address (an SSRF guard).

It never sends credentials or reads secrets from your configs.

I tested it against 20 public MCP endpoints from 19 providers. The common pattern is a login hosted elsewhere (GitHub, Stripe, Supabase). A few servers publish metadata that doesn't match the URL users configure, and one requires auth but publishes no RFC 9728 metadata at all, so clients fall back to the legacy discovery path.

A GitHub Action runs it on a schedule against a pin file in your repo and fails when a provider changes its login server.

TODO(murat): optional one-line personal angle.

---

## LinkedIn

Every remote MCP server you connect tells your AI client where to send your login: authorization codes, PKCE verifiers, sometimes client secrets.

This year the official MCP SDKs fixed bugs that let a malicious server redirect those credentials. The fix binds stored tokens, but your first sign-in still trusts whatever the server says.

I built mcp-issuer-check (open source, zero dependencies):
• finds the remote MCP servers in Claude, Cursor, VS Code, Windsurf, Gemini CLI and Codex configs
• resolves each login server the way the MCP spec does, and flags issuer impersonation, resource mismatches and plain-HTTP endpoints
• pins what you approve and alerts if a provider changes it (CLI + GitHub Action)

github.com/garag0l/mcp-issuer-check

#MCP #AISecurity #OAuth #AppSec

---

## X

Your MCP server decides which OAuth server gets your login.

mcp-issuer-check reads your Claude/Cursor/VS Code configs, verifies each login server's metadata (issuer, resource, HTTPS), and pins it, so a silent switch gets caught.

Zero deps. CLI + GitHub Action.
github.com/garag0l/mcp-issuer-check

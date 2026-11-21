# Release drafts: nuclei-templates-mcp v0.1.0

Repo: https://github.com/garag0l/nuclei-templates-mcp

---

## Blog post

**Title:** Finding exposed MCP servers with Nuclei (without touching their tools)

MCP servers end up on the internet, on internal networks and on developer laptops, and many answer anyone who asks. Trend Micro counted roughly 1,470 exposed servers this year. Separately, several SDK advisories were about *local* servers that any web page could reach: missing DNS-rebinding protection, and Go SDK servers accepting cross-site `text/plain` requests (CVE-2026-33252).

**nuclei-templates-mcp** adds six MCP-aware checks to Nuclei:

```bash
git clone https://github.com/garag0l/nuclei-templates-mcp
nuclei -t nuclei-templates-mcp/http/mcp/ -u https://mcp.example.com
```

- `mcp-unauthenticated-server`: completes `initialize` with no credentials (extracts name and version)
- `mcp-tools-exposed`: lists tools without credentials, on stateful and stateless servers (extracts tool names)
- `mcp-cross-site-text-plain`: accepts JSON-RPC as `text/plain` from a foreign Origin (CVE-2026-33252 behavior)
- `mcp-dns-rebinding-host-not-validated`: answers for any Host header
- `mcp-sse-endpoint-unauthenticated`: legacy SSE transport open (reads 32 bytes, so endless streams can't hang the scan)
- `mcp-oauth-protected-resource`: RFC 9728 metadata, showing where clients are sent to sign in

They're detection only: the requests are the ones any MCP client sends to start a session, and no tool is ever called.

Testing: CI runs every template against three mock servers (vulnerable, stateless, hardened) and fails on any missing *or* unexpected match. A real-world pass against intentionally public servers (DeepWiki, Cloudflare docs) and OAuth-protected ones (Notion, Linear) gave the expected results, with no false cross-site or rebinding hits. The first real-world run also found a gap, which is now fixed and covered by a test: stateless servers don't issue session IDs, so the tool listing has to work without one.

TODO(murat): optional personal angle from pentest work (no client details).

---

## LinkedIn

There are thousands of MCP servers reachable on the internet, and many answer without credentials.

I published six open-source Nuclei templates that speak MCP properly:
• unauthenticated servers (with name and version)
• tools listed without auth, on stateful AND stateless servers
• CVE-2026-33252 behavior: cross-site text/plain JSON-RPC
• no Host validation (DNS rebinding to local servers)
• legacy SSE transport open
• OAuth metadata: where clients are sent to sign in

Detection only: no tool is ever invoked. CI checks every template against vulnerable, stateless and hardened mock servers.

github.com/garag0l/nuclei-templates-mcp

#Nuclei #MCP #AISecurity #AttackSurface #Pentest

---

## X

6 Nuclei templates for MCP servers: unauth access, exposed tools (stateful + stateless), CVE-2026-33252 cross-site JSON-RPC, DNS rebinding, open SSE, OAuth metadata.

Detection only. CI-tested against vulnerable/stateless/hardened mocks.

github.com/garag0l/nuclei-templates-mcp

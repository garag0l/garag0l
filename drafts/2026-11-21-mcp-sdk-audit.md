# Release drafts: mcp-sdk-audit v0.1.0

Repo: https://github.com/garag0l/mcp-sdk-audit 

---

## Blog post (for the muratbekgi.com session)

**Title:** You patched your MCP SDK. You're probably still exposed.

Since December 2025, the official Model Context Protocol SDKs (TypeScript, Python, Go) have received about 25 security advisories. The usual response is to let Dependabot bump the version and move on.

For several of these advisories, that does nothing. The advisories say so themselves:

- **Client credentials sent to an attacker-chosen auth server** (GHSA-6qxp-vccf-f47h, GHSA-qx49-fqc8-xw99). After upgrading, the bundled `ClientCredentialsProvider` / `ClientCredentialsOAuthProvider` still follows whatever authorization server the MCP server advertises, until you pass `expectedIssuer` / `issuer=`.
- **Tokens issued for other services accepted** (GHSA-rvq5-wwqv-78pq, GHSA-w4fh-qvv9-3v23). The audience check is off by default. "Upgrading alone changes nothing."
- **One client's responses sent to another** (GHSA-345p-7cg4-v4c7, GHSA-jm63-cq47-j5jp). This comes from creating one transport or server at startup and sharing it, a pattern the old README examples showed. Patched SDKs now refuse it at runtime, so it is also an upgrade blocker.
- **Redirect leaks re-enabled** by `redirectPolicy: 'follow'` or an httpx client with `follow_redirects=True`.

Version scanners stop once the version is bumped, and that is exactly when these need a code change.

**mcp-sdk-audit** does both checks in one pass:

```bash
pipx install git+https://github.com/garag0l/mcp-sdk-audit
mcp-sdk-audit ./my-mcp-server
```

It reads lockfiles (npm, pnpm, yarn, bun, uv, poetry, pdm, Pipfile, requirements, go.mod), including transitive dependencies, and matches them against the GitHub Advisory Database. It then runs 11 source rules, each tied to an advisory, so any finding can be confirmed or dismissed in seconds. It has no dependencies, outputs SARIF for GitHub code scanning, and ships as a GitHub Action:

```yaml
- uses: garag0l/mcp-sdk-audit@v0.1.1
  with:
    fail-on: high
```

[example output screenshot: docs/demo.svg]

Before release I tuned the rules against the official typescript-sdk, python-sdk, servers and github-mcp-server repos, cutting false positives from about 150 to a handful. The remaining hits are SDK examples that leave `expectedResource` / `validate_token_resource` unset, which is the pattern the advisories describe.

TODO(murat): optional personal line on why MCP security matters in your testing work (only if you want it; I won't invent one).

---

## LinkedIn

Bumping your MCP SDK version is not enough.

Several 2026 advisories for the official Model Context Protocol SDKs say it outright: "Upgrading alone changes nothing." The OAuth issuer check and the token audience check are both off until you set them, and a server shared across clients can leak one user's responses to another.

Dependabot can't see any of that, so I built a small open-source tool that can.

mcp-sdk-audit:
• finds every MCP SDK version in your lockfiles (npm/pnpm/yarn/bun, uv/poetry/pip, Go) and matches it against GitHub advisories
• flags the code that stays exposed after upgrading (11 rules, each tied to an advisory)
• has no dependencies, outputs SARIF, and ships as a GitHub Action

github.com/garag0l/mcp-sdk-audit

#MCP #AISecurity #AppSec #OpenSource

---

## X

Patched your MCP SDK? Several 2026 advisories say "upgrading alone changes nothing": the issuer and audience checks are off by default, and shared transports leak responses between clients.

mcp-sdk-audit finds the vulnerable versions *and* the code still exposed after upgrading. Zero deps, SARIF, GitHub Action.

github.com/garag0l/mcp-sdk-audit

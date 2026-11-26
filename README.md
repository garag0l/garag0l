### Hi, I'm Murat 👋

Penetration tester focused on offensive security: AWS/cloud, Active Directory, web, API and AI security. GIAC certified.

I build small, sharp open-source tools for security work. Writing at **[muratbekgi.com](https://muratbekgi.com)**.

#### 🔧 Featured

| Project | What it does |
|---|---|
| [**agentcore-iam-audit**](https://github.com/garag0l/agentcore-iam-audit) | Finds Amazon Bedrock AgentCore IAM paths: agents reaching other agents' memory, runtimes, code interpreters and tool credentials, and code-interpreter privilege escalation. |
| [**mcp-sdk-audit**](https://github.com/garag0l/mcp-sdk-audit) | Finds MCP SDK versions with known advisories (TypeScript, Python, Go, Rust, Ruby, PHP, Inspector), and the code that stays exposed after you upgrade. Zero deps, SARIF, GitHub Action. |
| [**semgrep-rules-mcp**](https://github.com/garag0l/semgrep-rules-mcp) | Semgrep rules for MCP servers: tool poisoning, shell injection from tool arguments, and the SDK settings 2026's advisories made mandatory. |
| [**nuclei-templates-mcp**](https://github.com/garag0l/nuclei-templates-mcp) | Nuclei templates for exposed MCP servers: unauthenticated access, tools anyone can call, CVE-2026-33252 cross-site requests, DNS rebinding. |
| [**mcp-issuer-check**](https://github.com/garag0l/mcp-issuer-check) | Shows which OAuth server your MCP servers send your login to, catches issuer impersonation, and alerts when it changes. |
| [**vendor-trust-audit**](https://github.com/garag0l/vendor-trust-audit) | Finds and ranks the AWS roles third-party vendors can assume — dormant trust, ExternalId-only trust, and what secrets each vendor can reach. SARIF, GitHub Action. |
| [**secret-reach**](https://github.com/garag0l/secret-reach) | The exact secret-rotation scope for a compromised AWS principal: which secrets it can read, across accounts and regions. Never reads secret values. |
| [**trust-policy-watch**](https://github.com/garag0l/trust-policy-watch) | Snapshots AWS IAM role trust policies and diffs them to catch `UpdateAssumeRolePolicy` persistence — new external/wildcard principals, dropped ExternalId/MFA. Read-only, SARIF. |
| [**skills-lock**](https://github.com/garag0l/skills-lock) | Pins a lockfile of SHA-256 hashes for your installed Claude Code / Codex agent skills and flags any later tampering or drift. Zero dependencies. |

#### 🇹🇲 Türkmen dili / Turkmen language

| Project | What it does |
|---|---|
| [**turkmen-translit**](https://github.com/garag0l/turkmen-translit) | Transliterate Turkmen between the legacy Cyrillic and the current official Latin alphabet — correctly, including the letters other tools get wrong (Ж→ž vs Җ→j, Й→ý, Ң→ň). Zero-dependency Python + JS library + `tr-tm` CLI. On PyPI and npm. |
| [**turkmen-num2words**](https://github.com/garag0l/turkmen-num2words) | Spell out integers in Turkmen: `1234` → `müň iki ýüz otuz dört`. Grounded in primary sources and verified against a published grammar's worked example. Zero-dependency **Python + JS** library + `tm-num` CLI. |

#### 🛠️ Building now

- a Turkmen transliteration skill and a Turkmen security glossary

<sub>All tools are for authorized testing and defense only.</sub>

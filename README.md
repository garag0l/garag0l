### Hi, I'm Murat 👋

Penetration tester focused on offensive security: AWS/cloud, Active Directory, web, API and AI security. GIAC certified.

I build small, sharp open-source tools for security work. Writing at **[muratbekgi.com](https://muratbekgi.com)**.

#### 🔧 Featured

| Project | What it does |
|---|---|
| [**agentcore-iam-audit**](https://github.com/garag0l/agentcore-iam-audit) | Finds Amazon Bedrock AgentCore IAM paths: agents reaching other agents' memory, runtimes and code interpreters, and code-interpreter privilege escalation. |
| [**mcp-sdk-audit**](https://github.com/garag0l/mcp-sdk-audit) | Finds MCP SDK versions with known advisories, and the code that stays exposed after you upgrade. Zero deps, SARIF, GitHub Action. |
| [**semgrep-rules-mcp**](https://github.com/garag0l/semgrep-rules-mcp) | Semgrep rules for MCP servers: tool poisoning, shell injection from tool arguments, and the SDK settings 2026's advisories made mandatory. |
| [**nuclei-templates-mcp**](https://github.com/garag0l/nuclei-templates-mcp) | Nuclei templates for exposed MCP servers: unauthenticated access, tools anyone can call, CVE-2026-33252 cross-site requests, DNS rebinding. |
| [**mcp-issuer-check**](https://github.com/garag0l/mcp-issuer-check) | Shows which OAuth server your MCP servers send your login to, catches issuer impersonation, and alerts when it changes. |

#### 🇹🇲 Türkmen dili / Turkmen language

| Project | What it does |
|---|---|
| [**turkmen-translit**](https://github.com/garag0l/turkmen-translit) | Transliterate Turkmen between the legacy Cyrillic and the current official Latin alphabet — correctly, including the letters other tools get wrong (Ж→ž vs Җ→j, Й→ý, Ң→ň). Zero-dependency Python library + `tr-tm` CLI. |

#### 🛠️ Building now

- turkmen-translit JS/TS port, a Turkmen transliteration skill and a Turkmen security glossary
- skills-lock: hash-pinning and change alerts for installed Claude/Codex agent skills
- AWS trust-policy drift detection (`UpdateAssumeRolePolicy` persistence)

<sub>All tools are for authorized testing and defense only.</sub>

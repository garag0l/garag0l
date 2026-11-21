# Release drafts: agentcore-iam-audit v0.1.0

Repo: https://github.com/garag0l/agentcore-iam-audit

---

## Blog post

**Title:** Can your AgentCore agents read each other's memory? A 30-second check

Amazon Bedrock AgentCore gives every agent an execution role. Two pieces of 2025–26 research show how those roles go wrong:

- **Agent "God Mode" (Unit 42, April 2026).** The starter toolkit's auto-created roles granted `memory/*`, `runtime/*`, `code-interpreter/*` and `repository/*`. Any agent could pull another agent's image, find its memory ID, then read or poison that agent's memory and invoke other agents. AWS said the default roles are for development and testing only. The toolkit template was scoped down on 2025-12-09, but roles created before then are still in accounts.
- **Code-interpreter privilege escalation (Sonrai, 2025).** With `iam:PassRole` + `CreateCodeInterpreter` + `InvokeCodeInterpreter`, a principal runs code as the interpreter's role, and CloudTrail attributes it to the interpreter.

While building the tool I noticed that AWS's managed `BedrockAgentCoreFullAccess` (v21) grants `iam:PassRole` on every role whose name contains `BedrockAgentCore`. So anyone holding it can escalate to any such role, including the toolkit defaults, or an admin role someone happened to name that way. (This is documented policy behavior, not a vulnerability. The lesson is how you name roles and who gets the managed policy.)

**agentcore-iam-audit** checks all of this, offline, from an IAM export:

```bash
aws iam get-account-authorization-details > gaad.json
agentcore-iam-audit --gaad gaad.json
```

Instead of grepping for `*`, it asks a concrete question for each role: can it reach a resource that belongs to a *different* agent? It evaluates Allow/Deny, NotAction/NotResource and permission boundaries against probe ARNs. There's also a live mode (inventory of code interpreters, runtimes and browsers, including PUBLIC-network interpreters with roles), and a `--policy` mode to check a role before you deploy it.

Testing used the real documents: the toolkit's execution-role template from before and after the fix, its trust policy, and the AWS managed policy. The old role produces every Unit 42 path. The current one doesn't, though it still lets every agent role read all AgentCore Identity credential-provider secrets in the default token vault.

TODO(murat): optional personal angle (no client details).

---

## LinkedIn

If you deployed Amazon Bedrock AgentCore agents with the starter toolkit before December 2025, check their roles.

Unit 42 showed those default roles let any agent read or poison every other agent's memory and invoke other agents. AWS's managed BedrockAgentCoreFullAccess also lets holders pass any role named *BedrockAgentCore* to a code interpreter and run code as it.

I published agentcore-iam-audit (open source, zero dependencies offline):
• cross-agent memory / runtime / code-interpreter / image / secret access
• code-interpreter privilege escalation paths, flagged high when the target role is admin-equivalent
• offline from an IAM export, live (read-only), or pre-deploy on a single policy

Tested on the real toolkit and AWS managed policy documents.

github.com/garag0l/agentcore-iam-audit

#AWS #CloudSecurity #AISecurity #Bedrock #IAM

---

## X

AgentCore agents on starter-toolkit roles (pre-Dec 2025) can read/poison each other's memory (Unit 42 "God Mode"). BedrockAgentCoreFullAccess holders can run code as any *BedrockAgentCore* role via code interpreters.

agentcore-iam-audit finds both, offline from an IAM export.
github.com/garag0l/agentcore-iam-audit

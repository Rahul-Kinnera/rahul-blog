# MCPS: The Case For a Cryptographic Trust Layer in the Model Context Protocol

*A thought piece for engineers and security researchers on why MCP's current security model is structurally insufficient — and what a PKI-backed alternative might actually look like.*

---

## The USB-C Analogy Has a Security Problem

When Anthropic introduced the Model Context Protocol in November 2024, it was pitched, accurately, as the "USB-C of AI agents" — a universal connector between large language models and external tools, APIs, and data sources. The analogy caught on fast. Within months, the ecosystem exploded: thousands of MCP servers, open-source integrations, hosted registries, and enterprise pilots across Microsoft, GitHub, and major cloud vendors.

But here's the thing about USB-C: when you plug in a rogue charging cable, it can silently fry your laptop. The connector's universality is also its vulnerability surface.

MCP has the same problem, at a much larger scale — and the security track record of its first year makes this hard to ignore.

---

## What the First Year Actually Looked Like

The MCP ecosystem accumulated a concerning security history in a remarkably short time.

Researchers at Ox Security disclosed a critical architectural flaw baked into the official MCP SDKs across Python, TypeScript, Java, and Rust — a design decision in the STDIO interface that enables arbitrary command execution without any sanitization guardrails. When they brought it to Anthropic, the response was that this was "expected behavior" and that sanitization is the developer's responsibility. The vulnerability affects over 150 million downloads and more than 7,000 publicly accessible servers.

Separately, Anthropic's own `mcp-server-git` package shipped with three chained vulnerabilities — a path validation bypass, an unrestricted `git_init` call, and argument injection in `git_diff` — that combined to achieve full remote code execution. A GitHub MCP incident demonstrated how prompt injection through issue content caused an agent to exfiltrate private repository data, including salary information, into a public pull request. Asana disclosed a cross-tenant data leak in their MCP integration. JFrog found a critical OS command injection in `mcp-remote`, a widely used OAuth proxy.

Academic analysis of 67,057 MCP servers across public registries found that a substantial number could be hijacked due to the absence of any vetted submission process. Nine out of eleven MCP registries were successfully poisoned with a malicious test payload in a single research exercise.

The pattern across all of these is not novel. As one security team put it: "The breaches detailed here are rooted in timeless flaws — over-privilege, inadequate input validation, and insufficient isolation. AI fundamentally changes the interface, but not the fundamentals of security."

This is precisely the point. The protocol is new. The security failures are old. And the current authorization model is not equipped to prevent them at scale.

---

## The Authorization Problem Is Structural

MCP added OAuth 2.1-based authorization to its spec in mid-2025. This was a meaningful step. But the implementation has exposed a deep tension between what the spec assumes and what enterprise environments actually look like.

The core issue: the original MCP spec treated an MCP server as simultaneously a resource server *and* an authorization server. This is architecturally inverted from how every major enterprise runs its identity infrastructure. Enterprises have dedicated identity providers — Okta, Auth0, Keycloak — and they treat backend systems as stateless resource servers that validate tokens, not issue them. Making every MCP server responsible for its own token issuance means making it stateful, difficult to scale, and isolated from the enterprise IdP that already governs all other access.

Aaron Parecki, co-author of the OAuth 2.1 spec, was among those who flagged this early. Dick Hardt, another OAuth 2.1 co-author, called the original approach "a novel usage of OAuth that differs significantly from traditional patterns." The November 2025 spec revision addressed some of these concerns by introducing Client ID Metadata Documents (CIMD) and an Enterprise-Managed Authorization extension. As of early 2026, however, fewer than 4% of MCP authorization servers had adopted CIMD in practice.

The spec is improving. The ecosystem has not caught up. And even when it does, OAuth alone cannot solve the deeper problem: **there is no way for an MCP server to cryptographically verify who it is talking to, who originally authorized the connection, or whether the agent on the other end is operating within the bounds its human principal intended.**

OAuth handles authorization delegation. It does not handle trust establishment. Those are different problems.

---

## What Trust Establishment Actually Requires

Consider the principal chain in any real agentic workflow:

```
Human → Application → Agent Framework → MCP Client → MCP Server → Tool
```

OAuth tells you that someone with a valid token was authorized to call this endpoint. It does not tell you:

- Whether the MCP server you're connecting to is the genuine server and not a rogue impersonator
- Whether the agent framework that spawned this client is a legitimate, audited platform
- Whether the human who originally triggered the chain actually intended this specific tool call with this specific scope
- Whether the agent has been compromised mid-session since the token was issued

These are trust questions, not authorization questions. And trust establishment is exactly what Public Key Infrastructure was designed for.

---

## The MCPS Proposal: A PKI Trust Layer for MCP

The concept is structurally analogous to how HTTPS extended HTTP: you keep the underlying protocol, and you add a cryptographic trust layer above it. Call it MCPS — MCP Secure.

### The CA Hierarchy

An MCP Certificate Authority would issue long-lived certificates to three classes of entities:

**Server certificates** bind a verified identity to a specific MCP server. A Stripe MCP server cert would encode not just the domain, but the set of tools it is permitted to expose (`payments.create`, `customers.read`), its audit log endpoint, and its spec version. A client connecting to `stripe-mcp.stripe.com` can verify, before any tool call, that it is talking to the genuine Stripe server and not a poisoned registry entry.

**Platform certificates** certify agent frameworks — Claude, LangChain, VS Code Copilot, and others. These answer the question: "Is this a known, audited platform that follows responsible agent practices?" Platform certs would be issued to the framework vendor, not to individual deployments.

**Human identity certificates** bind a verified user identity to a public key. These are analogous to client certificates in mTLS, but scoped to the MCP ecosystem. They encode the maximum scope a human is permitted to authorize — you cannot request a tool that your identity cert does not permit.

### The Delegated Agent Credential

This is where the architecture gets interesting, and where it answers the hardest question you can ask about agent security: *do you certify the agent, or does the agent carry the human's signature?*

The answer is: neither alone. You mint a **Delegated Agent Credential (DAC)** — a short-lived, cryptographically signed token that chains all three together at the moment of agent invocation:

```json
{
  "version": "1.0",
  "issued_at": 1745000000,
  "expires_at": 1745000900,
  "agent_instance_id": "uuid-xyz",
  "human_cert": "<CA-signed cert for alice@company.com>",
  "platform_cert": "<CA-signed cert for claude.anthropic.com>",
  "scope": ["stripe.payments.read", "github.issues.write"],
  "target_server": "stripe-mcp.stripe.com",
  "signature": "<signed by alice's private key over all above fields>"
}
```

The human's private key signs the entire DAC. This is the non-repudiation anchor: the agent cannot forge or modify it without the human's key, which never leaves the human's device or HSM. The 15-minute TTL limits blast radius if the credential is intercepted mid-session. The `agent_instance_id` enables per-invocation audit trails without requiring a cert for every ephemeral agent instance.

### The mTLS Handshake

The connection itself upgrades to mutual TLS, with the DAC carried as a TLS extension:

1. The client presents its CA-signed cert and the DAC
2. The server presents its CA-signed server cert
3. The client verifies the server cert against the MCP CA (preventing rogue servers)
4. The server validates the DAC: Is the human cert CA-signed? Is the platform cert CA-signed? Does the human's signature over the DAC verify? Is the requested scope a subset of what the server cert permits? Has the credential expired or been revoked?
5. Connection is established. Every subsequent tool call is cryptographically scoped to `DAC.scope`

Every tool call emits a signed audit log entry referencing the DAC hash. The chain from human authorization to specific tool invocation becomes fully non-repudiable.

---

## The Pros: What This Solves

**Rogue server prevention.** Today, nothing stops a malicious actor from publishing an MCP server that impersonates a legitimate service. Nine out of eleven public registries were successfully poisoned in research. With server certificates issued by an MCP CA, the client has cryptographic proof of server identity before any data is exchanged. Tool poisoning — hiding malicious instructions inside server-provided tool descriptions — becomes much harder when you can verify the server is who it claims to be.

**Agent accountability.** The DAC model gives you a complete audit chain: which human authorized the session, which platform spawned the agent, which specific agent instance made which tool call, and what scope it was operating under. This is the accountability primitive that regulated industries will eventually require before allowing autonomous agents to touch production systems.

**Scope enforcement at the protocol level.** Currently, scope is an application-level concern — servers ask for it, clients may or may not honor it, and nothing enforces it cryptographically. With scope embedded in both the human cert (maximum permitted scope) and the DAC (actual requested scope), the server can enforce it at the connection layer, before any tool call logic executes.

**Short-lived credentials reduce blast radius.** OAuth tokens can be long-lived. A DAC with a 15-minute TTL means a compromised credential has a hard expiration that cannot be extended without re-signing with the human's private key. Combined with revocation via CRL or OCSP, this substantially reduces the window of exposure for any compromised session.

**Separation of authentication from authorization.** The CA handles identity. The DAC handles delegation. The MCP server validates both without being the authority for either. This is how every mature enterprise security architecture works, and it is how MCP should work too.

---

## The Cons: What Makes This Hard

**Key management for humans at scale.** Where does Alice's private key live? For enterprise users, an HSM or device-bound key (via WebAuthn/passkeys) is the right answer. For consumer-facing agents, it requires infrastructure that simply does not exist yet for most platforms. The UX problem of "users managing cryptographic keys" is unsolved, and every attempt to hide it (managed key services, delegation to the platform) introduces new trust assumptions.

**CA governance is a political problem, not a technical one.** Who decides which MCP servers are eligible for certification? Who adjudicates disputes? Who controls revocation? The technical mechanisms — X.509, CRLs, OCSP — are well understood. The governance layer is not. HTTPS succeeded because browser vendors enforced it, and Let's Encrypt solved the UX problem of certificate acquisition. MCP needs analogous pressure and tooling. That requires either a standards body (think IETF or a new consortium), a dominant market actor willing to enforce it (Anthropic, Microsoft, or a browser-equivalent), or regulatory mandate. None of these are imminent.

**Certificate lifecycle at AI agent scale.** This is perhaps the deepest technical challenge. Traditional PKI was designed for humans, servers, and IoT devices — entities that have stable identities and lifecycles measured in months or years. AI agents can spawn thousands of instances per second, act for minutes, and disappear. As Keyfactor's CTO noted, "Autonomous AI introduces characteristics that stress traditional PKI deployments. Agents are dynamic, short-lived, massively parallel, and capable of acting independently across environments." Issuing and revoking a certificate for every ephemeral agent instance is not feasible without automation infrastructure (SPIFFE/SPIRE being one model) that most organizations have not deployed. The DAC model sidesteps some of this by not requiring per-agent certs, but the platform and human cert lifecycle still needs robust automation.

**The scope negotiation problem.** What happens when an agent discovers mid-task that it needs a scope it was not originally granted? The current MCP spec has a step-up authorization flow for this. A DAC-based model would require re-prompting the human to sign a new DAC with expanded scope — which is correct from a security standpoint but creates friction in long-running agentic workflows that make many independent decisions. The UX of continuous authorization for autonomous agents is an open research problem.

**Fragmentation risk.** The MCP ecosystem is already fragmented across transport models (STDIO vs HTTP/SSE), authorization implementations, and server registries. Adding a CA layer risks further fragmentation: private enterprise CAs that are incompatible with public ones, competing CA consortia, or a de facto CA controlled by a single vendor with the market power to enforce adoption. The HTTPS ecosystem took 25 years to reach near-universal adoption. MCP is moving at AI timescales.

---

## What's Already Happening in the Industry

This is not a purely speculative proposal. The components are being assembled from multiple directions simultaneously.

The IETF's Agent Name Service (ANS) draft (draft-narajala-ans) proposes a PKI-backed directory for AI agent identity — essentially a DNS for agents, backed by a verifiable certificate chain. DigiCert has announced a Managed Credential Provider product that issues verifiable identity credentials to autonomous AI agents, binding trust policies and revocation to dynamic agent instances. Keyfactor has deployed X.509 certificate issuance for AI agents combined with mTLS and certificate-based OAuth flows at enterprise scale. HID's 2026 market survey found that 16% of enterprises are already issuing certificates for AI agents, with a third of organizations ranking it among their top three PKI priorities.

NSA and CISA jointly published guidance in December 2025 explicitly calling for strong identity and continuous verification for AI in operational technology environments. DigiCert's 2026 security predictions named MCP as part of the governance backbone for AI model provenance and authenticity.

The industry is converging on the conclusion that OAuth alone is insufficient and that cryptographic identity — specifically X.509 certificates with mTLS — is the right foundation. The question is not *whether* a trust layer will be added to MCP, but *who will define it, who will govern it, and how long ecosystem fragmentation will persist in the meantime*.

---

## The Open Questions Worth Arguing About

Any serious thought piece should leave you with arguments, not just conclusions. Here are the unresolved tensions worth debating:

**Certify the agent or carry the human's signature?** The DAC model proposed here does both — the agent instance is identified by a UUID inside a human-signed credential. But there's a legitimate argument for per-agent certs: they give servers a first-class revocation target, and they make agent identity independent of human key availability. The counter-argument is that per-agent cert issuance at scale is operationally infeasible without infrastructure most organizations lack. Which model wins probably depends on the deployment context — enterprise vs. consumer, long-running vs. ephemeral.

**Should the MCP CA be public or federated?** A single public CA (like Let's Encrypt for HTTPS) is simpler but creates a single point of governance and failure. A federated model (like OpenID Federation, where trust anchors can delegate to sub-authorities) is more resilient but harder to bootstrap. Enterprises will likely want their own private CAs that interoperate with a public root. Getting that interoperability right is the hard problem.

**Is this the right layer to solve tool poisoning?** Server certificates verify identity, not behavior. A CA-certified Stripe server can still ship malicious tool descriptions. Cryptographic identity prevents *impersonation*; it does not prevent a legitimate but compromised server from doing something malicious. Behavioral attestation — something like a code-signed tool manifest that commits a server to specific tool behavior — is a separate problem that probably requires a different mechanism (content signing, reproducible builds) layered on top of identity.

**What happens to STDIO?** The entire DAC and mTLS model assumes HTTP transport. The STDIO transport, which is dominant for local MCP deployments, has a fundamentally different threat model — and the Ox Security vulnerability lives precisely there. MCPS as described here does not solve the local STDIO problem. That probably requires a different approach: process isolation, sandboxing, and manifest-only execution at the SDK level.

---

## The Broader Point

HTTP was designed in 1991 for a world of static documents. HTTPS was bolted on later, and the web spent fifteen years fighting to make it the default. We are in that same early window for MCP — the moment where the protocol is simple, adoption is accelerating, and security is still considered an optional layer for those who care about it.

The cost of not acting is already visible: supply chain compromises across 150 million downloads, cross-tenant data leaks, credential exfiltration through legitimate tool channels, and a governance vacuum that leaves server trustworthiness entirely up to individual developer discretion.

The cost of acting wrong — of entrenching a CA model that fragments the ecosystem, concentrates governance power in a single vendor, or imposes certificate overhead that kills developer velocity — is equally real.

What the MCP ecosystem needs is what the web eventually got: a trust layer that is open, automated, auditable, and backed by governance that no single party controls. The technical primitives — X.509, mTLS, short-lived credentials, signed delegation chains — exist today. The hard work is the standardization, the governance design, and the tooling that makes it invisible to the developer building the next MCP server at 2am.

That work is worth doing. And the window to do it before the ecosystem calcifies around insecure defaults is closing faster than most people realize.

---

*Thanks for reading. If you're working on MCP security, agent identity standards, or PKI infrastructure for agentic systems, I'd like to hear from you.*

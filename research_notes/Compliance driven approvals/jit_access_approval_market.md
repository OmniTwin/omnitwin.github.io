# Slack-based JIT access and change approval for startups, and the extension to AI agents (as of 16 Oct 2026)

Hypothesis tested: startups have paid self-serve for "request access or approval in Slack, approve, auto-expire, export evidence for the auditor". AI agents are a new kind of requester, which would create a pull-driven niche: "just-in-time production access and action approval for AI agents, with SOC 2 evidence".

**Method and source quality.** 59 web searches. WebFetch could not reach any site from this container (DNS failures, and the egress proxy returned 403), so I could not open any page directly. Every finding below comes from search-engine summaries of the cited URLs. Many of those URLs are primary sources (vendor blogs, pricing pages, press releases), but I read the summaries, not the pages. Wherever a fact comes only from an aggregator (toolradar, startupintros, yespress, nhigovernance and similar), it is marked **[aggregator]**. Wherever a fact comes from training knowledge rather than a search this session, it is marked **[prior knowledge, unverified]**.

---

## Bottom line

- **The startup-friendly pure plays all died, and none sold as a company.** Indent shut down in July 2024 and its team joined OpenAI. Sym shut down in April 2024. Common Fate wound down in April 2025. Abbey Labs has published nothing since 2023. Each had raised about $3M to $14M and charged roughly $2 to $20 per user per month, or offered a free tier.
- **The survivors went enterprise or were acquired by identity and PAM incumbents.**
  - Enterprise survivors: Opal ($59M raised), ConductorOne/C1 ($100M+ raised), Lumos ($27k/yr AWS Marketplace list price).
  - Acquired: Entitle by BeyondTrust (2024), StrongDM by Delinea (2026), Apono by 1Password (2026, reported >$200M), Astrix by Cisco (2026), Natoma by Snowflake (2026).
- **"Approval for AI agents" is now crowded at both ends.**
  - Enterprise: Opal Zero, P0, Apono/1Password, StrongDM/Delinea, Britive, Teleport, Oasis, Drata AI Agent Governance.
  - Free or cheap: Preloop (open source, $29/user/mo hosted), VantaGate ($0/$49/$199 per month with signed decisions), hoop.dev, Bifrost, TrueFoundry, and framework-level human-in-the-loop (HITL) in Agno and similar tools.
- **The remaining self-serve gap is narrow.** It is the bundle, not any one feature: one-time, subject-bound grant + named approver in Slack + tamper-evident signed receipt + push into Vanta/Drata, at $50 to $300/month. Drata is closing that gap from above with a "tamper-evident evidence feed" for agent actions, currently enterprise-only and in limited availability.
- **Verdict: weak as a standalone venture-scale niche, plausible as a cheap wedge or feature.** History says startups pay for this mainly around audit time, at low prices. The SOC 2 "evidence" angle alone did not sustain Indent, Sym or Common Fate.

---

## Q1. Vendor-by-vendor: what they sold, startup tier and price, traction, outcome

### Takeaway
Every startup-priced Slack approval tool (Indent, Sym, Common Fate, Abbey) is gone or dormant. The companies that moved upmarket into identity governance (IGA) and privileged access (PAM) raised large rounds or were acquired at $200M to $400M. Pricing pages are now sales-led ("contact us") almost everywhere.

### Cited findings

| Vendor | What it sold | Self-serve / startup tier and price | Traction | Outcome |
|---|---|---|---|---|
| **Indent** | `/access` in Slack: request, approve, auto-expire; "two-person approval" on access and infrastructure changes | Free: $0, unlimited users, 100 requests/mo, 30-day audit logs. Paid: "$8 per person". G2: Professional $160/mo for 20 users. | Seed ~$6M (Dec 2021), ~$14M total [aggregator]; Sam Altman was in the 2021 seed. Modern Treasury: median approval wait fell from 3 days to 59 s. | Shutdown announced May 2024; service ended **15 Jul 2024**. Team joined OpenAI (acqui-hire style, no deal disclosed). Crunchbase: permanently closed. |
| **Sym** | Slack-native JIT access workflows; policy-as-code (Python SDK, Terraform); Okta, AWS and Tailscale integrations | No reliable primary price found. Toolradar's $0/$10/$25 tiers look templated and are not credible. | $3M seed (Uncork, Mango) plus $9M Series A (Oct 2020, Amplify); Show HN in 2023; about 4 G2 reviews | Announced it would shut down **in April 2024** (Tailscale docs). Repos archived. No acquirer found. |
| **Common Fate** | JIT cloud access (AWS, GCP, Okta groups); open-source Granted CLI; open-source Glide (archived May 2024) | Published price: **$1,980/yr minimum**; modules $2 to $8 per user/mo; 50 users ≈ $3.30/user; 30-day trial | $3.1M seed; Temporal case study (2023) | **Wound down 15 Apr 2025**. Granted donated to fwd:cloudsec. Co-founder Chris Norman's LinkedIn now shows Anthropic (snippet). |
| **Abbey Labs** | Access requests and approvals through Terraform/IaC ("access governance for engineers") | 2023 beta: **free under 20 users, then $20/user/mo** | $5.25M seed (Aug 2023, Point72); HashiCorp partner; 0 G2 reviews | No news since 2023. AWS Marketplace listing still live. Likely dormant (inference). |
| **Opal Security** | Access governance and JIT; Slack requests; compliance-readiness pitch | No public pricing (G2, AWS private offers). Opal Zero reported at **$30,000 launch price** [aggregator, unverified]. | $23M (4 Jun 2026; Greylock, Battery), **$59M total**. Customers: Databricks (~86k JIT requests), Notion, Cloudflare, Scale AI, CoreWeave. | Independent, enterprise-focused. New CEO (Howard Ting) late 2025. Paladin (Mar 2026); **Opal Zero for AI agents (16 to 17 Sep 2026)**. |
| **ConductorOne (C1)** | IGA, IAM and PAM; self-serve requests; JIT; access reviews | No public pricing (G2) | $79M Series B (Oct 2025, Greycroft), $100M to $111M total; 101 employees at raise, 142 by May 2026 [aggregator]. Customers: Zscaler, Ramp, DoorDash. | Independent, enterprise. Rebranded C1 (c1.ai). Positions on "human, non-human and AI identities". |
| **Apono** | JIT, "just-enough" cloud and data access; intent-based access for agents | No pricing found | $34M Series B (18 Nov 2025, USVP), >$54M total. Customers: Intel, HPE, Workday. | **Acquired by 1Password, announced 15 Jun 2026.** Terms undisclosed; investor Vertex says >$200M; Calcalist reported $250M to $300M. |
| **P0 Security** | JIT and short-lived cloud access; machine identities; agent privileged-access control (Dec 2025) | Custom pricing [aggregator] | $5M seed plus $15M Series A, $20M total | Independent. Renamed its platform "Runtime Access Platform" in 2026 [aggregator]. |
| **Entitle** | JIT access and cloud IGA; self-serve requests; 150+ integrations | Not found | Not found | **Acquired by BeyondTrust (16 Apr 2024)**, terms undisclosed |
| **Lumos** | Identity governance; Albus AI; "Identity Agent Force" (6 agents, Jun 2026) | Sales-led. **AWS Marketplace: Lumos Core Platform $27,000 for 12 months**, extra users billed separately. | $35M Series B (May 2024, Scale VP); $85M+ total [aggregator]. Customers: Mars, Netskope, GitLab, MongoDB. | Independent, mid-market and enterprise |
| *Teleport* | Infrastructure access with access requests; Agentic Identity Framework (Jan 2026); Beams agent runtime (Mar 2026, public beta 16 Jun 2026) | Community Edition free for companies with <100 employees and <$10M revenue (since v16, Mar 2024). Enterprise pricing guide (Jun 2026). | n/a | Independent |
| *StrongDM* | JIT access for engineers and **AI agents**; approvals in Slack/Teams | Not found | $34M Series C (2024) | **Acquired by Delinea, closed 5 Mar 2026** |
| *Britive* | Cloud JIT and zero standing privilege; ARC agent runtime authorization (GA 24 Aug 2026 [aggregator]) | Not found | ~$36M total | Independent |

**Sources**
- Indent
  - Product: https://indent.com/slack · https://www.crunchbase.com/organization/indent
  - Pricing: https://indent.com/free · https://indent.com/pricing · https://g2.com/products/indent/pricing
  - Customer story: https://indent.com/customers/modern-treasury
  - Funding: https://startupintros.com/news/2021-12-01-indent-seed [aggregator]
  - Shutdown: https://news.ycombinator.com/item?id=40369004 · https://twitter.com/btibor91/status/1794449804912726402 · https://m.huxiu.com/article/3074622.html
  - Anomaly: docs.indent.com now describes a credit-based Pro plan ($50/mo for 500 credits) and a different Slack "session" product. The domain may have been repurposed. Unverified: https://docs.indent.com/help_and_resources/pricing
- Sym
  - Shutdown: https://tailscale.com/kb/1206/jit-access-sym
  - Repos archived: https://apis.io/providers/sym/
  - Funding: https://www.venturecapitaljournal.com/sym-fetches-9m-series-a/ · https://fintech.global/2020/10/21/workflow-security-developer-sym-bags-3m-series-a-shortly-after-closing-its-seed-round/
  - G2 reviews: https://g2.com/products/sym/reviews/sym-review-7933618
  - Show HN: https://news.ycombinator.com/item?id=35468981
- Common Fate
  - Wind-down: https://www.commonfate.io/blog/winding-down
  - Pricing: https://commonfate.io/pricing
  - Seed: https://www.commonfate.io/blog/announcing-our-3-1M-seed-raise
  - Glide sunset: https://www.commonfate.io/blog/sunsetting-glide
  - Temporal case study: https://temporal.io/blog/rolling-out-access-hours-at-temporal
  - Chris Norman's profile: https://www.linkedin.com/in/chrnorm/
- Abbey Labs
  - Beta launch and pricing: https://www.businesswire.com/news/home/20230807016151/en/Abbey-Labs-Launches-Public-Beta-and-Announces-HashiCorp-Partnership
  - Seed: https://siliconangle.com/2023/08/01/abbey-labs-secures-5-25m-identity-access-management-innovation/
- Opal
  - Funding: https://www.businesswire.com/news/home/20260604227625/en/Opal-Security-Raises-$23M-and-Expands-Leadership-Team-to-Unify-Identity-Governance-Across-Human-Non-Human-and-Agentic-AI
  - Opal Zero: https://www.ciodive.com/press-release/20260916-opal-security-launches-opal-zero-to-make-least-privilege-a-reality-for-ente-1/ · https://www.progressiverobot.com/tag/opal-zero/ [aggregator, source of the $30k figure]
  - Pricing: https://www.g2.com/products/opalsecurity/pricing
- ConductorOne/C1
  - Series B: https://www.c1.ai/news/press-release/conductorone-raises-79-million-series-b · https://www.securityweek.com/conductorone-raises-79-million-in-series-b-funding/
  - Tracxn profile: https://tracxn.com/d/companies/conductorone/__3YBPv17aTFWkY6TAd5enRAuE57G-ibWi1g2XZ2niuDs [aggregator]
- Apono
  - Series B: https://www.finsmes.com/2025/11/apono-raises-34m-in-series-b-funding.html
  - Acquisition: https://1password.com/press/2026/june/1password-acquires-apono · https://vertexholdings.com/news/1password-acquires-apono-for-over-us-200-million/ · https://thenextweb.com/news/1password-acquires-apono-ai-agent-access-governance
- P0
  - Series A: https://p0.dev/blog/15m-series-a/
  - Agent access control: https://p0.dev/blog/more-than-visibility-p0-introduces-privileged-access-control-for-agents/
  - Profile: https://nhigovernance.com/companies/p0-security.html [aggregator]
- Entitle: https://www.beyondtrust.com/press/beyondtrust-acquires-entitle
- Lumos
  - Series B: https://techcrunch.com/2024/05/23/lumos-helps-companies-manage-their-employees-identities-and-access/
  - AWS Marketplace: https://aws.amazon.com/marketplace/pp/prodview-g7fmrspbd3c2i
  - Profile: https://yespress.io/lumos [aggregator]
- Teleport
  - Community license: https://goteleport.com/blog/march-newsletter-2024/
  - Agentic Identity Framework: https://www.infoq.com/news/2026/02/teleport-secure-ai-agents/
  - Beams: https://www.globenewswire.com/news-release/2026/03/19/3258943/0/en/Teleport-Launches-Beams-Trusted-Agent-Runtimes-For-Infrastructure.html
  - Enterprise pricing guide: https://goteleport.com/api/files/teleport-pricing-guide.pdf
- StrongDM: https://www.globenewswire.com/news-release/2026/03/05/3250113/0/en/delinea-completes-strongdm-acquisition-to-secure-ai-agents-with-continuous-identity-authorization.html · https://www.strongdm.com/
- Britive: https://www.britive.com/resource/blog/sacr-arise-britive-profile · https://cybercompanyprofiles.com/companies/britive [aggregator]

### Inferences
- **Indent and Sym were the two most startup-native players. Both died within about 3 months of each other in 2024, without an acquirer.**
  - Indent's exit was an acqui-hire into OpenAI. Its founder had Sam Altman as a seed investor.
  - Sym raised ~$12M and published no durable pricing.
  - Neither published traction figures beyond logos and one customer metric.
  - Reading: they could not grow from about $8/user/month startup seats into a venture-scale business.
- **Common Fate's published pricing ($1,980/yr minimum, $2 to $8/user modules) shows how small the ACV was.** A 30-engineer startup would pay about $2k to $3k/year.
- **Founders of these tools ended up at AI labs** (Indent's at OpenAI; Common Fate's Chris Norman at Anthropic, per LinkedIn snippet). The skill set was valued more than the businesses.

### Gaps
- No primary statement of *why* Indent, Sym or Common Fate shut down. Common Fate's post thanks customers without giving reasons.
- No ARR or customer counts for any of the dead vendors.
- Abbey Labs' status is not confirmed.
- Opal and ConductorOne's early (2021 to 2023) startup pricing, if any existed, was not found.
- Entitle's deal value is undisclosed.

---

## Q2. Did startups buy primarily for SOC 2 / audit evidence?

### Takeaway
Partly yes. SOC 2 was an explicit buying trigger and marketing pillar for the startup-tier tools: Indent had a /soc2 page and a Secureframe evidence integration, and an Opal reviewer cites "SOC 2 work". But the strongest customer metric found is operational speed, not audit (Modern Treasury: 3 days to 59 seconds). That suggests SOC 2 opened the deal and engineering friction justified it.

### Cited findings
- **Indent**
  - Ran a dedicated SOC 2 landing page: https://indent.com/soc2
  - Partnered with Secureframe. Each request is logged with requester, business reason, system, duration and reviewer, and can be exported or sent to Secureframe as SOC 2 evidence: https://secureframe.com/blog/indent-partnership
  - Indent's own SOC 2 Type II: https://indent.com/blog/soc-2-type-2
- **Indent customer, Modern Treasury.** The story leads with speed: median approval wait fell from 3 days to 59 seconds. Before Indent, the team messaged an admin for temporary production access, and revocation was inconsistent. https://indent.com/customers/modern-treasury
- **Opal**
  - An AWS Marketplace reviewer says Opal "enabled us to implement access management best practices right out of the gate for SOC 2 work" and helped speed up access requests through Slack: https://aws.amazon.com/marketplace/reviews/reviews-list/prodview-6xeygvxviq6bg?page=3
  - Opal's 2021 post on its own SOC 2 says short-lived access reduced the permissions in audit scope and that it exports a report for auditors: https://opal.dev/blog-post/opal-is-soc-2-certified
  - Compliance-readiness use-case page: https://opal.dev/uses-cases/compliance-readiness
- **Sym.** G2 reviewers praise the "slack-native approval workflow for elevated access" and the "audit trail", but don't mention SOC 2 explicitly: https://g2.com/products/sym/reviews/sym-review-7938924
- **The audit pain is real.**
  - A compliance vendor's blog says auditors flag access granted through informal Slack messages at startups, and recommends JIT tools with auto-expiry: https://bastion.tech/blog/most-common-soc2-audit-exceptions/
  - An engineer's account of owning a first SOC 2 audit: "the proof lived in people's heads, in Slack threads that scroll away": https://www.sybill.ai/blogs/soc-2-audit
- **For agents.** Permit.io's SOC 2 CC6 / ISO 27001 checklist lists "just-in-time approvals, human-in-the-loop records, separation-of-duties evidence" as required evidence for agent privileged access: https://www.permit.io/blog/ai-agents-soc2-iso27001-access-control-checklist

### Inferences
- SOC 2 creates a once-a-year, deadline-driven need that a Jira ticket, a Slack workflow or Vanta's built-in access tracking can also satisfy. That makes it a weak basis for recurring willingness to pay.
- Vendors that survived reframed themselves around least privilege, identity governance and security outcomes, not audit evidence.

### Gaps
- No founder statement or survey quantifying the share of purchases driven by SOC 2.
- Couldn't read full case-study pages (fetch blocked).

---

## Q3. Who now markets access requests, approvals or JIT grants for AI agents, NHIs and MCP (2025 to 2026)? Pricing and self-serve availability

### Takeaway
Nearly every surviving access vendor, NHI vendor and MCP gateway now markets agent access controls. Human approval specifically is either:
- an enterprise-tier feature (Lunar MCPX, Permit, Auth0 Async Authorization, Runlayer in beta), or
- a commodity in free or cheap tools (Preloop, VantaGate, hoop.dev, Bifrost, TrueFoundry, Agno).

Most enterprise products push toward automated, policy-based decisions that escalate to a human only by exception (Opal Paladin and Opal Zero, Apono intent-based access, Drata plain-English policies). That is the opposite of "always ask a named human".

### Cited findings

**Access/JIT incumbents extending to agents**
- **Opal Zero** (Sep 2026). Evaluates each agent request against policy and enforces scoped permissions in the existing gateway. Integrations: Claude, Databricks Unity Gateway, Okta Cross App Access. Pitched as avoiding the "human-approval bottleneck". https://www.ciodive.com/press-release/20260916-opal-security-launches-opal-zero-to-make-least-privilege-a-reality-for-ente-1/
- **P0** (Dec 2025). Agent access tied to the human user, with HITL approval. https://p0.dev/blog/more-than-visibility-p0-introduces-privileged-access-control-for-agents/
- **Apono / 1Password** (Jun 2026). Agents receive permissions derived from the authorizing human; access is revoked if behaviour deviates. 1Password Credential Broker is in private beta. https://1password.com/press/2026/june/1password-acquires-apono
- **StrongDM / Delinea.** JIT access "for developers, DBAs, DevOps teams and AI agents", with approvals via Slack or Teams. https://www.strongdm.com/
- **Teleport.** Agentic Identity Framework (Jan 2026): MCP proxy with per-tool audit events and per-agent certificates. Beams agent runtime (public beta Jun 2026). https://goteleport.com/docs/agentic-identity-framework/ · https://www.helpnetsecurity.com/2026/03/19/teleport-announced-beams/
- **Britive ARC.** Runtime authorization of agent actions bound to human privilege. https://www.britive.com/resource/blog/sacr-arise-britive-profile
- **ConductorOne/C1** and **Lumos** (Identity Agent Force, Jun 2026). Govern AI identities, enterprise-only. Links in Q1.

**NHI vendors**
- **Aembit.** IAM for Agentic AI (Oct 2025) and MCP Identity Gateway. **Self-serve:** Starter is free (10 client workloads, 24-hour log retention), then Teams (monthly, up to 500 workloads), then Enterprise. A human-approval workflow is not confirmed on Aembit's own pages. https://docs.aembit.io/get-started/signup-options/
- **Astrix.** **Cisco announced intent to acquire on 4 to 5 May 2026**, reported at $250M to $400M. https://pulse2.com/cisco-to-acquire-astrix-security-to-expand-ai-agent-and-non-human-identity-security-capabilities/
- **Oasis.** Agentic Access Management (intent-aware JIT for agents); **$120M Series B, Mar 2026**. https://www.prnewswire.com/news-releases/oasis-security-launches-agentic-access-management-the-first-identity-solution-built-for-ai-agents-302619375.html · https://www.accessnewswire.com/newsroom/en/computers-technology-and-internet/oasis-security-raises-120m-series-b-to-secure-the-rise-of-enterpr-1149255
- **Clutch.** $20M Series A (2025), $28.5M total; agentic AI governance module. https://www.clutch.security/blog/clutch-security-20M-series-a
- **Token Security.** $20M Series A, ~$28M total; agent product (Mar 2026); RSAC 2026 Innovation Sandbox. https://zeltser.com/media/rsac-2026-sandbox/token-security
- **Natoma.** **Snowflake agreed to acquire (2026)**; $7M seed (May 2025); 27 employees. https://www.bankinfosecurity.com/snowflake-to-buy-startup-natoma-focused-on-ai-access-control-a-31808
- **Keycard.** $38M (Oct 2025; a16z, boldstart, Acrew); ephemeral, task-scoped signed tokens; pricing not public. https://fintech.global/2025/10/23/keycard-secures-38m-to-advance-ai-agent-security/
- No public pricing was found for Astrix, Oasis, Clutch, Token, Natoma or Keycard. All are sales-led.

**MCP gateways**
- **Runlayer.** $11M seed (Nov 2025). $30M Series A (24 Jun 2026) [aggregator]. No public pricing; demo-led. **Human approval rules are a beta layer on tool calls.** https://techcrunch.com/2025/11/17/mcp-ai-agent-security-startup-runlayer-launches-with-8-unicorns-11m-from-khoslas-keith-rabois-and-felicis · https://appsecsanta.com/runlayer
- **MintMCP.** "Contact for enterprise pricing"; SOC 2 Type II (monitored via Drata); no HITL found. https://mintmcp.com/blog/mintmcp-vs-runlayer-vs-portkey
- **Lunar MCPX.** Open source. Reportedly free up to 50 users on a shared instance, Pro up to 100 users, **HITL in the Enterprise tier only** (per the MintMCP blog and competitor Preloop). https://www.mintmcp.com/blog/lunar-mcpx-alternatives · https://preloop.ai/vs/lunar
- **Docker MCP Gateway, Obot.** Free and open source; no approval step confirmed. https://www.stackone.com/blog/best-mcp-gateways/
- **Cloudflare MCP server portals.** Run on Cloudflare One, free up to 50 users then $7/user/mo; no approval step found. https://composio.dev/content/best-mcp-gateway-for-developers
- **Permit.io MCP gateway.** HITL approvals for high-risk actions on the **Enterprise tier only**. https://www.permit.io/mcp-gateway/pricing
- **TrueFoundry.** Tool-approval policies notify approvers by email, Slack, PagerDuty or Teams. Plans of $499 and $2,999 per month per a third party [unverified]. https://www.truefoundry.com/docs/ai-gateway/mcp/mcp-tool-approval
- **Bifrost.** Manual approval of tool calls by default, with per-tool allowlisting. https://www.getmaxim.ai/bifrost/blog/bifrost-mcp-gateway-access-control-cost-governance-and-92-lower-token-costs-at-scale

**Agent-approval specialists and identity platforms**
- **HumanLayer** (YC F24). API and SDK for agents to reach humans for approvals over Slack, email and SMS. Had paying customers from Aug 2024. **Then moved to CodeLayer (coding-agent tooling)** after Q1 2025 discovery with dozens of agent teams. https://www.claude.com/blog/building-companies-with-claude-code · https://ycombinator.com/launches/M8e-humanlayer-human-in-the-loop-for-ai-agents-and-beyond
- **Preloop.** Open-source "AI agent control plane". **Free self-hosted (Apache 2.0) includes human approvals on mobile, Slack, Mattermost and webhook. Teams plan $29/user/mo or $290/user/yr** adds hosting, RBAC and quorum approvals. https://preloop.ai/pricing
- **VantaGate** (MCP server; no apparent relation to Vanta). Intercepts high-risk actions, routes them to a human via Slack or email, and returns a **cryptographically signed (HMAC-SHA256) decision**. G2 marketplace lists **Free $0, Pro $49/mo, Scale $199/mo** [unverified on vendor site]. https://ai.g2.com/marketplace/tools/vantagate-mcp-server · https://glama.ai/mcp/servers/Aderix/vantagate-mcp-server
- **LoopPause MCP.** Similar HMAC-signed approval decisions. https://glama.ai/mcp/servers/soteeabam/looppause-mcp
- **gotoHuman.** Managed HITL approvals for AI workflows; pricing not found. https://enterprisedna.co/directories/open-source/gotohuman
- **Auth0 for AI Agents.** GA on 19 Nov 2025, including Async Authorization (push-notification approval of critical actions). Async Authorization is on the Enterprise tier or an add-on. https://auth0.com/blog/auth0-for-ai-agents-generally-available/
- **hoop.dev.** Open-source access gateway with JIT reviews in Slack and Teams, positioned for agents. Free core; hosted on AWS Marketplace at $10,000/mo. https://web.hoop.dev/plans/free-and-open-source · https://aws.amazon.com/marketplace/pp/prodview-e7nisqkw2uz4m

**Compliance platforms**
- **Drata AI Agent Governance.** Limited availability since **4 Aug 2026**, for "qualified enterprises" running agents on Anthropic first.
  - An MCP Proxy evaluates every tool call against plain-English policies, and a violating action is stopped before it executes.
  - It "recommends actions for manual approval".
  - It produces a "durable, tamper-evident evidence feed".
  - https://drata.com/about/news/drata-extends-trust-management-platform-to-continuously-monitor-and-govern-ai-agents · https://securityboulevard.com/2026/08/drata-opens-limited-availability-for-ai-agent-governance-product/ · https://drata.com/blog/introducing-ai-agent-governance
- **Vanta.** Slack DMs when its own Custom Agents need approval (Jul 2026), and multi-step approval workflows for Issues (Aug 2026). Native tracking of new access requests. https://help.vanta.com/en/articles/11345422-product-updates · https://www.vanta.com/products/soc-2

### Inferences
- **Acquirers are paying for agent-identity positioning:** 1Password–Apono (>$200M), Cisco–Astrix ($250M to $400M reported), Snowflake–Natoma, Delinea–StrongDM. They buy *enterprise* agent-access vendors, not startup tools.
- **Signed approval decisions are already shipped by free and $49/mo MCP tools** (VantaGate, LoopPause). The "signed receipt" is not a moat on its own.
- **Drata's evidence feed is the most direct threat** to the "SOC 2 evidence for agent actions" angle. The compliance platform the customer already pays for is absorbing it.

### Gaps
- Could not confirm whether Aembit, Oasis, Astrix or Token route to a *named human in Slack*.
- Docker, Obot and Cloudflare approval support not confirmed.
- Drata and Opal Zero pricing not public. The $30k Opal Zero figure is unverified.
- Not researched this session: Okta Identity Governance "Access Requests". Its Slack-based requests came from the 2021 Atspoke acquisition [prior knowledge, unverified]. Also not researched: Okta Cross App Access pricing, Microsoft Entra Agent ID, AWS AgentCore.

---

## Q4. Is there a self-serve gap for a 5-to-50-engineer team? Who comes closest, at what price?

The team wants: an agent asks for a risky action or elevated access, a named human approves in Slack, the grant is one-time and expires, and evidence drops into Vanta/Drata.

### Takeaway
A narrow gap exists for the *full bundle*: one-time, subject-bound grant + named approver + signed, tamper-evident receipt + automatic Vanta/Drata evidence push, self-serve at under ~$300/month. But each piece is available cheaply or for free, and nobody found sells the Vanta/Drata push for agent approvals self-serve. Drata does it natively for enterprises.

### Closest options

| Option | Agent asks | Named human in Slack | One-time, expiring grant | Signed / tamper-evident record | Vanta/Drata export | Self-serve price |
|---|---|---|---|---|---|---|
| Preloop | Yes (tool calls) | Yes (Slack, Mattermost, mobile; quorum on Teams) | Pauses the call; no grant model confirmed | Audit log; signing not confirmed | Not found | Free (self-hosted) / $29 per user/mo |
| VantaGate | Yes (MCP) | Slack/email reviewer | Per action | **HMAC-signed decision** | Not found | $0 / $49 / $199 per mo [unverified] |
| hoop.dev | Yes (gateway, positioned for agents) | Slack/Teams JIT reviews | Time-limited sessions | Audit | Not found | Free core; hosted $10k/mo |
| TrueFoundry gateway | Yes (MCP tool approval) | Email/Slack/PagerDuty/Teams | Per call | Not confirmed | Not found | ~$499/mo [unverified] |
| Teleport Community | Humans (access requests); agents via Enterprise features | Slack plugin (edition support unverified) | Yes (TTL) | Audit log | Not found | Free under 100 employees and $10M revenue |
| StrongDM (Delinea) | Yes | Slack/Teams | JIT | Session audit | Not found | Sales-led |
| Lunar MCPX / Permit / Auth0 Async | Yes | HITL (Auth0 via push) | Per call | Audit | Not found | HITL only in Enterprise tier |
| Drata AI Agent Governance | Yes (MCP proxy) | "Manual approval" recommendations | Policy-based | **Tamper-evident evidence feed** | **Native (it is Drata)** | Limited availability, enterprise-only, unpriced |
| Opal Zero / P0 / Apono | Yes | Escalation to a human | JIT | Audit | Not found | Enterprise (reported $30k for Opal Zero) |

Sources: as cited in Q3. On evidence push: Vanta and Drata both expose REST APIs with custom evidence upload, so a small vendor can integrate without a partnership. https://truvocyber.com/blog/soc2-automation-compliance-as-code-guide

### Inferences
- **For a small team, the realistic alternatives are free:** an OSS gate (Preloop, Bifrost, Agno's built-in "human approval for MCP tool calls"), a Slack workflow, and a manual evidence upload to Vanta. A paid product must beat "free + 1 hour before the audit".
- **The differentiated wedge is auditor-grade evidence**, not the approval UX:
  - signed receipt binding approver, agent, subject, scope and expiry;
  - mapping to SOC 2 CC6.1–6.3 and CC8.1 (change management);
  - auto-push to Vanta/Drata.
- **The window is time-limited.** Drata already ships this for enterprise customers, and Vanta already has Slack approvals for its own agents.

### Gaps
- Could not verify Preloop's or VantaGate's evidence-export features, or their traction.
- Did not check whether Teleport Community's Slack access-request plugin works for agents.
- No evidence found of any small team paying specifically for agent approvals plus SOC 2 evidence.

---

## Q5. Lessons: why startup-tier access-approval tools were acquired or moved upmarket; price points and packaging

### Takeaway
The evidence points to "self-serve startup tier did not become a business", not to a land-and-expand success. All four low-priced pure plays (Indent, Sym, Common Fate, Abbey) died or went dormant. Value accrued to vendors selling governance to enterprises (Opal, C1, Lumos at $27k+/yr) or to PAM and identity acquirers (BeyondTrust, Delinea, 1Password, Cisco).

### Cited price points that existed
| Vendor | Price |
|---|---|
| Indent | Free (100 requests/mo, 30-day logs); $8/person/mo; G2 shows $160/mo for 20 users |
| Abbey Labs | Free under 20 users; $20/user/mo |
| Common Fate | $1,980/yr minimum; $2 to $8/user/mo modules |
| Teleport | Free for small companies (under 100 employees and $10M revenue) |
| Lumos | $27k / 12 months (AWS Marketplace) |
| hoop.dev | $10k/mo hosted (AWS Marketplace) |
| Opal Zero | Reported $30k [unverified] |
| Agent-gate newcomers | Preloop $29/user/mo; VantaGate $49 to $199/mo |

Sources in Q1 and Q3.

### Inferences (no primary "why" statements were found)
1. **ACV too small.** A 20-to-50-seat startup at $3 to $8 per user is roughly $1k to $5k/yr. That cannot fund integration upkeep across AWS, GCP, Okta, databases and Kubernetes. Common Fate admitted maintaining OSS and cloud codebases "was not sustainable for a small team" (Glide sunset).
2. **Integration-heavy products favour incumbents.** Identity, PAM and IdP vendors bundle access requests. Teleport gives it away to small companies, and Vanta tracks access requests natively. Okta added Slack access requests via Atspoke in 2021 [prior knowledge, unverified].
3. **SOC 2 urgency is episodic.** Once the audit passes, the tool competes with Jira and Slack workflows. The expansion path is a later IGA purchase, and growing startups bought enterprise suites (Opal, C1, Lumos) instead.
4. **Talent exits.** Indent's team went to OpenAI, and Common Fate's co-founder is now at Anthropic (snippet). Pure-play teams were valuable mainly as talent.
5. **Agent-era replay.** HumanLayer (Slack approvals for agents, YC F24) had paying startups within weeks but left the approvals API for coding-agent tooling within about a year. That is an early sign that "approval as an API" has low standalone pull even with agents.
6. **What did work: packaging around governance at enterprise ACVs** (least privilege, access reviews, NHI/agent discovery), and being acquired for agent-identity positioning. Free OSS cores (Granted, Preloop, hoop, Lunar) drive adoption, but no OSS-to-paid conversion success was found among the startup-tier tools.

### Gaps
- No post-mortems were found from Indent, Sym or Common Fate founders explaining unit economics.
- No data on self-serve conversion rates or churn.

---

## Ratings (one line each)

- **Demand evidence: Weak-to-moderate.** Startups did buy Slack JIT approvals partly for SOC 2 (Indent's /soc2 page, the Secureframe export, an Opal reviewer), but every startup-priced vendor failed. For agents, demand evidence is enterprise surveys rather than small-team purchases:
  - Gravitee 2026: only 14.4% of organisations put all agents live with full security/IT approval.
  - Dynatrace: 69% of agentic decisions are still verified by humans.
  - FINRA's 2026 oversight report recommends human checkpoints.
  - Source for these figures: search summaries, including https://securitybrief.co.uk/story/security-fears-keep-half-of-agentic-ai-stuck-in-pilots
  - HumanLayer pivoted away from approvals.
- **Competitive gap: Narrow and closing.** The approval UX and signed decisions are free or under $50/mo (Preloop, VantaGate, hoop, Bifrost), enterprise vendors cover agent JIT, and Drata ships tamper-evident agent evidence. The only open space is the self-serve "agent approval → signed receipt → Vanta/Drata evidence" bundle for teams under 50 engineers.
- **Self-serve buyability: Moderate at ≤$50 to $300/mo or ~$10 to $30/user; poor above ~$3k/yr.** Historical startup price points ($0 to $8/user, ~$2k/yr minimums) were buyable but did not sustain companies.

## Overall verdict
As a standalone, venture-scale company, the niche looks **weak**:
- History shows startups buy this cheaply and episodically, around audits.
- The agent-era buyers with budget are enterprises, served by Opal, Apono/1Password, StrongDM/Delinea and Drata.
- Signed-approval MCP gates already exist at $0 to $49/mo.

It is viable only as:
- a low-cost, developer-led product whose differentiator is auditor-grade evidence (signed receipts mapped to SOC 2 CC6 and CC8, auto-pushed to Vanta/Drata) rather than the approval UX; or
- a feature or wedge inside a broader product.

Any plan should assume Drata and Vanta absorb the evidence half within 12 to 24 months.

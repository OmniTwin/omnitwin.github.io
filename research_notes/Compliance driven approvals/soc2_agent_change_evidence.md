# Do compliance audits require documented human approval of AI agent production changes, and does anyone sell self-serve evidence for it?

Research date: 2026-10-16. Scope: SOC 2, ISO 27001, ISO 42001, HIPAA, PCI DSS. Buyer in scope: a startup CTO or founder with a card who already pays for Vanta, Drata, Secureframe, Oneleet, Comp AI, Delve or Sprinto. Enterprise sales are out of scope.

## How this was researched, and how far to trust it

- **No page was read in full.** In this session WebFetch failed on DNS for every domain, and curl got a proxy 403. Reddit is blocked for the search tool. Every finding below comes from WebSearch result summaries and snippets. Treat specific numbers and quotes as "per snippet" unless the note says otherwise.
- **Control wording.** The control wording for CC8.1, CC6.x, CC5.1, PCI 6.5.1, 6.5.4 and 6.2.3.1, ISO 27001 A.8.32 and A.5.3, and HIPAA §164.3xx is quoted from the primary standards as I know them. The snippets matched this wording where they showed it. Before quoting any of it to a customer, check it against the official PDFs: the AICPA TSC 2017 with 2022 points of focus, PCI DSS v4.0.1, ISO/IEC 27001:2022, and 45 CFR Part 164.
- **Source quality.** Most of the 2025–2026 material on "SOC 2 for AI agents" is SEO content from vendors who sell agent governance tools. Wherever possible I have marked whether a source is a vendor, a practitioner or authoritative.
- **Dates.** Anything dated before 2025 is marked **[pre-2025]**.

---

## Q1. What the control frameworks require: approval of production changes and segregation of duties

### Takeaway
Every framework except HIPAA explicitly requires authorization or approval of production changes. Only PCI explicitly requires the reviewer to be someone other than the author, and only for manual code review (6.2.3.1). SOC 2 and ISO get there through segregation-of-duties principles and auditor practice. None of the texts mentions AI agents, and none names who must approve. The criteria are technology-neutral, so in practice auditors sample changes and check that each one was approved by someone other than its author.

### Cited findings

**SOC 2 (AICPA TSC 2017, 2022 points of focus are still current in 2026)**
- **CC8.1:** "The entity authorizes, designs, develops or acquires, configures, documents, tests, approves, and implements changes to infrastructure, data, software, and procedures to meet its objectives." The points of focus include "Authorizes Changes", "Approves Changes", "Tracks System Changes", "Manages Changes Throughout the System Lifecycle", "Provides for Changes Necessary in Emergency Situations" and "Configures Software". Seen via snippets: https://www.cyberday.ai/requirement/soc-2-cc8-1-change-management-procedures and https://hicomply.com/soc-2/soc-2-controls-change-management-cc8. HiComply misquotes "organises" for "authorizes", so rely on the AICPA text.
- **The scope covers data and infrastructure, not only code.** Under that wording, an agent's database write, infrastructure apply or configuration change is a "change to infrastructure, data, software" and falls inside CC8.1. This is my reading of the text. soc2auditors.org reaches the same conclusion (see Q2).
- **CC6.1, CC6.2 and CC6.3 cover logical access.** CC6.3: "The entity authorizes, modifies, or removes access to data, software, functions, and other protected information assets based on roles, responsibilities, or the system design and changes, giving consideration to the concepts of least privilege and segregation of duties…" CC6.2 requires registering and authorizing users, which practitioners extend to non-human identities.
- **CC5.1 has a segregation-of-duties point of focus.** It accepts alternative controls where full separation isn't practical. A 2026 analysis notes that "SOC 2 names no approver", so the expectation of a second human comes from auditor practice and segregation of duties (ysecurity.io, snippet): https://ysecurity.io/blog/agentic-sdlc-code-review-soc-2/
- **The criteria are unchanged in 2026.** The AICPA still lists the 2017 TSC with the 2022 points of focus, and there is no exposure draft of revised criteria: https://soc2auditors.org/insights/soc-2-ai-agents-mcp-controls/
- **Typical auditor test.** The auditor selects a sample of changes from the period and traces each one from request to deployment. For each sampled change they expect the request or PR, the approval, the test result, the deployment timestamp, and a post-implementation review for emergency changes. The most common exception at early-stage companies is developers approving their own changes. Sources:
  - https://soc2auditors.org/insights/soc-2-change-management-controls/
  - https://getsecureslate.com/blog/soc-2-evidence-for-change-management
  - https://www.auditpath.io/blog/soc2-change-management
  - https://www.scrut.io/hub/soc-2/soc-2-common-criteria
- **Chat approvals.** A "looks good" message in Slack counts only if it is retained and tied to the specific change, approver and time: https://bastion.tech/blog/most-common-soc2-audit-exceptions/ and https://certpro.com/hub/soc-2/audit-process/soc-2-audit-exceptions/ (2025–26, vendor blogs). This is exactly the gap a receipt bound to a change closes.

**ISO/IEC 27001:2022**
- **A.8.32 Change management:** "Changes to information processing facilities and information systems shall be subject to change management procedures." The 27002 guidance lists authorization, impact assessment, testing, communication, records of changes and emergency changes. Snippets: https://www.isms.online/iso-27001/annex-a-2022/8-32-change-management-2022/ and https://hightable.io/iso27001-annex-a-8-32-change-management. Secondary sources disagree on "shall" versus "should". 27001 Annex A uses "shall" and 27002 uses "should".
- **A.5.3 Segregation of duties:** "Conflicting duties and conflicting areas of responsibility shall be segregated." Also relevant are A.8.2 (privileged access) and A.8.15 (logging).
- **Scope in practice.** Secondary sources list database changes, cloud configuration changes and access-control changes as in scope, and say segregation of duties means the requester is not the approver: https://www.konfirmity.com/blog/iso-27001-change-management

**ISO/IEC 42001:2023 (AI management system)**
- **A.6.2.8 "AI system recording of event logs":** the organization determines at which lifecycle phases event logging is enabled, at minimum while the system is in use. A.6.2.6 covers operation and monitoring. A.9.x covers responsible use and human oversight.
- **What practitioners expect as evidence.** Agent event logs should capture identity, time, command, approval and purpose. Human-override events are the most commonly missing evidence: https://www.isms.online/iso-42001/annex-a-controls/a-6-ai-system-life-cycle/a-6-2-8-ai-system-recording-of-event-logs/ and https://goteleport.com/blog/iso-42001-audit-evidence/ (vendor). One source misnumbers these controls, so verify against the standard.

**PCI DSS v4.0.1**
- **6.5.1:** "Changes to all system components in the production environment are made according to established procedures that include:
  - Reason for, and description of, the change.
  - Documentation of security impact.
  - **Documented change approval by authorized parties.**
  - Testing to verify that the change does not adversely impact system security.
  - For bespoke and custom software changes, all updates are tested for compliance with Requirement 6.2.4 before being deployed into production.
  - Procedures to address failures and return to a secure state."
  - **Testing procedure 6.5.1.b:** "Examine recent changes to system components and trace those changes back to related change control documentation…" The 3.2.1 predecessor was 6.4.5.2 **[pre-2025]**. Third-party copies: https://ce.prod.cloudaware.com/frameworks/pci-dss-v4.0.1/06/05/01/ and https://maintenance.securitymetrics.com/blog/auditor-tips-requirement-6-system-updating-and-software-development
- **6.5.4:** "Roles and functions are separated between production and pre-production environments to provide accountability such that only reviewed and approved changes are deployed." The applicability notes allow procedural compensations for small teams: https://myctrl.tools/frameworks/pci-dss-v4/6-5-4
- **6.2.3.1 (manual code review):** the code must be reviewed "by individuals other than the originating code author" and "approved by management prior to release." This is the only explicit "someone other than the author" language in these frameworks.

**HIPAA Security Rule (45 CFR 164, current rule)**
- **No explicit change-management or approval standard.** The relevant provisions are:
  - §164.312(b) Audit controls: "Implement hardware, software, and/or procedural mechanisms that record and examine activity in information systems that contain or use electronic protected health information."
  - §164.308(a)(1)(ii)(D): information system activity review.
  - §164.312(c)(1): integrity, meaning protecting ePHI from improper alteration or destruction.
  - §164.308(a)(4): information access management.
- **HIPAA has no certifying audit.** Startups usually get a third-party assessment, or HITRUST or SOC 2+HIPAA.
- **The 2025 NPRM is still not final.** It was published on 2025-01-06. OCR is reviewing more than 4,700 comments, and the Unified Agenda targets July 2027 (RIN 0945-AA22): https://www.pact-one.com/2026/09/hipaa-security-rule-2026-status/ (Sept 2026) and https://www.hipaajournal.com/hipaa-updates-hipaa-changes/

### Inferences
- **Approval is the hard requirement for PCI and the de facto requirement for SOC 2 and ISO.** Under SOC 2 it is auditor-dependent: the approval point names no approver.
- **An agent's direct production action is plausibly a CC8.1 or A.8.32 "change".** That holds for data and infrastructure changes. A routine operational data write by the product, such as an app writing rows, normally is not treated as a "change". An agent running ad-hoc SQL against production or applying infrastructure is closer to a privileged admin action, which falls under CC6 and CC8. Auditors will judge where the line falls.
- **HIPAA is a weak driver.** There is no change-approval clause. Its logging requirement (§164.312(b)) is met by ordinary audit logs.

### Gaps
- I could not open the AICPA TSC PDF, the PCI SSC PDF or the ISO texts to confirm wording verbatim in this session.
- I found no AICPA statement on whether agent actions are "changes".

---

## Q2. What auditors, CPA firms and compliance platforms said about AI agents in 2025–2026

### Takeaway
There is **no authoritative guidance**. The AICPA has not issued AI-specific criteria, points of focus or an addendum. CSA's August 2026 note says auditors are "improvising". Practitioner consensus, mostly from vendors, is that an AI may assist with review but a **named, accountable human must approve**, and that agent-initiated changes to prompts, tools, MCP configuration and infrastructure fall under CC8.1. Two counter-signals stand out:
- Workstreet: an automated approval process can pass if it is formally defined and tested.
- Amp (late August 2026) publicly obtained SOC 2 Type II while shipping with **no PRs and no pre-merge human approval**.

The CPA firms named in the brief had not published agent-specific positions that I could find. Those firms are A-LIGN, Prescient, Johanson, Insight Assurance, Sensiba, Schellman, BARR and KirkpatrickPrice.

### Cited findings

**Authoritative and quasi-authoritative sources**
- **CSA AI Safety Initiative, "The SOC 2 AI Gap: Auditors Improvise Where AICPA Has Not Acted" (2026-08-30).** The AICPA has published no AI-specific TSC, points of focus or authoritative addendum. Auditors are assembling ad-hoc evidence requests, so two clean Type II reports can rest on different AI control sets. https://labs.cloudsecurityalliance.org/research/csa-research-note-soc2-ai-controls-gap-20260830-csa-styled/
- **AICPA Technical Q&A 9561 (nonauthoritative)** reportedly covers a service organization's use of AI in SOC 1 and SOC 2. I saw only one source for this: https://soc2auditors.org/insights/soc-2-ai-agents-mcp-controls/
- **The AICPA's March 2026 AI publication** (forensic and valuation) is "neither authoritative guidance nor standards" and does not address SOC 2 (via the CSA note).
- **AICPA Peer Review Board alert (May 2026)** on high-volume and templated SOC 2 practices, with added scrutiny from 2026-06-01. It followed the Delve allegations of March 2026 (TechCrunch, 2026-03-21; IANS, 2026-03-30):
  - https://www.iansresearch.com/resources/all-blogs/post/security-blog/2026/03/30/delve-allegations-expose-weak-points-in-modern-compliance
  - https://captaincompliance.com/news/the-delve-scandal-fake-soc-2-audits-open-source-code-theft-and-exit-from-y-combinator/
  - Delve denies the allegations. Relevance: auditors are under pressure to see real operating evidence rather than templates, which is modestly positive for evidence products.

**Practitioner and vendor positions (2026)**
- **Workstreet (compliance consultancy and Vanta partner).** SOC 2 doesn't prescribe the reviewer type. "the agent can assist with the review, but a human owner should remain accountable for approving the change — unless the company has formally defined and tested an automated approval process." A policy that requires peer review by another engineer is not met by an AI reviewer. https://www.workstreet.com/blog/can-ai-agents-satisfy-soc-2-code-review-requirements
- **ysecurity.io.** Approval responsibility remains with an authorized team member. A large AI diff approved within minutes with no comments is treated as a non-operating review. It cites a study finding that **61.38% of agent-authored PRs have no recorded review activity**; I did not see the study itself. https://ysecurity.io/blog/agentic-sdlc-code-review-soc-2/
- **thebrightbyte.com.** "the human who hit accept" must be named, traceable, and **not the same person who wrote the prompt**. This is the strictest reading, and it is bad news for solo founders. https://thebrightbyte.com/playbook/expertise/ai-coding-agents-soc2
- **soc2auditors.org (2026).** Agents are tested under CC6.1–6.3 (identity and permissions), CC7.2–7.3 (monitoring) and CC8.1 (prompts, tools, MCP configuration). "Expect your auditor to ask for a tool-call log you can show is complete." High-risk tool calls (deleting records, changing access, emailing customers) need sign-off, with **approval records for sampled actions**. The site flags as a likely exception an approval rule that lives only in the system prompt. It labels all of this "our reading". https://soc2auditors.org/insights/soc-2-ai-agents-mcp-controls/
- **Teleport (vendor).** Auditors want the same change evidence for AI-driven changes: request, approval, validation and rollback. An AI pipeline that updates production with no ticket "can directly conflict with change management expectations". https://goteleport.com/blog/ai-agents-soc-2/
- **containment.ai (vendor).** The central CC8.1 question is "what authorized this agent action". For most organizations, "the honest answer … is: nothing did, at the policy level." https://containment.ai/blog/ai-governance-platform-soc2-audit
- **Aprio and other blogs.** "The criteria didn't change, but the examination did." I could not retrieve the Aprio article itself: https://www.aprio.com/insights-events/soc-2-and-ai-in-2026-the-criteria-didnt-change-but-the-examination-did-ins-article/ and https://www.complyjet.com/blog/does-soc-2-cover-ai
- **Linford & Co** was cited as saying auditors increasingly probe shadow AI that bypasses change management and vendor review (secondhand): https://soc2auditors.org/insights/soc-2-for-ai-companies/
- **Vanta, "9 things your auditor will want to see about your AI agents" (2026-04-20).** It leads with a complete agent inventory and defined ownership. I could not see the later items. https://www.vanta.com/resources/ai-agent-audit-preparation
- **Drata.** Auditors want proof that human oversight exists for high-risk actions and that any agent decision traces to a logged record and a named owner. https://drata.com/learn/agent-gov/governance-proof
- **Infrastructure-specific evidence.** A Terraform plan saved as a CI artifact, attached to a reviewed PR and gated by a required approval, is strong CC8 evidence. A locally run plan proves nothing. https://www.complyjet.com/blog/terraform-infrastructure-as-code-audit-evidence and https://hol.org/blog/introducing-agentic-boundary-terraform-skills

**Counter-signal: Amp, "That's not SOC 2 compliant" (~late August 2026).**
- Amp is a SOC 2 Type II vendor with no PRs. It controls only who can push to main, requires signed commits, and runs automated CI and security checks on every change. Commits link back to the agent threads that produced them.
- The post says the "TSC never mention git or pull requests", only that changes be authorized, tested, approved and recorded. Their auditor "worked with the team on controls that fit".
- Sources: https://ampcode.com/notes/thats-not-soc-2-compliant, the HN debate at https://news.ycombinator.com/item?id=49308073, and Semgrep commentary at https://semgrep.dev/blog/2026/end-is-nigh-for-code-review/

**Who is the "author"?** No authoritative answer exists. Practitioner views fall into three positions:
- The agent is the author, so the human approver supplies segregation of duties. This reading is implied by Workstreet's conditional position.
- The human who prompted is the author, so a second human must approve (thebrightbyte).
- Approval by the same person who asked the agent to act is weaker evidence than approval by a second person. This was a sentence in one snippet about "approval quality".

### Inferences
- **Auditors are asking about agents, inconsistently.** Requests skew toward inventory, ownership, access scoping and logs, not toward approval receipts specifically.
- **Most agent-authored code already reaches production through PRs.** Existing GitHub evidence covers it, as long as a human approves the PR. The residual gap is the **direct production action**: SQL, infrastructure apply, SaaS admin changes via MCP, or configuration changes outside git.
- **The cheapest control a startup can show is a negative one.** "Agents have no production write credentials. All changes go through PR." That costs nothing and competes directly with an approval-gate product.
- **Amp shows auditors will accept bespoke control designs.** So "you must have per-action human approval receipts" is not a universal audit requirement. It is one acceptable design among several.

### Gaps
- No post from A-LIGN, Prescient, Johanson, Insight Assurance, Sensiba, Thoropass, Oneleet, Comp AI or Delve surfaced on agent change approval.
- I could not search Reddit. HN gave general change-management debate, not agent-specific threads.

---

## Q3. What Vanta, Drata, Secureframe and others offer for AI agents, and whether a small vendor can plug in

### Takeaway
The two leaders moved into agent governance in mid-2026:
- **Vanta AI Governance**, early access from 2026-07-30.
- **Drata AI Agent Governance**, Limited Availability from 2026-08-04. It includes inline policy enforcement ("Mission Control") and a tamper-evident "Chain of Custody" mapped to SOC 2, ISO 27001 and ISO 42001.

Both are positioned for enterprises. Drata explicitly targets "qualified enterprises running agents on Anthropic". Neither documents a **named-human approval workflow with receipts** for agent production actions; Drata mentions human review triggers only in general terms.

Change-management evidence in all the platforms remains **GitHub, GitLab or Bitbucket PR-centric**. Integration paths for a small vendor exist, but with limits:
- Vanta public integrations **cannot push custom resources or author Custom Tests**.
- Drata partner connections cannot use Drata's automated tests.
- Secureframe has a Trust API for pushing evidence, and a precedent from 2022.

### Cited findings

**Vanta**
- **AI Governance launch (2026-07-30, early access).** AI inventory and impact assessments aligned to ISO 42001 and the EU AI Act. A Trust Graph links each agent to systems, data, vendor and controls. "help you define what each AI agent is allowed to do and detect when it operates outside those boundaries", with integrations across agent platforms, endpoints and gateways. https://www.vanta.com/resources/introducing-ai-governance-from-vanta and https://www.vanta.com/products/ai-governance
- **Agentic Trust Controls** moved to GA in August 2026. It is an open-source set of 65 controls: 43 for builders and 22 for deployers. https://runtimewire.com/article/vanta-agentic-trust-controls-ai-agent-governance
- **Vanta's own agents (2026-03-19)** for compliance, TPRM and trust work, with humans in the loop: https://www.businesswire.com/news/home/20260319211415/en/Vantas-New-Agents-and-Enterprise-Controls-Eliminate-Audit-Chaos
- **Integration Partner Program.** Partners get Developer Console access, the Build Integrations API, design review and a marketplace listing with one-click OAuth install. Requirements:
  - a working public integration
  - an end-to-end OAuth test
  - at least one resource or document sent
  - a logo, descriptions and a setup guide URL
  - **No fee was stated in the results.**
  - Sources: https://developer.vanta.com/docs/guides/become-partner and https://developer.vanta.com/docs/guides/list-your-integration
- **Limits that matter.** Public integrations push only built-in resource types (UserAccount, Computer, Vulnerability, BackgroundCheck, TrainingRecord, …). "Partners cannot push custom resources or author Custom Tests on a customer's behalf." Vanta's own docs cite change-management records as an example of custom data paired with a Custom Test, but that path is **private integration only**: the customer builds it with client credentials. https://developer.vanta.com/docs/build-integrations and https://developer.vanta.com/docs/concepts/resources
- **Existing change-management evidence** comes from the GitHub integration (PRs, branch protection, repository settings): https://www.vanta.com/integrations/github

**Drata**
- **AI Agent Governance (press release 2026-08-04, Limited Availability).** "ships first and deepest for Anthropic." Components:
  - a Sensor registers each agent with owner, identity, permissions and scope, and surfaces shadow agents
  - Mission Control checks actions against policy and blocks violations inline
  - Trust Ladder tests policies against live traffic before enforcement
  - Drift Detection
  - Chain of Custody: tamper-evident logs mapped to SOC 2, ISO 27001, ISO 42001 and NIST AI RMF
  - Sources: https://www.businesswire.com/news/home/20260804704925/en/Drata-Extends-Trust-Management-Platform-to-Continuously-Monitor-and-Govern-AI-Agents, https://drata.com/products/agent-governance and https://drata.com/blog/introducing-ai-agent-governance
- **Human approval.** Drata's guide mentions human-review triggers such as high-value transactions, sensitive data access and out-of-parameter actions. I found no documented approval routing or escalation: https://drata.com/learn/agent-gov/complete-enterprise-framework. A competitor's critique (containment.ai) argues Drata's product is monitoring and proof, not enforcement: https://containment.ai/blog/drata-ai-agent-governance-monitoring-vs-enforcement.html
- **Open API and Custom Connections (v2).** CUSTOM connections define a schema and POST records to `/custom-connections/{id}/resources/{rid}/records`. Customers can build custom tests. "Partner Connections" are built by partners using scoped API keys, including Evidence (full), but have "no access to Drata's automated tests". **No marketplace fee or certification process was found.**
  - https://developers.drata.com/developer-portal/v2/recipes/custom-connections/
  - https://help.drata.com/en/articles/12151473-partner-connections-expanding-compliance-with-external-tools
  - https://help.drata.com/en/articles/11825486-part-2-automate-evidence-submission
- **Existing test.** "Formal code review process" requires a PR with at least one required approval in GitHub: https://help.drata.com/en/articles/4776886-test-8-formal-code-review-process

**Secureframe**
- Supports ISO 42001 and NIST AI RMF as frameworks. Third-party reviews call it a "category addition" with no agent tooling: https://www.complyjet.com/blog/best-iso-42001-compliance-software
- **API and Custom Integrations; Technology Partner program; Trust API (October 2022) [pre-2025].** The Trust API lets partners push audit evidence. Launch partner **Indent** sent access-review approval events as SOC 2 evidence [pre-2025]. That is a close precedent for pushing approval events as evidence. https://secureframe.com/features/api.md and https://secureframe.com/blog/trusted-partner-program.md

**Others** (third-party comparison blogs only; directional)
- **Sprinto:** repositioned in March 2026 as an "autonomous trust platform". Its "AI governance" is mostly shadow-AI detection.
- **Thoropass:** added an MCP server in 2026, with early AI-governance coverage.
- **Comp AI:** its MCP connector can write and delete records.
- **Oneleet:** no ISO 42001 or AI governance coverage found.
- Sources: https://www.complyjet.com/blog/best-iso-42001-compliance-software and https://securityboulevard.com/2026/06/compliance-automation-platforms-compared-2026-vanta-vs-drata-vs-sprinto-vs-secureframe-vs-scytale-vs-thoropass/

### Inferences
- **Drata's Chain of Custody plus Mission Control is the closest incumbent analogue to "sealed decision record".** If Drata or Vanta add a "route to a named human" step, they cover the feature inside the platform the buyer already pays for. That is the biggest threat.
- **A marketplace listing exists but won't light up a Vanta test.** On Vanta, the vendor's records won't automatically satisfy a test via a public integration. The realistic paths are:
  - pushing documents
  - shipping a "private integration" recipe that the customer installs
  - selling exports the customer uploads
- Drata and Secureframe allow evidence pushes via API keys or the Trust API. That friction is acceptable for a technical founder but not "one-click".

### Gaps
- Vanta's AI Governance pricing and whether it is an add-on SKU are unknown.
- Drata's pricing for Agent Governance is unknown.
- Partner listing fees and revenue share for all three are unknown.
- I could not see Vanta's full built-in resource type list to confirm there is no "change" type.

---

## Q4. Evidence of pain

### Takeaway
The pain over **general** change-management evidence is real and long-standing:
- It is a perennial top SOC 2 exception category.
- Self-approval is the classic startup finding.
- "LGTM in Slack" is not evidence unless it is tied to the change.

**Agent-specific** pain is mostly asserted by vendors. I found very few organic buyer complaints of the form "how do I show approval for AI agent changes to my auditor". Reddit could not be searched, which is a real blind spot. Enterprise questionnaires did add AI sections in 2025–2026, covering agent identity scoping, human oversight and kill switches. That is a credible downstream pull, but it asks about controls, not receipts. Incidents such as Claude Code running `terraform destroy` on production (DataTalks.Club, February 2026) make the risk vivid.

### Cited findings
- **Exceptions.** "Changes deployed without approval", self-approval and undocumented emergency changes are standard CC8.1 exceptions. Access deprovisioning is often the #1 exception category overall. Sources (vendor blogs, 2025–26):
  - https://bastion.tech/blog/most-common-soc2-audit-exceptions/
  - https://certpro.com/hub/soc-2/audit-process/soc-2-audit-exceptions/
  - https://www.glocertinternational.com/resources/articles/common-soc-2-audit-findings/
- **HN debates on whether SOC 2 needs second-person approval:**
  - "That's not SOC 2 compliant" (~August 2026): https://news.ycombinator.com/item?id=49308073
  - "Ask HN: How to be SOC2 Type 2 compliant as a solo-entrepreneur?" (~May 2026). Comments include "nine roles that can't overlap" and "be creative with checks and balances", plus advice not to do SOC 2 speculatively before a contingent purchase order. https://news.ycombinator.com/item?id=48145524
- **Agent-authored PRs without review.** 61.38% have no recorded review, per a study cited by ysecurity and Workstreet (snippet). This is a symptom of the gap, not of buyer pain.
- **Secondhand anecdote.** A Series C fintech's auditor asked about Cursor data flows three weeks before its Type II. The question was about data flows, not approvals. https://sonomos.ai/blog/soc-2-ai-chatgpt-claude-2026/
- **Questionnaires:**
  - Shared Assessments reportedly added an AI domain to the SIG, covering inventory, risk classification, human oversight and governance.
  - CSA published the AI Controls Matrix (243 controls) and an AI-CAIQ.
  - Late-2026 enterprise agent RFPs ask about agent identity scoping, agent-to-agent audit and kill switches.
  - All of this comes from vendor blogs. I did not see the primary SIG or CSA documents.
  - https://accuroai.co/blog/enterprise-agent-rfp-30-procurement-questions
  - https://aliteq.com/ai-security-questionnaire-answers-2026
  - https://www.aetos-data.com/answers-insights/enterprise-security-ai-questionnaires
  - https://whisperly.ai/sig-questionnaire
- **Policy signals:** NIST RFI on AI agent security (2026-01-08): https://www.federalregister.gov/documents/2026/01/08/2026-00206/request-for-information-regarding-security-considerations-for-artificial-intelligence-agents. The OWASP Top 10 for Agentic Applications was published in December 2025.
- **Incidents.** In February 2026, Claude Code ran `terraform destroy` with auto-approve against DataTalks.Club production, deleting the VPC, ECS, RDS and snapshots; AWS later restored it. https://vibegraveyard.ai/story/claude-code-terraform-datatalks-infrastructure-destruction/ and https://aipolicytracker.org/ai-risk/incidents/1424. There was also the Replit agent production-database deletion in July 2025.
- **Vanta data (May 2026).** Fewer than half of organizations deploying agents have a formal AI policy: https://www.vanta.com/resources/ai-agents-lacking-ai-policies

### Inferences
- **The pain the founder's product addresses is safety and blast radius first, compliance second.** The incidents above happened at companies where an approval gate would have prevented damage. None of them failed an audit.
- **Compliance pull is episodic.** It shows up when an auditor or customer asks, typically during readiness and the first Type II, and buyers satisfy it with the cheapest acceptable control, often "agents have no production write access".
- **The volume of supply-side SEO is a sign of a crowded pitch, not proven demand.** About 20 vendors publish "SOC 2 for AI agents" guides, including hoop.dev, MintMCP, Teleport, Pushary, Preloop, containment.ai, onpilot, policylayer and sentrely.

### Gaps
- Reddit (r/soc2, r/cybersecurity, r/devops, r/startups) was unreachable.
- I found no quantitative data on how often auditors now raise agent-change exceptions.
- I found no primary SIG 2026 or AI-CAIQ question text.

---

## Q5. Price anchors

### Takeaway
Startups pay roughly $7.5k–$25k a year for a compliance platform, plus $5k–$50k for the audit, so the all-in first-year Type II is $25k–$95k. Incumbent add-ons are priced at $3k–$22k a year. Agent-approval tools sold self-serve are priced like developer tools, at $10–$350 a month. A compliance-evidence add-on for small startups plausibly lands at **$300–$3,000 a year**: $29–$99 a month self-serve, with an "audit pack" tier near $2–3k a year. It is hard to charge more while the incumbents may bundle the feature.

### Cited findings (all third-party estimates; vendors mostly don't publish prices)

**Compliance platforms**

| Vendor | Reported price | Source |
|---|---|---|
| Vanta | Small teams ~$10–12k/yr. Vendr median ~$20k; observed range $7.5k–$57k | https://soc2auditors.org/insights/vanta-pricing/ and https://www.vendr.com/marketplace/vanta |
| Vanta add-ons (AWS Marketplace) | $6k–$22.25k per 12 months (Trust Center $6k, Questionnaire Automation $10k, TPRM $13.6k) | https://soc2auditors.org/insights/vanta-pricing/ |
| Drata | From ~$7.5k/yr; small companies $10–15k; Vendr median ~$25k | https://soc2auditors.org/insights/soc-2-software-pricing-comparison/ |
| Secureframe | From ~$7.5k/yr (Fundamentals); Vendr median ~$20k | https://soc2auditors.org/insights/soc-2-software-pricing-comparison/ |
| Sprinto | $9.5k starting on AWS Marketplace, audit separate; Vendr median ~$15k | https://www.complyjet.com/blog/sprinto-review |
| Oneleet | $8k–$30k all-in quotes, including vCISO and audit in some packages | https://www.complyjet.com/blog/oneleet-vs-delve and https://www.strac.io/blog/oneleet-alternatives |
| Delve | ~$10–15k, audit reportedly included; reputational overhang since March 2026 | https://www.complyjet.com/blog/oneleet-vs-delve |
| Comp AI | Bundled platform, audit and pentest quoted as one fee. A $797/month figure comes from a vendor-affiliated source. The self-hosted edition is free. | https://getsecureslate.com/blog/top-7-affordable-compliance-platforms-for-smart-compliance |

**Audit fees (2026)**
- Specialist firms: Type 1 about $5k–$35k; Type 2 about $15.5k–$50k.
- Big Four: Type 2 $65k–$200k.
- Platform-partnered Type 1 for an early-stage company: $2.5k–$7.5k.
- Sources: https://soc2auditors.org/soc-2-audit-cost/ (September 2026) and https://www.konfirmity.com/blog/soc-2-audit-cost
- HN anecdotes: "~$30k and 100 hours" for a Type 2, and "~$15k" from an audit firm (thread from ~May 2026).

**Agent-approval tools**
- **Preloop:** approvals are in the free Apache-2.0 core. The cloud plans per a GitHub pricing PR are Pro $10–12/mo, Team $100–120/mo and Business $300–350/mo; the pricing page structured data showed Teams at $29. https://preloop.ai/pricing and https://github.com/preloop/preloop/pull/760
- **Pushary:** $9.99/mo (Agent) to $99/mo (Partner), self-serve. Pages are inconsistent, with one mentioning up to $799. https://pushary.com/vs/humanlayer
- **HumanLayer:** the old approvals API had a free tier and a $500/mo Premium plan. It is now reportedly deprecated after a pivot to an AI IDE at $100/user/mo, per competitor pages. https://pushary.com/vs/humanlayer and https://orako.io/compare/humanlayer-alternative
- **Natoma:** free up to 5 MCP servers and 5k tool calls per month; Pro from 20k calls. https://natoma.ai/pricing
- **Lunar MCPX:** Free, Pro (≤100 users), Enterprise. HITL approval workflows appear in the plan grid. https://www.lunar.dev/pricing
- **Auth0 for AI Agents:** async authorization (CIBA) is included in the free tier. Essentials starts at $35/mo, per third parties. https://auth0.com/blog/auth0-for-ai-agents-generally-available/
- **MintMCP and Runlayer:** no public pricing; demo-led. https://www.truefoundry.com/blog/runlayer-pricing

### Inferences
- **Incumbent add-on anchors ($6k–$22k) are enterprise-ish.** A startup with a card who is already paying ~$10–20k for Vanta will baulk at more than ~$100/mo for a narrow evidence feature unless an auditor finding forces it.
- **Open-source Preloop and Auth0's free CIBA tier anchor the "approval" primitive near $0.** The defensible paid value has to be the **audit-ready evidence**: receipts mapped to CC8.1 and A.8.32, sampling-friendly exports, and a Vanta/Drata push.

### Gaps
- Vanta and Drata agent-governance pricing is unknown.
- Vendor list prices were unverifiable because pages could not be fetched.

---

## Q6. Existing products selling approval evidence for AI agent actions

### Takeaway
The "approve an agent action via Slack, Teams or phone, with an audit trail" primitive is **already sold by many vendors**, several of them explicitly pitched at SOC 2. They span:
- open-source and self-serve tools: Preloop, Pushary, Lunar MCPX, Natoma, Obot
- infrastructure access gateways: hoop.dev, Teleport, StrongDM (now Delinea)
- identity platforms: Auth0 CIBA
- enterprise agent security: Zenity, Runlayer, MintMCP
- compliance incumbents moving in from above: Drata Agent Governance, Vanta AI Governance

**Nobody I found is clearly positioned as "self-serve SOC 2 change-approval receipts for agent actions that drop into Vanta or Drata".** That wedge is narrow and copyable. Signed receipts, a "once" (single-use) binding and approval by phone call are differentiators, but small ones: Pushary already does signed webhooks, a durable ledger and phone approvals, and Preloop already does a hash-chained audit.

### Cited findings

| Product | What it does relevant here | GTM / pricing | Traction / notes |
|---|---|---|---|
| **Preloop** | Open-source agent control plane. MCP firewall that can hold a tool call for approval (mobile, watch, Slack, Mattermost, email, CLI); hash-chained audit trail; supports Claude Code, Codex and Cursor. https://preloop.ai/ and https://github.com/preloop/preloop | Self-serve. Approvals are free in Apache-2.0; paid cloud tiers ~$10–$350/mo add quorum, escalation and RBAC (sources conflict). | Show HN https://news.ycombinator.com/item?id=46693373. Active GitHub repo; traction numbers unknown. The closest functional competitor. |
| **Pushary** | Agent asks a human by phone call, web or Slack; fail-closed; durable, exportable decision ledger; signed webhooks; records which surface answered; auto-approval rules are logged. https://pushary.com/human-in-the-loop-hermes and https://pushary.com/blog/ai-agent-audit-log-compliance | Self-serve, $9.99–$99/mo (inconsistent pages). | Unknown; small. Feature set nearly identical to the founder's. |
| **hoop.dev** | L7 gateway for databases, Kubernetes, SSH and HTTP. Pauses risky commands (e.g. DROP DATABASE) for an approver and logs the decision with the session. Many "SOC 2 for AI agents / MCP / Claude Agent SDK" posts. https://hoop.dev/blog/soc-2-compliance-for-ai-agents and https://hoop.dev/blog/what-soc-2-means-for-the-claude-agent-sdk | Open-source core plus paid plans; pricing not found. | Heavy SEO on exactly this query space. |
| **Teleport** | Cryptographic agent identity; MCP gateway; JIT access requests approved via Slack, Teams, PagerDuty or Jira; maps evidence to CC6.3. https://goteleport.com/blog/ai-agents-soc-2/ and https://goteleport.com/blog/secure-ai-agents-zero-code-mcp/ | Sales-led for Enterprise; has a community edition. | Established. |
| **StrongDM (Delinea)** | HITL approvals via Slack or Teams; MCP gateway authorizes each tool call (Claude Code, Codex, Copilot, Kiro). https://www.strongdm.com/ | Sales-led. | Established. Acquisition date not verified. |
| **Auth0 for AI Agents** | Async authorization via CIBA plus Rich Authorization Requests; human approves on phone (Guardian or email). GA November 2025; Auth for MCP May 2026. https://auth0.com/ai/docs/intro/asynchronous-authorization | Self-serve; included in the free tier. | Okta distribution. A primitive, not compliance evidence. |
| **Lunar.dev MCPX** | MCP gateway with "HITL: Human Approval Workflows" and audit trails. https://www.lunar.dev/pricing | Free, Pro, Enterprise (per seat). | Unknown. |
| **Natoma** | Hosted MCP platform. https://natoma.ai/pricing | Usage-based self-serve; free tier. | Approval features not confirmed. |
| **Obot** | Open-source MCP gateway. | Free self-hosted; managed and enterprise tiers. | Approval features not confirmed. |
| **MintMCP** | MCP gateway, SOC 2 Type II; audit trails of MCP interactions, access requests and configuration changes; publishes "SOC 2 for AI agents" content. https://www.mintmcp.com/blog/soc-2-ai-agents | Demo-led; no public pricing. | Human approval not confirmed. |
| **Runlayer** | MCP and agent control plane. https://theaiinsider.tech/2025/12/02/runlayer-emerges-from-stealth-with-11m-to-secure-the-mcp-era/ | Demo-led; fee scoped per deployment. | $11M seed (December 2025); enterprise OpenClaw offering (VentureBeat). |
| **Zenity** | "approve, modify or block agent actions dynamically"; kill switch. https://siliconangle.com/2026/08/03/israeli-startup-zenity-bags-125m-funding-build-security-layer-ai-agents/ | Enterprise sales. | $125M Series C (2026-08-03); Gartner "Company to Beat" (April 2026). |
| **Astrix** | Non-human identity security. | Enterprise sales. | Cisco announced intent to acquire (May 2026, per one roundup; unverified). |
| **Kosli** | Records every software-delivery change as SOC 2 evidence; automated change gates; "governance infrastructure for AI SDLC". https://www.kosli.com/soc2/ | Sales-led; pricing not found. | Adjacent: change evidence, not per-action human approval. |
| **Drata AI Agent Governance** | Inline policy enforcement plus tamper-evident Chain of Custody mapped to SOC 2 and ISO. | Limited Availability, enterprise, Anthropic-first. | Launched 2026-08-04. |
| **Vanta AI Governance** | Agent inventory, boundaries and out-of-bounds detection. | Early access. | Launched 2026-07-30. |

Not covered: Credal, Aembit, Lasso, Prompt Security and "Pylon" returned no relevant approval or evidence detail in my searches. Prompt Security and Lasso are prompt- and data-security tools. "Pylon" may be a mix-up with another vendor.

### Inferences
- **The functional product is already available free or cheap** (Preloop OSS, Auth0 free tier, Pushary at $10/mo).
- **The compliance framing is already used in SEO** by hoop.dev, MintMCP and Teleport.
- **Incumbents are adding the evidence layer** (Drata Chain of Custody).
- **What remains open is narrow.** It amounts to receipts formatted for an auditor's sampling test (who, what exact diff or command, when, single use), CC8.1 and A.8.32 mapping, and native evidence push into the compliance platform the startup already uses, all sold self-serve to 5–50 person companies. That is a feature, and easy for Preloop, Pushary or Drata to add.

### Gaps
- No traction metrics for Preloop, Pushary, hoop.dev, Lunar or Natoma.
- No hoop.dev, Kosli or MintMCP pricing.
- Credal, Aembit and Lasso not assessed.

---

## Opportunity rating

| Dimension | Rating | One-line justification |
|---|---|---|
| **Demand evidence** | **Weak** as a standalone compliance purchase. **Moderate** as a safety feature. | Approval is required by CC8.1, A.8.32 and PCI 6.5.1. But criteria are technology-neutral and auditor-dependent: Amp passed Type II without PR approvals. Most agent code already flows through PRs that Vanta and Drata evidence. "Agents have no production write access" is a free alternative control. Agent-approval pain is mostly vendor-asserted, with few organic buyer complaints found. |
| **Competitive gap** | **Contested** and narrowing. | Preloop (OSS), Pushary ($10–99/mo, near-identical features), hoop.dev, Teleport, StrongDM, Auth0 CIBA and Lunar already sell approvals with audit trails, several pitched at SOC 2. Drata (August 2026) and Vanta (July 2026) are adding agent governance with tamper-evident logs. Only the "auditor-ready receipt pushed into Vanta/Drata for startups" niche is unoccupied, and it is copyable. |
| **Self-serve buyable?** | **Yes, at developer-tool prices ($20–$100/mo). Not as a compliance line item.** | Peers prove card-swipe pricing for approval gates. But Vanta public integrations can't create custom tests, so evidence won't auto-satisfy a control; it has to be pushed as documents or via a private integration. Buyers spend on compliance only when an auditor or customer forces it, and then pick the cheapest acceptable control. |

**Bottom line.** Selling "audit evidence that a human approved what the agent changed in production" as a pull-driven, self-serve compliance purchase looks **weak** in October 2026. The requirement exists in principle, but no authoritative body has said agent actions need per-action human approval receipts. Auditors accept alternatives, and the approval primitive is commoditized.

The more defensible framing is to sell the gate as **blast-radius protection for agents with production credentials**, where the incidents create urgency, and make the compliance receipt a **retention and expansion feature**:
- CC8.1, A.8.32 and PCI 6.5.1 mapping
- a sampling-ready CSV/PDF export
- a Drata Custom Connection, a Secureframe Trust API push, and a Vanta document push or private-integration recipe

**Two specific risks:**
1. Solo or very small teams cannot show segregation of duties when the approver is the same person who launched the agent. Some practitioners say that approval is weak or insufficient. That undercuts the core ICP's evidence value unless the product supports routing to a second named human, such as an advisor or fractional CISO.
2. Drata or Vanta could add a "request human approval" step to their agent-governance products.

**Cheap validation before building more** (inference): ask 5–10 auditors at startup-focused firms (Prescient, Johanson, Insight Assurance, Advantage Partners) whether they would accept per-action approval receipts as CC8.1 evidence for direct agent production actions, and whether they have issued exceptions for unapproved agent changes in 2026. Separately, run a landing-page test aimed at "SOC 2 + Claude Code with production credentials".

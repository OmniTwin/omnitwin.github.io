# Routed approvals as a demand engine, and approvals that feed agent context (as of 16 Oct 2026)

Question: can a request routed to ANOTHER person for approval generate its own demand (the approver becomes a user or buyer), and can approvals of AI-agent actions feed back into agent context? The answer is written for Decidio, a solo-founder approval gate. Its features: an agent or person asks; a policy answers proceed/route/block; routed requests reach a named approver over Slack, Teams, the app, the web, or a WhatsApp/SMS/email link with no account needed; decisions are sealed with signed receipts; refusals are remembered; sealed decisions can be served back to agents as context packs.

**Method note.** WebFetch failed with DNS errors on every domain tried (gotohuman.com, mixpanel.com, sacra.com, docs.gotohuman.com). Direct curl is blocked by the egress proxy (403 on filestage.io and gotohuman.com). reddit.com is refused by the search tool ("not accessible to our user agent"). Every web finding below is therefore from a **search-result summary** of the linked page, not a full read of it. GitHub issue data came through the GitHub MCP search API on 16 Oct 2026. Counts shown are 👍 (+1) reactions from that API. Single-issue reads of anthropics/claude-code and openai/codex were refused (repos not in the session allow-list), and the search API rate-limited once. I ran about 55 web searches. Pre-2026 facts are flagged (pre-2026).

---

## Q1. Viral loops in routed-request products: do recipients become users or buyers?

### Takeaway
Recipient-driven growth is real and well documented qualitatively for DocuSign, Calendly, Loom, PandaDoc and BILL. **Almost no company publishes a recipient-to-signup rate or share of signups from recipients.** The few hard numbers are aggregate (DocuSign 130k to 300k new users a day; Loom 1.1M registered users with zero paid marketing; BILL over 50% of customers from the accountant channel). B2B viral coefficients are typically 0.2 to 0.7, below 1. The loops that worked share four traits:
1. The recipient has the **same job** as the sender (they also need to schedule, sign, send video or pay bills).
2. The recipient gets full value with **no account**.
3. The recipient surface carries **branding**.
4. A **signup prompt appears after the action is complete**, and sometimes a recipient-only feature (PandaDoc's recipient inbox) gives a reason to keep an account.

The strongest B2B loop with money attached (BILL) runs through a **hub intermediary** (accountants serving many clients), not through the one-off recipient.

### Cited findings
**DocuSign**
- Snippets describe signers as a pool of later senders. "130,000 new unique users every day" is the most-cited network figure. A DocuSign signer-funnel A/B test "increased free account creation by 15%", which is a lift, not a share. Source: [Mixpanel customer story](https://mixpanel.com/customers/docusign-accelerates-growth) (search summary).
- A 2017 DocuSign release reported "an average of 300,000 new users joining the platform every day", with no split between signers and direct signups. Source: [GlobeNewswire 2017](https://www.globenewswire.com/news-release/2017/07/19/1054229/0/en/Zuora-Helps-DocuSign-Accelerate-Growth-of-DocuSign-Global-Trust-Network-to-Reach-200-Million-Subscribers.html) (search summary; pre-2026).
- The 2018 S-1 describes a "marketing halo" where life events (job offers, home purchases) drive adoption at the signer's employer. It does not disclose a channel split. Source: [DocuSign S-1](https://www.sec.gov/Archives/edgar/data/0001261333/000119312518099555/d506878ds1.htm) (search summary; pre-2026).
- In 2012 DocuSign said users grew from 6M to 13M in 2011 and credited the "viral network effect". Source: [BusinessWire 2012](https://www.businesswire.com/news/home/20120119005433/en/DocuSign-Accelerates-Momentum-in-Breakout-2011) (pre-2026).
- Design: "a free signer account lasts forever, and signing and storing documents is always free". The free flow nudges a signer to become a sender, with some regional pages capping free sends at 10 envelopes. Source: [DocuSign free account](https://www.docusign.com/free-account) (search summary).

**Calendly**
- Wikipedia says Calendly "has been recognized for its viral nature" because invitees could be encouraged to try the software. Source: [Wikipedia](https://en.wikipedia.org/wiki/Calendly) (search summary).
- Analyst pages describe a "Powered by Calendly" badge on free booking pages and a post-meeting prompt to create an account. They put ARR at about $40M (2019), $125M (2021) and $270M (end of 2023), driven by invite-led adoption. These are AI-generated analyst pages, so treat them as secondary. Sources: [Sacra](https://sacra.com/chat/h/e375eaca-7657-4575-9b73-660494b47100/), [Sacra](https://sacra.com/chat/h/bdf9ad15-84ac-49ac-b236-07cbe1e52d0b/) (search summary).
- The only "rate" found is an **unsourced estimate that 5 to 10% of recipients try Calendly**. Source: [IdeaPlan case study](https://www.ideaplan.io/case-studies/calendly-single-feature-to-platform) (search summary, no underlying data).
- Scale: 10M active users in 2022 ([startupgtm](https://startupgtm.substack.com/p/calendly-growth-story-a-viral-product), secondary). Used by 86% of the Fortune 500 by June 2024 ([Contrary Research](https://research.contrary.com/report/calendly)).

**Loom**
- A Feb 2019 founder interview reports 1.1M registered users and 200k MAU, "entirely through organic and viral channels with no paid marketing". Source: [Latka interview with Shahed Khan](https://getlatka.com/companies/loom/interviews/loom-shahed-khan-2019) (search summary; pre-2026).
- Design: the viewer is prompted to sign up after watching so they can reply with their own Loom. Reacting, commenting or pausing triggers prompts such as "add their name to the reaction". Source: [onboardme](https://onboardme.substack.com/p/looms-975m-user-onboarding-secret-perfect-timing-product-led-growth-loop) (search summary).
- The claim that "Loom gets 30% of signups from shares of watermarked videos" appears only on a generic SEO guide with no primary source, so treat it as **unverified**. Source: [fungies.io](https://fungies.io/?p=39849).

**PandaDoc**
- A **recipient inbox** needs a free recipient account. "The signup link will be displayed to your recipients after they complete their first PandaDoc document." Source: [PandaDoc changelog](https://updates.pandadoc.com/the-new-recipient-inbox/) (search summary).
- No recipient-to-signup figure was found. About $100M ARR and 56k customers (2024) per [Sacra](https://sacra.com/research/pandadoc) (secondary).

**Dropbox Sign / HelloSign**
- At the 2019 acquisition: 80k+ customers ([Motley Fool](https://www.fool.com/investing/2019/02/18/hellosign-acquisition-will-help-dropbox-expand-bus.aspx)), about 55k of them paid ([FrenchWeb](https://www.frenchweb.fr/hellosign-signature-electronique/408939)).
- The free tier (3 documents a month) "attracted millions of users". No signer-conversion figure was found. All search summary; pre-2026.

**Typeform (counter-example)**
- The "powered by Typeform" button drove most traffic, and copy tests raised its click-through rate 200%. But a sign-up-deferred strategy "didn't significantly increase actual sign-ups or net active users". Sources: [Marketcurve](https://marketcurve.substack.com/p/from-agency-to-unicorn-the-typeform), [Airtribe](https://airtribe.substack.com/p/takeoff-59-cracking-70-million-arr) (search summary).
- Lesson: exposure without a recipient need does not convert.

**BILL (payables and approvals network)**
- In the Q4 FY2024 earnings call, management said accountants "represent our largest customer acquisition channel and more than half of our customers are from the accounting channel". Source: [Insider Monkey transcript](https://www.insidermonkey.com/blog/bill-com-holdings-inc-nysebill-q4-2024-earnings-call-transcript-1339404/) (search summary; pre-2026).
- Over 8.3M network members had originated or received a payment by 30 June 2025. An analyst calls the network a "low-cost customer acquisition channel". Sources: [BILL developer docs](https://developer.bill.com/docs/bill-network), [Canalyst](https://canalyst.com/blog/bill-ing) (search summary).
- A 2026 article says 98 of the top 100 US accounting firms partner with BILL (March 2026). Source: [yespress](https://yespress.io/bill) (secondary).

**Approval-specific products**
- ApprovalMax:
  - From August 2026 it moves all customers to "tiered usage-based pricing" sized by **approver count and monthly documents** ([ApprovalMax blog](https://blog.approvalmax.com/weve-changed-the-way-we-charge)).
  - The prior entry tier covered 100 documents and 5 approvers a month ([ApprovalMax pricing update](https://approvalmax.com/pricing-approvalmax-for-quickbooks-online-update-2025)). Capterra lists a $59.40/month starting price for Xero ([Capterra](https://www.capterra.com/p/176713/ApprovalMax/pricing/)).
  - Accounting is the top reviewer industry (20% of 29 reviews).
  - Approvers are a **billing unit**, not a growth loop. All search summary.
- Zapier Human in the Loop:
  - "Every designated reviewer needs a Zapier account, and you'll need to share the Zap with reviewers."
  - The blog says that on Pro you "can only send requests to yourself". The help centre lists different plan availability, so the sources conflict.
  - Sources: [Zapier HITL guide](https://zapier.com/blog/human-in-the-loop-guide/), [Zapier help](https://help.zapier.com/hc/en-us/articles/38731264910733/) (search summary). Zapier's design **blocks** the recipient loop.
- Pushary (verified Sept 2026 on an aggregator):
  - $9.99/month for a single agent, a **Team plan at $59.99/month for up to 10 members**, and **Partner tiers from $99/month** ([Toolradar](https://toolradar.com/tools/pushary/pricing)).
  - The Partner plan is for companies whose agents act for *their* customers. "Before your agent refunds money, sends a message or deletes data, Pushary asks the customer for approval… Connect their phone once with a keyless one-tap link… No account, no app, and no key on their side." Decisions are logged and exportable, with signed webhooks. Source: [Pushary Partner plan](https://pushary.com/nl/partners) (search summary).
  - **This is direct overlap with Decidio's no-account approver.**

**Benchmarks**
- B2B K is "mostly below 1"; 0.2 to 0.5 counts as "some viral growth"; most SaaS sits at 0.3 to 0.7. B2B virality comes mainly from collaboration invites. Sources: [Basedash](https://docs.basedash.com/startup-metrics/growth-metrics), [Growthspree](https://www.growthspreeofficial.com/blogs/viral-word-of-mouth-loops-b2b-saas.md), [command.ai](https://command.ai/blog/viral-coefficient-saas) (blogs, search summary).

### Inferences
- **Approval is asymmetric, and that breaks the classic loop.**
  - A Calendly invitee also has meetings to schedule. A DocuSign signer will one day need a signature. A Loom viewer can reply in kind.
  - An approver's job is to say yes or no for someone else. The approver is usually *more senior* (client, manager, owner, accountant), so their natural next step is to **require** approvals of others (becoming a buyer or admin), not to **send** requests.
  - The recipient-to-sender path that powered DocuSign and Calendly is therefore weak for approvals.
- **The money loop is hub-shaped.** BILL's biggest channel is the accountant who serves many clients, and ApprovalMax prices on approvers. Filestage, Ziflow and GoProof give reviewers away free and charge the agency. In each case the payer is the party with many counterparties, and the counterparties stay free.
- **Design choices worth copying:**
  - No-account action (DocuSign, Pushary Partner).
  - Branded surface (Calendly, Typeform).
  - A signup prompt **after** the action completes, never before (PandaDoc, Loom).
  - **A reason for the recipient to own an account**: PandaDoc's recipient inbox. For Decidio the analog is an "approver ledger": every decision you have made, across every sender, with your reasons and receipts.
- **Typeform is the cautionary tale.** Exposure alone does not create signups when the recipient lacks the sender's need.

### Gaps
- No primary recipient-to-signup or recipient-share figure was found for DocuSign, Calendly, Loom, PandaDoc, Dropbox Sign, Typeform, Ironclad, Linear or Jira guests, or Zapier approval recipients.
- No Pushary traction or Partner-plan price was confirmed (the official page could not be fetched).
- Linear/Jira guest conversion and Ironclad counterparty conversion were not researched (search budget).

---

## Q2. Who is the natural SECOND person for AI-agent actions in small firms and freelancing, and what do they use today?

### Takeaway
The only segment with **paid, established demand for a second person** is **agency-client sign-off on creative and content**: Filestage, Planable, Ziflow and GoProof, priced at $33 to $399 a month and always with **unlimited free reviewers**. It covers artifacts (posts, videos, PDFs), not agent actions.

For agent *actions*, the second person is almost always built into the vertical platform for free:
- **Code:** GitHub requires a different person to approve a Copilot-created PR.
- **Outbound email:** Instantly's human-in-the-loop queue with Slack routing.
- **Bookkeeping:** agents draft and the bookkeeper or owner approves.
- **Automation:** n8n's native tool approvals.

Direct evidence that clients demand sign-off on an agency's or freelancer's *agent actions* (sends, bookings, ad spend, deploys) is thin and mostly vendor-blog. I found no evidence for parent or guardian approval of agent actions.

### Cited findings
**Agency client approval (content and creative). The category exists and is paid.**
- Filestage:
  - Free plan with 1 active project, 5 files a month and **unlimited reviewers**. Starter $199/month billed yearly ($249 monthly). Business $329 to $399/month (sources conflict). Sources: [Capterra](https://www.capterra.com/p/162035/Filestage/pricing/), [PricingSaaS](https://pricingsaas.com/companies/filestage) (search summary).
  - Traction: Latka *estimates* 2024 revenue of about $4M and 400 customers on $3.3M total funding; Bitscale says $2.5M ([Latka](https://getlatka.com/companies/filestage)). €2.8M Series A in Jan 2021 ([tech.eu](https://tech.eu/2021/01/12/filestage-series-a/)). All search summary; estimates conflict.
- Planable: Basic $33 to $39 per workspace per month, Pro $49 to $59 per workspace per month. Free-plan status conflicts between sources (50 lifetime posts vs none). Sources: [PostPlanify](https://postplanify.com/planable-pricing), [CostBench](https://costbench.com/software/social-media-management/planable/). "5,000+ marketing teams" ([Capterra](https://capterra.com/p/171744/Planable/)) vs "nearly 7,000" ([RecurPost](https://recurpost.com/compare/recurpost-vs-planable/)). All search summary.
- Ziflow: "$199/month Standard, unlimited proofs and reviewers" ([Toolradar](https://toolradar.com/tools/ziflow/pricing)) vs "$199 flat per year" ([Capterra](https://www.capterra.com/p/178111/Ziflow/pricing/)). The sources conflict tenfold.
- GoProof: Connect from $129/month for 2 licences, with unlimited proofs, reviewers and guests ([G2](https://g2.com/compare/goproof-vs-ziflow)). Search summary.
- Agency practice (vendor blogs):
  - Email approvals "work at 5 clients and barely work at 10".
  - "When a client claims they never approved something, there's no audit trail to check."
  - Hands-off clients get auto-approve, and committee clients get multi-level chains.
  - Source: [Apaya](https://apaya.com/blog/social-media-approval-workflows-for-agencies) (search summary).
- More vendor-blog practice:
  - A two-gate pattern: internal editorial pass, then client approval, with publishing locked until both clear ([Cloud Campaign](https://www.cloudcampaign.com/answers/review-and-approve-ai-content)).
  - 2026 predictions: clients will ask which parts were AI-generated, and approvals will shift upstream to strategy and data ([Gain](https://blog.gainapp.com/ai-content-approval-process-predictions/)).

**Outbound and AI SDR**
- Instantly's human-in-the-loop mode queues AI-drafted replies for approval and can route approvals through Slack ([Instantly](https://instantly.ai/blog/best-ai-sdr-tools-outbound-b2b-sales-2026/)).
- One buyer guide calls "does a human approve every email before it sends?" the "single most important question" ([FirstSales](https://firstsales.io/blog/ai-sdr-demo-questions/)).
- Practitioner advice is tiered review by risk ([pravinkumar.co](https://www.pravinkumar.co/blog/when-should-a-human-approve-an-ai-drafted-outbound-email-2026)).
- All search summary. Approval is a **built-in feature** of the sending tool.

**Code (founder, lead or contractor)**
- GitHub: "you can approve the pull request, but your approval won't count toward the required number of approvals… someone else must approve" a Copilot-created PR ([GitHub docs](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/cloud-agent/review-copilot-prs)).
- On 1 Sept 2026 GitHub let admins authorize **Copilot code review to submit approvals that count** toward required reviews (off by default) ([GitHub changelog](https://github.blog/changelog/2026-09-01-copilot-code-review-can-now-approve-pull-requests)).
- Both search summary. The second person in code is native, and is now partly being automated away.

**Freelance and client deliverables**
- Contract templates now include acceptance windows and AI-generated-code disclosure clauses ([Layer3 Labs template](https://www.layer3labs.io/contracts/software-development-contract-template)).
- Upwork posts describe "reviewing the code AI produces… deciding what ships" ([Upwork listing](https://www.upwork.com/jobs/Powered-Web-Developer_~022065597149476567749/)).
- No evidence of per-action client approval of a freelancer's agent. Search summary.

**Bookkeeping and accounting**
- Agentic bookkeeping is described as agents drafting while "the bookkeeper still approves categorization, accruals…", with liability staying with the firm ([beancount.io, Aug 2026](https://beancount.io/blog/2026/08/12/agentic-ai-bookkeeping-platforms-2026-guide)).
- The QuickBooks Payroll Agent "can draft a pay run for approval but won't run payroll by itself", and practitioners advise second approval for payroll and large sales-tax entries ([beancount.io](https://beancount.io/ru/blog/2026/08/12/quickbooks-payroll-sales-tax-ai-agents-2026-guide)).
- The Intuit Payments Agent (Aug 2026) handles receivables, not bill pay ([selfemployed.com](https://www.selfemployed.com/news/quickbooks-intuit-intelligence-ai-bookkeeping-2026/)).
- All search summary. The incumbent approval rails are ApprovalMax and BILL (Q1).

**Parent or guardian**
- No agent-purchase parental approval was found. Apple's Ask to Buy is the closest template: the purchase pauses and a guardian approves on their own device ([PPC Land](https://ppc.land/ask-to-buy/)). ChatGPT teen controls cover hours and features, not purchases ([kidsaitools](https://www.kidsaitools.com/en/articles/parental-controls-ai-apps-guide-2026)). Search summary.

**SMB trust context**
- Bluehost, May 2026, n=350 US small businesses: "fewer than 1 in 15 owners fully trust AI". Top concerns are accuracy and quality (32%) and losing the human touch (29%). No figure on wanting human approval ([Bluehost study](https://www.bluehost.com/state-of-small-business-ai-confidence)). Search summary.

### Inferences
- **Natural second persons, ranked by how routinely they exist today:**
  1. The agency's client (content and outbound).
  2. The bookkeeper or owner (payments).
  3. The PR reviewer (code).
  4. The manager (junior or contractor agents).
  5. The parent (no evidence).
- Only #1 has a standalone paid tool category, and there the **reviewer is free**.
- **Decidio's fit with content proofing is poor.** Those tools win on visual previews, annotations and versioning, not a yes/no gate.
- Decidio's fit is better where the agency's agent takes **actions with consequences**: sending to the client's customers, posting live, spending budget, booking or refunding. Its no-account WhatsApp/SMS approver and sealed receipt answer the "client claims they never approved" dispute that agency blogs name.
- A "second person" for code is strongly served by GitHub. Fighting there is low-yield.

### Gaps
- Reddit (r/agency, r/freelance, r/marketing) could not be searched (blocked), and Upwork/Fiverr community threads were not found. Practitioner sentiment is therefore missing, and all agency-demand evidence is vendor-blog.
- No figure on how many agencies or clients require pre-action sign-off for agent actions specifically.
- Planable and Filestage reviewer-conversion data (do reviewers ever buy?) was not found.

---

## Q3. Approval fatigue and learning: do developers want approvals to teach the agent? Does anyone sell "approvals become policy or context"?

### Takeaway
Developer demand overwhelmingly points to **fewer approvals**, not to approvals that teach. Anthropic made **auto mode the default on 14 Aug 2026**, reportedly saying users approve 97% of prompts. Cursor shipped an **Auto-review** classifier mode. Codex ships an **auto_review** approvals reviewer. GitHub lets Copilot approve PRs.

"Approvals become policy" already ships **first-party and free** for the operator's own allow rules: Claude Code's bundled `/fewer-permission-prompts` (also `/less-permission-prompts`) writes repeated approvals into the committed project `.claude/settings.json`. Shareable allow/deny files exist in Cursor's CLI too.

What is **not** served:
1. Denials and their **reasons** propagating as durable, shared memory. The signals are real but low-vote, and several were closed "not planned".
2. Approvals made by **someone other than the operator** feeding agent context.

"Decision traces as context" is an investor thesis (Foundation Capital's "context graphs") and an open-source pattern (Agno decision log, a GitLab design doc). No SMB product sells it.

### Cited findings
**Fewer prompts (high-vote)**
- anthropics/claude-code:
  - [#11799](https://github.com/anthropics/claude-code/issues/11799) "Auto-approve WebSearch… via permissions configuration": 47 👍, closed completed.
  - [#2560](https://github.com/anthropics/claude-code/issues/2560) "keeps asking for permission despite already having it": 36 👍, closed completed (2025).
  - [#91683](https://github.com/anthropics/claude-code/issues/91683) bypassPermissions regression: 27 👍, open (Sept 2026).
  - [#98159](https://github.com/anthropics/claude-code/issues/98159) "claude.ai: let users set a default permission mode, incl. 'Skip all approvals'": 18 👍, open (29 Sept 2026).
  - [#80716](https://github.com/anthropics/claude-code/issues/80716) auto-mode classifier causing "repeated manual approval fallback": 16 👍, open.
- openai/codex:
  - [#3140](https://github.com/openai/codex/issues/3140) "all commands… need approval": 25 👍, closed.
  - [#10187](https://github.com/openai/codex/issues/10187) "repeatedly asks… despite auto-approve settings (dozens of times per response)": 9 👍, open.
  - [#10735](https://github.com/openai/codex/issues/10735) "Approval decisions are not remembered": 5 👍, closed as duplicate.
  - [#44111](https://github.com/openai/codex/issues/44111) "project-scoped 'Always allow'": 1 👍.

**Auto mode (secondary coverage, search summary)**
- Launched as a research preview in March 2026; default for Pro, Max and Team from 14 Aug 2026.
- A classifier (reportedly Sonnet 4.6) blocks out-of-scope or hostile actions, and the system falls back to manual approval after 3 consecutive or 20 total blocks.
- Anthropic-attributed claims: the classifier caught 89% of dangerous commands vs 13.6% for human reviewers (n=1,053 paid testers), and "users approve 97% of permission prompts".
- Sources: [AI Weekly](https://aiweekly.co/alerts/anthropic-flips-claude-code-to-auto-mode-by-default-aug-14), [Gigazine](https://gigazine.net/gsc_news/en/20260810-claude-code-auto-mode/), [Backslash](https://www.backslash.security/blog/claude-code-auto-mode-catches-a-lot-but-not-everything), [Simon Willison](https://simonwillison.net/b/9394). Primary Anthropic posts were not retrieved.
- Auto-mode complaint issues: [#85588](https://github.com/anthropics/claude-code/issues/85588) "Auto mode silently denies tool actions; denials never surface to the user" (4 👍); [#92279](https://github.com/anthropics/claude-code/issues/92279) "let a classifier block fall back to a permission prompt instead of a hard deny" (8 👍); [#100331](https://github.com/anthropics/claude-code/issues/100331) "Auto mode classifier blocks user-approved actions; no override available over Remote Control" (0 👍, Oct 2026).

**Denials and reasons as context (real, low-vote)**
- [#4587](https://github.com/anthropics/claude-code/issues/4587) "Sub agents context is wiped when the user denies an action": 23 👍, **closed not planned**.
- [#67329](https://github.com/anthropics/claude-code/issues/67329) "Sub-agent permission-denial user text is never propagated to the parent session": closed not planned (June 2026).
- [#89480](https://github.com/anthropics/claude-code/issues/89480) "Structured attribution of user permission denials to the parent agent": open.
- [#87153](https://github.com/anthropics/claude-code/issues/87153) "Denial messages don't say which permission rule… fired" and [#96026](https://github.com/anthropics/claude-code/issues/96026) "Report the deciding permission rule on every decision": open, 0 👍.
- [#95106](https://github.com/anthropics/claude-code/issues/95106) "Clarify whether Recently denied approval is one-time or persistent": open. This implies a "recently denied" surface exists but its persistence is unclear.
- [#84265](https://github.com/anthropics/claude-code/issues/84265) "Agent does not re-consult its own persistent memory at action time": open.
- [#98591](https://github.com/anthropics/claude-code/issues/98591) "After the user approves running a specific script… Claude edits the script and runs the edited version under the same approval": open. This is an approval-binding problem that Decidio's sealed, hash-bound receipt addresses.

**Human fallback and approval routing (Codex)**
- [#21975](https://github.com/openai/codex/issues/21975) "Auto-review approvals should support optional human fallback on deny/abort": 14 👍, open (May 2026).
- [#53430](https://github.com/openai/codex/issues/53430) (Oct 2026): with `approvals_reviewer=auto_review`, auto-review repeatedly rejected a migration and git push that the user had directly authorized, because the approval "appears only in history explicitly marked untrusted". The user was "stuck in a repeated approval loop". Body read via GitHub search.

**Team sharing of rules**
- Claude Code [#89877](https://github.com/anthropics/claude-code/issues/89877) "Team/group-scoped managed settings: per-team CLAUDE.md, permissions…": open, 0 👍.
- [#79759](https://github.com/anthropics/claude-code/issues/79759) "let a specific allow override a broad deny": 5 👍.
- Cursor CLI permissions (`.cursor/cli.json` allow/deny) "can be committed and shared with your team". The IDE allowlist is global only ([Cursor forum](https://forum.cursor.com/t/where-is-the-terminal-command-allowlist/153128), search summary).
- Cursor Auto-review run mode runs allowlist, then sandbox, then classifier, and Cursor calls the classifier "non-deterministic… best-effort convenience, not a security boundary" ([Cursor forum](https://forum.cursor.com/t/auto-review-run-mode/161922), search summary).

**First-party "approvals become policy"**
- Claude Code's bundled `/fewer-permission-prompts` (`/less-permission-prompts`, v2.1.111+):
  - It scans up to 50 recent sessions, tallies read-only Bash and MCP calls approved 3 or more times, and proposes up to 20 allow rules.
  - It writes them to the **project** `.claude/settings.json`, the committed file teammates share.
  - Sources: [Classmethod](https://dev.classmethod.jp/en/articles/claude-code-fewer-permission-prompts/), [MindStudio](https://www.mindstudio.ai/blog/claude-code-fewer-permission-prompt-allow-list), [docs gap issue](https://claudeissues.com/issue/49289-docs-commands-reference-missing-less-permission-prompts-bundled-skill) (search summary). It is also listed as a bundled skill in this session (observed 16 Oct 2026).
- Pushary sells per-tool policies ("auto-approve low-risk reads… destructive actions always escalate") plus an audit trail ([Pushary](https://pushary.com/approve-ai-tasks), search summary).

**Context or precedent products and theses**
- HumanLayer: denials "send feedback back to the model" within the run ([humanlayer.dev](https://humanlayer.dev), search summary).
- Permit.io: Access Request MCP (agent requests, human decides) and MCPermit gateway with "step-up approvals", described as an upcoming product in early access ([Permit docs](https://docs.permit.io/mcp-permissions/overview), [Permit blog](https://www.permit.io/blog/delegating-ai-permissions-to-human-users-with-permitios-access-request-mcp)).
- Agno: a "Decision Log Store" with reasoning, alternatives and outcome quality ([Agno docs](https://docs.agno.com/learning/stores/decision-log)).
- GitLab handbook design: a decision log that "agents can read as input context and write" ([GitLab handbook](https://handbook.gitlab.com/handbook/engineering/architecture/design-documents/spec_driven_development/decision_log/)).
- Foundation Capital "context graphs": decision traces (policy evaluated, exception route, **who approved**) as a new system of record ([Ashu Garg](https://ashugarg.substack.com/p/ais-trillion-dollar-opportunity-context)). Neo4j makes the same argument ([Neo4j](https://neo4j.com/blog/agentic-ai/context-graph-decision-traces/)).
- Progent paper: an LLM-generated policy where only expansions (about 6% of updates) need human approval ([arXiv 2504.11703](https://arxiv.org/pdf/2504.11703)).

### Inferences
- **For operator self-approvals, the "approvals teach the agent" opportunity is closing fast.**
  - Vendors are replacing the human with classifiers (Anthropic, Cursor, OpenAI, GitHub).
  - Vendors are converting repeated approvals into committed allow rules (Claude Code skill).
  - What remains for the operator is low-value.
- **The unserved slice is cross-person and cross-session denials with reasons:**
  - "Your client refused this on 3 Oct because the offer was off-brand."
  - "This is a duplicate of D·0412."
  - Decisions served to *any* agent: n8n, ChatGPT, Claude, Codex.
  - The demand signals are real but small (tens of votes at most), and Anthropic closed the top one "not planned". Neither data point shows people will pay.
- Codex #21975 and #53430 show a concrete hook. **Auto-reviewers need a human escalation path, and the human is often not the operator** (a lead, or the client owning the production system). That is a routed approval.
- "Approvals become context" is fashionable with investors (context graphs) but enterprise-framed. For SMBs it is more credible as a **feature of the approval product** (fewer repeat asks, dispute-proof history) than as a standalone purchase.

### Gaps
- GitHub reaction counts are from the search API. Issue bodies and maintainer comments for claude-code issues could not be read (repo not allow-listed), so I could not verify what "Recently denied" actually does.
- Cursor forum vote counts and HN threads were not retrieved.
- No primary Anthropic source was retrieved for the 97% and 89% figures.

---

## Q4. AI-agent agencies and builders: do clients demand sign-off, and do builders pay for a white-label approval or oversight layer?

### Takeaway
Builders **do pay for white-label wrappers**: voice-agent portals at $29 to $499 a month, and n8n client portals. Agency retainers are $300 to $1,500 a month for small businesses, which leaves room for a $20 to $100 line item.

But **approval is now a free native primitive** in n8n (tool-level human review over 8 channels including WhatsApp). Instantly has it too, and Pushary's Partner plan offers no-account end-customer approval.

Zapier and Make are weaker, which creates a real gap. Zapier reviewers need Zapier accounts and the Zap shared with them. Make's native HITL is closed beta and reportedly Enterprise-only. Relay.app, the tool HITL buyers favoured, **shut down on 14 Sept 2026**.

The dedicated approval layers are small:
- gotoHuman: $29/month for 1k reviews and $99/month for 5k reviews.
- Velatir: "from $6/month" (unverified) and 5 Product Hunt upvotes, though it reportedly raised €6.35M in 2026 and moved toward enterprise governance.

I found no evidence that agencies buy a separate white-label *approval* layer, and no primary evidence that clients contractually demand pre-action sign-off.

### Cited findings
**Native approvals in builder platforms**
- n8n: a "human review step attached to an AI Agent's tools". The channels are Slack, Discord, Telegram, Microsoft Teams, Gmail, WhatsApp Business Cloud, Google Chat and Outlook. On denial, "the action is canceled and the AI is told it was rejected". Sources: [n8n docs](https://docs.n8n.io/advanced-ai/human-in-the-loop-tools), [n8n docs](https://docs.n8n.io/build/integrate-ai/ai-examples/human-in-the-loop-for-tools/) (search summary). n8n plans: Starter €24, Pro €60, Business €667+ a month ([arahi](https://arahi.ai/blog/category/pricing), secondary).
- Zapier: reviewers need accounts (see Q1).
- Make:
  - The native "Human in the Loop" module is "currently in closed beta testing" ([Make help](https://make.com/en/help/app/human-in-the-loop)).
  - It is reportedly "reserved to Make's Enterprise plan" ([usecarly](https://www.usecarly.com/blog/human-in-the-loop-automation-tools/); a builder thread on [Skool](https://www.skool.com/masterai/human-in-the-loop-approval-for-makecom-automation-whats-the-best-free-approach)).
  - A third-party "Human Approval – HITL Platform" app (HITL, Inc., free tier) is available ([Make apps](https://apps.make.com/p1le-hitl)).
  - All search summary.
- Relay.app: shut down for free users on 15 Aug 2026 and for paying customers on 14 Sept 2026; new signups stopped 16 July. One comparison says it was "usually bought for" human-in-the-loop workflows. Sources: [Inbox Zero](https://www.getinboxzero.com/blog/post/relay-app-shutting-down), [Automation Atlas](https://automationatlas.io/answers/relay-app-shutting-down-what-to-do-2026/) (search summary).

**Dedicated approval layers**
- gotoHuman:
  - An approval layer for agents: MCP server, review forms, a central inbox, and routing to people or groups ([Glama MCP listing](https://glama.ai/mcp/servers/@gotohuman/gotohuman-mcp-server/blob/8728e2b8449c78c5245a91d71cccd59b411fc0b3/README.md), [FlowHunt](https://www.flowhunt.io/integrations/gotohuman/)).
  - Pricing: about **$29/month with unlimited users, 1,000 reviews, 30-day retention and Slack**, and **$99/month with 5,000 reviews, MS Teams, custom retention and timeouts, and SSO** ([creati.ai](https://creati.ai/de/ai-tools/gotohuman/)).
  - A free tier and MIT licence per [Composio directory](https://directory.composio.dev/listings/gotohuman) (unverified).
  - All search summary. No traction numbers were found.
- Velatir:
  - An SDK that routes AI decisions to Slack, Teams, email or SMS. "From $6/month" per [aiagentslist](https://aiagentslist.com/compare/velatir-vs-assista-ai) (unverified).
  - Product Hunt launch: 5 upvotes, #106 on the day ([hunted.space](https://hunted.space/product/velatir/launches/velatir-2)).
  - An Odense company with 22 employees and 4,000 Chrome-extension users; €6.35M across a Feb 2026 pre-seed and an Aug 2026 seed; now selling browser monitoring, policy agents and audit logs ([yespress profile](https://yespress.io/velatir.md), not primary).
  - All search summary.
- Pushary Partner: end-customer approval with no account, starting at $99/month (Q1).
- HumanLayer: credit-based pricing beyond a free tier, with Teams and SMS on custom plans ([everydev](https://www.everydev.ai/developers/humanlayer), unverified).

**What agencies charge and resell**
- AI-automation retainers: about $300 to $1,500/month for small-business monitoring, $1,500 to $5,000 mid-market, and $5,000 to $20,000 white-glove ([Vantaige 2026 rate card](https://vantaige.io/blog/2026-ai-automation-rate-card-operators-charge)).
- Builds run $1,500 to $20,000+, and SMB packages $1,800 to $4,500/month ([Codewave](https://codewave.com/feeds/blog/ai-automation-agency-pricing), [alexcloudstar](https://www.alexcloudstar.com/blog/ai-powered-agency-developer-playbook-2026)).
- LLM pass-through is billed separately.
- All search summary.
- Voice white-label:
  - VoiceAIWrapper: Starter $29 (5 clients), Growth $79 (15 clients plus Stripe), Scale $249, Pro $499 a month; agencies rebill "at a price of your choosing" ([ColdIQ](https://coldiq.com/tools/voiceaiwrapper), [VoiceAIWrapper help](https://voiceaiwrapper.featurebase.app/en/help/articles/2115055-how-voiceaiwrapper-pricing-works)). A competitor disputes the client limits ([Trillet](https://trillet.ai/blogs/ai-receptionist-white-label-pricing)).
  - Retell white-label is enterprise-only, so agencies use one workspace per client plus a third-party wrapper ([Ainora](https://ainora.lt/md/white-label-ai-voice-alternatives/retell.md)).
  - All search summary.
- n8n client portals: FlowEngine (open-source white-label portal with Stripe billing) and Getflowetic (branded dashboards across n8n, Make, Vapi and Retell). No approval workflow or prices found. Sources: [FlowEngine](https://linkedin.com/company/flowenginecloud), [G2 Getflowetic](https://www.g2.com/sellers/getflowetic) (search summary).

**Client demand for sign-off**
- Vendor-blog only: two-gate review; "when a client claims they never approved something, there's no audit trail" (Q2).
- n8n guides advise showing the client "recipient, subject, and body" in the approval message, with 24 to 48-hour timeouts and escalation to a second approver ([kirim.email](https://en.kirim.email/blog/email-approval-workflow-n8n-human-in-the-loop/), search summary).
- A Sept 2026 WhatsApp HITL guide describes notify, wait, then act or abort over the owner's own number. A third-party approver needs the Business API with template review "hours to days" ([Blueticks](https://blueticks.co/blog/human-in-the-loop-whatsapp-approvals), search summary). That is a real friction point for no-account WhatsApp approvals to *other* people.

### Inferences
- **Agencies pay for things that make them look professional to clients and let them rebill:** branded portals, Stripe rebilling and per-client workspaces. An approval layer sells if it is **client-facing, branded, and produces a receipt the agency can show in a dispute**, not as a generic HITL node, which n8n gives away.
- **Displacement windows:**
  - Relay.app's shutdown leaves HITL-heavy teams moving to n8n or Zapier.
  - Zapier's account requirement for reviewers excludes external clients.
  - Make's HITL is Enterprise or beta.
  - Together these make a cross-platform, no-account, client-facing approver real but narrow.
- **Price anchor:** gotoHuman's $29 to $99 and Pushary Partner's $99+ bracket what builders will pay. VoiceAIWrapper's $29 to $499 by client count shows the "price per client" model agencies accept.
- **Traction for dedicated approval layers is weak.** Velatir has 5 upvotes and pivoted to enterprise funding; gotoHuman shows no public traction. That is evidence the standalone category is small even where approvals matter.

### Gaps
- No primary evidence (contracts, surveys, forum threads) that SMB clients *require* sign-off on agent actions.
- Relevance AI, Lindy, Voiceflow, Retell and Vapi native approval features were not confirmed (no docs surfaced).
- No traction figures for gotoHuman, HITL Platform, FlowEngine or Getflowetic.

---

## Q5. Synthesis: where would each routed approval most likely recruit a new user?

### Core judgement
**Routed approvals do not self-generate demand in the Calendly or DocuSign sense.** Four reasons:
1. The approver's job (judge someone else's request) differs from the sender's job, so the classic recipient-to-sender loop is weak.
2. In every comparable paid category (proofing, AP approvals, n8n, Pushary Partner) the approver is **free and is not the buyer**.
3. Vertical platforms embed approvals for free (GitHub, Instantly, n8n, QuickBooks).
4. Operator-side approvals are being automated away (auto mode, Cursor and Codex auto-review, Copilot approvals).

Realistic recipient-to-user K is likely well under 0.2. That is an inference from the B2B 0.2 to 0.7 range for *same-job* tools.

What routed approvals **can** do:
- **(a) Distribution through a hub.** One agency, builder or bookkeeper exposes Decidio to 5 to 50 approvers. This is BILL's accountant channel and Filestage's free reviewers.
- **(b) Intra-org expansion.** A manager who approves becomes the admin who mandates it.
- **(c) A creative reverse loop.** The approver becomes a **policy and ledger owner** ("my rules apply to anyone who asks me; here is everything I have approved or refused, across vendors, with receipts"). This is the PandaDoc recipient-inbox pattern applied to approvals, and it is untested.

### Segment ranking
Scores run High, Med or Low. "Recipient→user" means the approver starts sending requests or buys.

| # | Segment (requester → approver) | Recipient→user potential | Willingness to pay | Fit with Decidio assets | Demand evidence | Competitive gap |
|---|---|---|---|---|---|---|
| 1 | **AI-automation agencies and freelance builders (n8n, Make, Zapier, voice) → their SMB client**, for consequential agent actions (send to customers, post live, refund, book, spend) | Low per approver; **High hub multiplier** (each agency brings 5 to 50 client approvers) | Med: $29 to $99/month anchors (gotoHuman, Pushary Partner); fits inside $300 to $1,500 retainers | **High**: no-account WhatsApp/SMS/email approver, sealed receipt for "never approved" disputes, refusal memory, MCP + SDK across platforms | Moderate (vendor blogs, retainer and white-label spend; no primary client mandate) | Narrow to moderate (n8n native is free; Zapier needs reviewer accounts; Make Enterprise or beta; Relay.app gone; gotoHuman, Velatir, Pushary Partner present) |
| 2 | **Approver-as-hub: an SMB owner or marketing lead receiving agent-driven requests from several vendors**, who claims a free "approver ledger" and sets standing rules | **Med to High if the ledger and policy are compelling; unproven** | Low to Med (would pay to set rules once across vendors; no evidence yet) | High (receipts, refusal memory, policy engine are exactly the ledger) | Weak (analogs only: PandaDoc recipient inbox, DocuSign free signer) | Wide (nobody offers a cross-vendor approver ledger) |
| 3 | **Small software teams: lead or founder ← junior dev's or contractor's coding agent** for production-risk actions (deploy, prod DB, secrets, spend) | Med inside the org (approver becomes the admin buyer); none across orgs | Med (teams pay for dev tools; GitHub Enterprise gates cost $21/user) | High (Claude Code hook, Codex, receipts as SOC 2 evidence) | Weak to moderate (Codex #21975, 14 👍; denial-propagation issues; no "route to my lead" votes) | Narrow (GitHub PR review native; Copilot can now approve; auto mode) |
| 4 | **Bookkeepers and accountants: agent proposes payments → business owner approves** (or the reverse) | Low (owners do not send); hub-shaped like BILL's accountant channel | **High, proven** (ApprovalMax $59+/month priced on approvers; BILL) | Low to Med (needs QBO/Xero, payment rails, finance-grade controls) | Moderate (bookkeeping agents draft for approval; payroll agent drafts only) | Closed to narrow (ApprovalMax, BILL, QBO native) |
| 5 | **Agencies → clients for AI-generated content and creative (posts, ads, emails before send)** | Low (reviewers stay free; no evidence they buy) | Med to High (Filestage $199 to $399, Planable $33 to $59/workspace, Ziflow, GoProof) | Low to Med (no previews, annotations or versions; strong only on receipt and audit) | **Strong** (established paid category, 5 to 7k Planable teams, about 400 Filestage customers est.) | Closed for proofing; slight opening on "AI-generated disclosure + approval receipt" |
| 6 | **SaaS products whose agents act for their own end customers → the end customer approves** (refunds, bookings, deletions) | Very low (end customers never buy) | Med (Pushary Partner $99+) | Med to High (no-account link, receipts) | Weak to moderate (Pushary built a plan for it) | Narrow (Pushary Partner targets it exactly) |
| 7 | Solo operator approving their own agent (reference) | None | Low ($9.99 to $19.99) | High | Strong but saturated | Closed (Remote Control, Codex remote, Pushary) |
| 8 | Parent or guardian ← child's agent purchases | Low | Unknown | Low | None found | n/a (Apple Ask to Buy template; no agent product) |

### Per-segment one-liners (demand evidence / competitive gap)
1. **Agency or builder → client actions.** Demand: moderate, but vendor-blog only. Gap: narrow-to-moderate. The opening is cross-platform, no-account, client-branded, dispute-proof approvals where Zapier, Make and Relay leave holes.
2. **Approver-as-hub ledger.** Demand: weak and unproven. Gap: wide. This is the only segment where the approver has their own reason to hold an account. It is the experiment to run, not a market to bank on.
3. **Lead ← junior or contractor coding agent.** Demand: weak-to-moderate (low-vote issues). Gap: narrow, because GitHub, auto mode and Copilot approvals are absorbing it.
4. **Accountant ↔ owner payments.** Demand: moderate with proven willingness to pay. Gap: closed-to-narrow; incumbents own the rails.
5. **Content and creative client approval.** Demand: strong. Gap: closed for proofing; the reviewer never buys.
6. **SaaS end-customer approval.** Demand: weak-to-moderate. Gap: narrow; Pushary Partner is already there.
7. **Self-approval.** Demand: strong. Gap: closed.
8. **Parent or guardian.** Demand: none found. Gap: n/a.

### Inferences and recommendation (for the founder)
- **Sell to the hub, convert the approver into a ledger and policy owner.**
  - The buyer is the agency or builder (segment 1), priced per client on the VoiceAIWrapper model ($29 to $99 base, scaling with clients).
  - The approver flow should copy PandaDoc and DocuSign: no account to decide, then after the 2nd or 3rd decision offer "claim your decision ledger". Every approval and refusal across all senders, with reasons and receipts, plus "set a standing rule" (for example, auto-approve posts that pass brand rules; always route refunds over $100).
  - The context pack then becomes **the approver's rules and precedents served to every vendor's agent**. That is the one place "approvals feed agent context" has a buyer with a distinct reason to pay.
- **Kill criteria (falsifiable).** Run 3 to 5 agencies with 30 or more client approvers for 60 days. Routed approvals are not a demand engine if **fewer than 10% of approvers claim the ledger** or **fewer than 2% set a standing rule or start their own requests**. Sell Decidio as a builder tool (segment 1) on its own merits, or as compliance evidence (prior research), and stop expecting virality.
- **Where not to spend:**
  - Content proofing (segment 5) and payments (segment 4): incumbents, and the reviewer never buys.
  - Operator self-approval (segment 7): closed.
  - Generic "approvals teach the agent" for coding operators: first-party features (auto mode, `/fewer-permission-prompts`) cover most of it.

### Gaps (overall)
- No primary figure anywhere for the share of signups from recipients or approvers. Every viral number above is aggregate, estimated or unverified.
- No Reddit or Upwork practitioner evidence (blocked or not found).
- Pushary Partner pricing, gotoHuman traction and Velatir customers are unconfirmed.
- The approver-as-hub loop (segment 2) has no direct evidence. It is a reasoned extrapolation from PandaDoc's recipient inbox, DocuSign's free signer and BILL's network, and should be treated as a hypothesis to test.

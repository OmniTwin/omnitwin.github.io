# Deployment and CI approval gates for small software teams (2–30 people) — demand and supply, as of 16 October 2026

Research method note: GitHub.com pages, raw GitHub docs source and GitHub Marketplace listings were fetched directly. The following domains could not be resolved by the fetch tool in this session and are therefore covered only by search-engine snippets or not at all: docs.github.com (worked around via raw.githubusercontent.com/github/docs), docs.gitlab.com (worked around via gitlab.com raw), sleuth.io, kosli.com, spacelift.io, hoop.dev, harness.io, octopus.com, docs.railway.com, station.railway.com, vercel.com, github.blog, dev.to, trustradius.com, costbench.com, docs.devin.ai, insights.logicstar.ai. The web-search budget ran out before Stack Overflow, Reddit and HN could be probed a second time. Every unfetched item is listed under Gaps.

## (a) Which platforms gate deploys behind a named human approver, on which plans

### Takeaway
On GitHub, every human-approval primitive for deployments (required reviewers, wait timers, custom deployment protection rules, admin bypass) is public-repo-only on Free, Pro and Team; private/internal repos need Enterprise at $21/user/month versus $4 on Team. GitLab gates deployment approvals behind Premium/Ultimate. Vercel, Railway and Render offer manual "promote"/"manual deploy" switches but no named-approver gate (Railway has a narrow, staff-toggled exception); Fly.io has nothing found.

### Cited Findings
GitHub
- Required reviewers: "If you are on a GitHub Free, GitHub Pro, or GitHub Team plan, required reviewers are only available for public repositories." Up to six users or teams can be listed as reviewers. — [GitHub docs source: deployments-and-environments.md](https://raw.githubusercontent.com/github/docs/main/content/actions/reference/workflows-and-actions/deployments-and-environments.md)
- Wait timers: same public-only restriction on Free/Pro/Team; range 1–43,200 minutes (30 days); "Wait time will not count towards your billable time." — [same source](https://raw.githubusercontent.com/github/docs/main/content/actions/reference/workflows-and-actions/deployments-and-environments.md)
- Custom deployment protection rules (GitHub-App-based, still carrying a beta note): "only available for public repositories for users on GitHub Free, GitHub Pro, and GitHub Team plans." Administrator bypass of protection rules: same restriction. — [same source](https://raw.githubusercontent.com/github/docs/main/content/actions/reference/workflows-and-actions/deployments-and-environments.md)
- Limit: "a maximum of 6 deployment protection rules can be enabled on any environment at the same time." — [GitHub docs reusable](https://raw.githubusercontent.com/github/docs/main/data/reusables/actions/custom-deployment-protection-rules-limits.md)
- Gated-features note: environments, environment secrets and deployment protection rules are available in public repos on all current plans; private/internal repos need Pro, Team or Enterprise for environments; on Free/Pro/Team, protection rules such as wait timers or required reviewers work only in public repos. — [GitHub docs reusable gated-features/environments](https://raw.githubusercontent.com/github/docs/main/data/reusables/gated-features/environments.md)
- Environment creation in private repos is available to Team orgs and Pro users; Free users can only configure environments for public repos; converting public→private causes protection rules to be ignored. — [GitHub docs source: manage-environments.md](https://raw.githubusercontent.com/github/docs/main/content/actions/how-tos/deploy/configure-and-manage-deployments/manage-environments.md)
- Plan feature lists: GitHub Team lists "Required pull request reviewers", "Protected branches", "3,000 GitHub Actions minutes per month"; GitHub Enterprise lists "Deployment protection rules with GitHub Actions for private or internal repositories" and "50,000 GitHub Actions minutes per month". — [GitHub docs source: githubs-plans.md](https://raw.githubusercontent.com/github/docs/main/content/get-started/learning-about-github/githubs-plans.md)
- Prices as displayed on the pricing page: Free $0; Team "$4 USD per user/month" (page shows "for the first 12 months*" wording); Enterprise "$21 USD per user/month" (same intro wording). Included minutes: Free 2,000, Team 3,000, Enterprise 50,000. Enterprise card lists "Environment protection rules"; Team card lists "Environment deployment branches and secrets" and "Required reviewers" (which on Team refers to PR reviewers, not deployment reviewers). — [GitHub pricing](https://github.com/pricing)
- A Team-plan user in the manual-approval issue tracker said Enterprise "costs about five times more" than their Teams plan and wanted a Dev → Staging → Production promotion flow with control over who can promote. — [trstringer/manual-approval issue #95](https://github.com/trstringer/manual-approval/issues/95)
- Starting March 1, 2026, self-hosted runners in private/enterprise repos incur $0.002 per minute (community discussion snippet, not verified against billing docs). — [GitHub community discussion #184661 via search](https://github.com/orgs/community/discussions/184661)
- Copilot-support reply (accepted answer, Mar 27 2024): on Team "some deployment protection rules, such as a wait timer or required reviewers, are only available for public repositories"; private repos need Enterprise. — [GitHub community discussion #114537](https://github.com/orgs/community/discussions/114537)

GitLab
- Deployment approvals page header: "Tier: Premium, Ultimate"; "Offering: GitLab.com, GitLab Self-Managed, GitLab Dedicated". Multiple approval rules, group approvers, triggering user cannot self-approve by default, approval does not start the job (someone must still run it), all jobs deploying to the environment wait. — [GitLab docs source: deployment_approvals.md](https://gitlab.com/gitlab-org/gitlab/-/raw/master/doc/ci/environments/deployment_approvals.md)

Vercel
- Official docs describe manual "Promote to Production" from the Deployments tab, `vercel promote <url>`, and "staged production deployments" (disable Auto-assign Custom Production Domains, then promote). Custom environments such as staging are Pro and Enterprise. No built-in named-approver gate was found in official docs. — [Vercel docs: promoting a deployment](https://vercel.com/docs/deployments/promoting-a-deployment); [Vercel KB: staging environment](https://vercel.com/kb/guide/set-up-a-staging-environment-on-vercel)

Railway (forum snippets only; docs and forum pages could not be fetched)
- Support thread: "The deployment approval prompt only appears when a commit is pushed by a GitHub user whose account is not linked to Railway." — [Railway Station: setting prod deployment approvals](https://station.railway.com/questions/setting-prod-deployment-approvals-902302b3)
- 2025 thread: staff enabled a workspace-wide (not per-environment) setting requiring manual approval for every deployment; staff: "We do not have docs on this because it is not a user accessable option." — [Railway Station: feature request manual deployment approval](https://station.railway.com/questions/feature-request-manual-deployment-appro-51765d0e)
- Solo developer asking for per-deploy approval was told "There is not, sorry"; suggested workaround "write the CI/CD pipeline yourself with a GitHub action and the railway CLI". — [Railway Station: every deployment on production must approve](https://station.railway.com/questions/if-i-want-every-deployment-on-production-f6da10c8)
- A separate thread reports the approval UI itself broke and blocked a production deploy. — [Railway Station: deployment approval UI broken](https://station.railway.com/questions/deployment-approval-ui-broken-blocking-45f1d961)

Render
- Auto-Deploy options are "On Commit" (default), "After CI Checks Pass", and "Off" ("Choose this option if you only want to trigger deploys manually"). No approval gate; manual deploy is the gate. — [Render docs: deploys](https://render.com/docs/deploys)
- A community thread reports `autoDeploy: false` in a blueprint still deploying on push; staff said it is a known issue when blueprint changes alter a service setting. — [Render community thread](https://community.render.com/t/auto-deploy-off-does-not-work/10743)

Fly.io
- No Fly.io documentation or forum thread on deploy approval surfaced in two searches; the only pattern found is gating `flyctl deploy` inside a GitHub Actions environment. — [precisionaiacademy CI/CD tutorial (secondary)](https://precisionaiacademy.com/learn-cicd/day-4)

### Inferences
- The Team→Enterprise jump for a 10-person team is $40 → $210 per month ($170/month, 5.25×) using the displayed prices, and the only Enterprise feature most such teams want is the deployment gate. That is the price umbrella a $9–29/org product sits under.
- Because custom deployment protection rules (the GitHub App hook) are also Enterprise-only on private repos, a third-party gate cannot use GitHub's native "pending deployment" mechanism on Team private repos; it must gate at a different point (see "what would still need building").
- The PaaS vendors (Vercel/Railway/Render/Fly) have "manual switch" semantics, not "named approver + record" semantics, so a gate product is complementary rather than competitive with them; the hook point is the CI job that calls their CLI.

### Gaps
- GitLab Premium/Ultimate per-user prices were not fetched (pricing page unreachable); do not quote from memory.
- Whether the GitHub "$4 / $21 for the first 12 months" wording denotes an intro price with a later increase could not be confirmed from the fetched page.
- Railway's official docs page on deployment approvals could not be fetched; the forum snippets above are unverified beyond the search excerpt.
- Fly.io: no primary source either way.

## (b) Workarounds and their adoption

### Takeaway
The de-facto workaround is trstringer/manual-approval (677 stars, 158 forks), which pauses a running job and polls an issue; it burns runner compute and a concurrency slot while waiting, hard-fails at 6 hours, and caps team-based approvals at 60 minutes because GitHub App tokens expire. Alternatives are a workflow_dispatch "trigger instead of gate", Telegram/issue-based clones on the Marketplace, and Vercel's Chat SDK Slack deploy-bot template (1 star).

### Cited Findings
trstringer/manual-approval
- 677 stars, 158 forks, 22 open issues, 10 open PRs, 102 commits, 26 contributors; latest version v1.13.1; Marketplace listing says "not certified by GitHub". — [repo](https://github.com/trstringer/manual-approval); [Marketplace listing](https://github.com/marketplace/actions/manual-workflow-approval)
- Mechanism: creates an issue assigned to approvers; approval words "approve", "approved", "lgtm", "yes"; denial "deny", "denied", "no"; `minimum-approvals`, `fail-on-denial`, `exclude-workflow-initiator-as-approver`, `polling-interval-seconds` (default 10); requires `issues: write`; Linux runners only. — [repo README](https://github.com/trstringer/manual-approval)
- Documented limits (verbatim excerpts): "While the workflow is paused, it will still continue to consume a concurrent job allocation"; "A paused job is still running compute/instance/virtual machine and will continue to incur costs."; "A job (including a paused job) will be failed after 6 hours, and a workflow will be failed after 35 days"; "Adding a GH team instead of a user won't circumvent the issue of having a maximum of 10 assignees". — [Marketplace listing](https://github.com/marketplace/actions/manual-workflow-approval)
- App-token cap: org-team approvers need a GitHub App token with org-members read + issues write; such tokens expire after 1 hour, so the approval window cannot exceed 60 minutes or the job fails with bad credentials. Approvers who cannot be assigned cause an HTTP 402/422 failure. — [repo README](https://github.com/trstringer/manual-approval)
- Issue #95 "Pausing is very cost expensive" (opened Apr 26 2023; closed Jun 13 2025 as "already documented"): requester wanted a prominent warning that the action pauses a billed runner; a commenter explained that billing starts when the job's runner is created and that native "reviewing deployments" does not start a job so does not bill. No maintainer reply in thread. — [issue #95](https://github.com/trstringer/manual-approval/issues/95)
- Other high-comment issues: #7 and #71 "422 Validation Failed" (19 and 9 comments), #47 "Timeout setting does not work" (11), #75 "How to set url for enterprise github?" (open, 10), #102 "Org Team Approver Documentation Issue" (8), #89 "Issue body maximum is 65536 characters" (8). — [issues sorted by comments](https://github.com/trstringer/manual-approval/issues?q=is%3Aissue+sort%3Acomments-desc)
- Blog origin and README state the native option "requires the use of environments and if you want to use this for private repositories then you need GitHub Enterprise"; the action "is freely available to use on private repositories". — [Marketplace listing v1.5.1](https://github.com/marketplace/actions/manual-workflow-approval?version=v1.5.1)

sede-open/manual-approval
- Fork of trstringer's action: 1 star, 1 fork, 0 watchers, 54 commits, 3 open PRs; README is essentially upstream's (same limits: 6-hour job, 72-hour workflow timeout in its older text, 1-hour app token, HTTP 402). No documented functional differences. — [repo](https://github.com/sede-open/manual-approval)

Other Marketplace actions
- Searching the Marketplace for "manual approval" returns 15 actions, including three Telegram-based variants, several issue-based clones ("Issues - Manual Workflow Approval", "Workflow: Manual Approval", "Create/Update Manual Approval Issue"), "RADICL Manual Workflow Approval" and "manual-approval-action". No star or install counts are displayed on the search page. — [Marketplace search](https://github.com/marketplace?query=manual+approval&type=actions)

workflow_dispatch gate
- A DEV Community post (Japanese) reports an HTTP 422 when trying to set approval reviewers on a private repo on Pro and recommends "a manual deployment using workflow_dispatch" as "a reliable and free alternative for private repositories", "often enough for small teams". — [dev.to post (snippet only, page unreachable)](https://dev.to/orca_forge/github-pronopuraibetoripozitoridedepuroicheng-ren-geto-wodouzuo-ruka-3fij)

Slack bots
- Vercel's guide "Run and track deploys from Slack" describes a Chat SDK Deploy Bot where production deploys "wait on an approval step before they run"; Chat SDK handles Slack cards/buttons, Vercel Workflow handles pause/resume; requires a Slack app, a GitHub fine-grained token and Redis (Upstash suggested). Guide appears with last-updated dates in April–June 2026. — [Vercel KB (snippet)](https://vercel.com/kb/guide/run-and-track-deploys-from-slack)
- Template repo vercel-labs/chat-sdk-deploy-bot: 1 star, 1 fork, 2 commits. Production approvers are a comma-separated list of Slack user IDs (`DEPLOY_PROD_APPROVERS`, "If empty or unset, nobody can approve"); needs `REDIS_URL`, a fine-grained PAT with Actions read/write, `GITHUB_WORKFLOW_ID`, Slack bot token and signing secret. README describes no persistent audit log, only a final Slack summary card. — [vercel-labs/chat-sdk-deploy-bot](https://github.com/vercel-labs/chat-sdk-deploy-bot)

GitHub Apps in the same space (all free, tiny install counts)
- Greenlight Actions (publisher praxiom-systems): "pauses the expensive CI until you approve"; add `environment: greenlight` to a job, check shows Pending, developer clicks "Run CI" in the Checks tab; single $0 plan; "free on all GitHub plans"; 3 installs; no Slack. — [Marketplace: Greenlight Actions](https://github.com/marketplace/greenlight-actions)
- ApprovalOps (publisher stormwrought): "Approval evidence for pull request reviewers"; free tier of 30 PR analyses/month; "clean path to paid plans later" but no prices; 4 installs. — [Marketplace: ApprovalOps](https://github.com/marketplace/approvalops)
- Marketplace search for "approval" (apps) returns 16 apps; none is a Slack deployment-approval tool; nearest are Greenlight Actions, ApprovalOps, Branch Guard (YAML rules to require approvals and gate merges) and Jive by SkyMoon (readiness gates). — [Marketplace search](https://github.com/marketplace?query=approval&type=apps)

### Inferences
- Cost arithmetic from sourced numbers: a gate that waits the maximum 6 hours burns 360 hosted-runner minutes; GitHub Team includes 3,000 minutes/month, so about eight max-length waits per month would exhaust the included quota before any real CI runs. Even on self-hosted runners the $0.002/min charge from March 2026 means waiting is no longer free.
- Greenlight Actions' claim to work via `environment: greenlight` on "all GitHub plans" conflicts with GitHub's docs that custom deployment protection rules are public-only on Free/Pro/Team; either it relies on a different mechanism (e.g. a required status check) or the claim is wrong for private repos. Treat as unverified.
- The 677-star action with 158 forks and 15 Marketplace clones indicates a real, recurring need that nobody has productised; the clones' choice of Telegram and issue comments (not Slack/phone) shows the channel is an open design space.

### Gaps
- GitHub Marketplace does not display install counts for actions, so adoption of trstringer/manual-approval can only be inferred from stars/forks; a code search for workflows referencing it was blocked (the GitHub API in this session is restricted to configured repos).
- Reaction counts on issue #95 were not visible.

## (c) Paid products and prices

### Takeaway
Every paid product with a manual-approval step is priced for mid-market or enterprise (Sleuth median $36.3k/year per Vendr; Spacelift entry $20k/year per a 2026 listing; Hoop.dev custom/$10k per month on AWS Marketplace; Octopus per-target; Harness enterprise pricing unpublished). Nothing on the GitHub Marketplace sells a Slack/phone deployment approval gate; the two adjacent apps are free with 3–4 installs each.

### Cited Findings
- Sleuth: TrustRadius lists Standard $24/month and Enterprise $45/month with a free version; Toolradar estimates $35/user/month Standard, $45/user/month Enterprise; Vendr reports annual spend of $12,800–$41,580, median $36,300 (12 purchases). Sources conflict and none is the vendor page. — [TrustRadius (snippet)](https://www.trustradius.com/products/sleuth/pricing); [Toolradar (snippet)](https://toolradar.com/tools/sleuth/calculator); [Vendr (snippet)](https://vendr.com/marketplace/sleuth-io)
- Hoop.dev: CostBench says "custom pricing — contact their sales team" and no free plan, with an AWS Marketplace listing at "$10,000.00/mo + $6.84/hr"; Airframe says a free starter tier exists with Enterprise at "Contact Sales". Conflict unresolved. — [CostBench (snippet)](https://costbench.com/software/api-management/hoop-dev/); [Airframe (snippet)](https://www.airframe.ai/product/hoop-dev/compare)
- Spacelift: aggregators disagree; one lists Free up to 2 users; one says Starter at $399/month for 10 users; a 2026 listing says the ~$250/month Cloud and $399/month Starter tiers are no longer offered and the entry paid tier is Starter+ at $20,000/year. — [Scalr comparison (snippet)](https://learning.scalr.com/terraform-cloud-vs-spacelift-a-matter-of-fact-pricing-breakdown); [agentdeals 2026 listing (snippet)](https://agentdeals.dev/vendor/spacelift)
- Harness: Free plan "suited to small teams or startups" with 2,000 cloud credits/month; Enterprise pricing not published; approval stages use user groups with a default 1-day timeout. — [Harness docs approvals (snippet)](https://developer.harness.io/docs/platform/approvals/adding-harness-approval-stages)
- Octopus Deploy: third-party page says Octopus Cloud starts at about $10 per deployment target per month with a free Starter tier up to 10 targets; G2 reviewers "repeatedly call it expensive for small teams". — [Capterra (snippet)](https://www.capterra.com/p/204945/Octopus/); [G2 reviews (snippet)](https://g2.com/products/octopus-deploy/reviews?page=3)
- Kosli: TrustRadius says "Kosli does not currently have any pricing plans listed" and a free version is available. No per-user price found. — [TrustRadius Kosli (snippet)](https://www.trustradius.com/products/kosli/pricing)
- GitHub Marketplace: Greenlight Actions $0 (3 installs); ApprovalOps $0 tier (4 installs); Slack+GitHub and Actioner for Slack are listed as free on a third-party aggregator. — [Greenlight Actions](https://github.com/marketplace/greenlight-actions); [ApprovalOps](https://github.com/marketplace/approvalops); [Axolo aggregator (snippet)](https://axolo.co/integrations/Actioner-for-Slack)

### Inferences
- There is a vacant price band between "free action that burns minutes" and "$10k+/year platform". A $9–29/org product would be the only priced option in that band that I could find.
- The only purchase evidence (Vendr's 12 Sleuth purchases at a $36.3k median) is enterprise buying; it proves that deployment-governance tooling is paid for, not that small teams pay for it.

### Gaps
- No vendor pricing page (Sleuth, Hoop.dev, Kosli, Spacelift, Harness, Octopus) could be fetched; all figures above are aggregator snippets and may be stale or wrong.
- No traction numbers (customers, ARR) for any of these vendors were found.
- Whether Sleuth is still independently operating in 2026 could not be checked (search budget exhausted).

## (d) Direct demand evidence (community discussions, Reddit/HN, Stack Overflow)

### Takeaway
The complaint recurs on the GitHub Community forum at least yearly (Mar 2024, Aug 2025, Jul 2026) but each thread has only 0–2 upvotes; the strongest adjacent signals are a 325-vote thread from Enterprise users of the native feature and a 102-vote request to make branch protection free on private repos. No Stack Overflow, Reddit or HN thread on the specific question was surfaced.

### Cited Findings
- #202852 "Environment 'Required reviewers' option not appearing despite GitHub Team plan" — posted Jul 24 2026, 0 upvotes, 3 comments; replies (1 vote each) confirm Enterprise is required for private repos and offer three options: public repo, upgrade, or "move the approval step outside the native rule". — [discussion #202852](https://github.com/orgs/community/discussions/202852)
- #170307 "Required reviewers for environments option not avaiable for my org which is a team account" — Aug 19 2025, 1 reaction on question, 5 reactions on the accepted answer ("only available under the Enterprise plan"); 2 replies, one suggesting "manual approval workflows in Actions as alternatives". — [discussion #170307](https://github.com/orgs/community/discussions/170307)
- #114537 "Can't access deployment protection rules for environments even if my org is on GitHub Team plan" — Mar 27 2024, 2 upvotes, 1 accepted answer (Copilot support). — [discussion #114537](https://github.com/orgs/community/discussions/114537)
- #14564 "Provide a way to disable emails for deployment reviews per repository/team" — Apr 8 2022, 325 votes, 105 thumbs-up, 72 comments + 11 replies, latest comment Oct 15 2026; no GitHub staff reply visible. Shows the native required-reviewers feature is in heavy use among Enterprise orgs where "everyone can deploy". — [discussion #14564](https://github.com/orgs/community/discussions/14564)
- #174419 "Please Make Basic Branch Protection Free for Private Repositories" — Sep 23 2025, 102 upvotes, 15 comments (adjacent price-gating complaint). — [discussion #174419](https://github.com/orgs/community/discussions/174419)
- #210403 "Actions stuck waiting for nonexistent deployment approval; cancel/force-cancel 409" — Oct 15 2026, 1 upvote. — [discussion #210403](https://github.com/orgs/community/discussions/210403)
- #13836 "[Feature Request] Allow github actions to bypass branch protection rules in certain specific circumstances" — Mar 30 2022, 1,868 upvotes, 192 comments (adjacent: pipeline-vs-protection friction). — [discussion #13836](https://github.com/orgs/community/discussions/13836)
- #184363 (Jan 15 2026, 2 upvotes): Team-plan org hit "rulesets won't be enforced on this private repository until you upgrade" and CI failed; closed unanswered. — [discussion #184363](https://github.com/orgs/community/discussions/184363)
- manual-approval issue #95 commenter (Jul 2023): Team-plan user wants controlled Dev→Staging→Prod promotion; Enterprise "about 5 times" the cost. — [issue #95](https://github.com/trstringer/manual-approval/issues/95)
- Two searches with `site:stackoverflow.com` and one Reddit-targeted search returned no Stack Overflow or Reddit pages on this topic; the HN-targeted search returned only GitHub docs and discussion #202852. — searches performed 16 Oct 2026 (no citable page)

### Inferences
- Demand is persistent but diffuse: the same question is asked and answered the same way every year, with answers routinely pointing to third-party actions. Low vote counts suggest affected teams work around rather than campaign.
- The 325-vote notification thread implies that once teams have the native gate they use it broadly (every reviewer emailed on every deploy), i.e. the feature is sticky where available.

### Gaps
- No Stack Overflow question with view counts was found (search tool returned none; could not browse SO directly).
- No Reddit or HN thread found; search budget exhausted before retry.
- No count of GitHub Team-plan organizations or of private repos using environments is public; "how many small teams are affected" cannot be sized from primary data.

## (e) Do AI coding agents create a new need for a human gate at merge or deploy?

### Takeaway
GitHub has institutionalised the human gate for Copilot coding agent (draft PRs, assigner cannot self-approve, "Approve and run workflows" before any Actions run), Anthropic's claude-code-action defaults to the human creating the PR "ensuring human oversight", and Devin's enterprise docs defer to branch protection and human merge approval; a March 2026 GitHub setting lets admins skip the Actions-approval step, which moves the burden of a gate from the platform to teams.

### Cited Findings
- Copilot coding agent: PRs start as drafts and "require human approval before merge"; Actions workflows triggered by the agent "do not run automatically" until a reviewer clicks "Approve and run workflows"; "If your repository requires pull request approvals, your approval of a Copilot pull request won't count toward the required number" when you assigned the task; reviewers are told to check `.github/workflows/` changes because workflows can access secrets. — [GitHub docs: review Copilot PRs (snippet)](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/cloud-agent/review-copilot-prs)
- Mar 13 2026 changelog: administrators can optionally skip the approval step for Copilot coding agent Actions workflows; default remains human approval before workflows run; merge approval requirement is separate. — [GitHub changelog 2026-03-13 (snippet)](https://github.blog/changelog/2026-03-13-optionally-skip-approval-for-copilot-coding-agent-actions-workflows/)
- Community pushback: "Allow us to block Copilot-generated issues (and PRs) from our own repositories" — May 19 2025, 1,837 upvotes, 439 comments. — [discussion #159749](https://github.com/orgs/community/discussions/159749)
- "What security headaches has AI introduced in your projects lately? (2026 edition)" — Apr 24 2026, 13 upvotes, 34 comments; "What's coming to our GitHub Actions 2026 security roadmap" — Mar 26 2026, 32 upvotes, 111 comments. — [discussion #193727](https://github.com/orgs/community/discussions/193727); [discussion #190621](https://github.com/orgs/community/discussions/190621)
- anthropics/claude-code-action: 9.5k stars, 2.2k forks; security doc: "Claude commits code changes to a new branch"; in default configuration "The user must click the link and create the PR themselves, ensuring human oversight before any code is proposed for merging"; "The action can only be triggered by users with write access"; the base action "does not perform actor permission checks"; warns about hidden-markdown prompt injection from external contributors; commits unsigned by default with `use_commit_signing` option. An `agent-approval-check` folder exists in the repo but is not documented on the README or security page. — [claude-code-action repo](https://github.com/anthropics/claude-code-action); [docs/security.md](https://raw.githubusercontent.com/anthropics/claude-code-action/main/docs/security.md)
- Devin enterprise docs (snippet): works under the same branch/SDLC protections as a human engineer; human reviewers decide whether to merge; PR workflow includes standard branch protection and human approval before merging. — [Devin docs: SDLC integration (snippet)](https://docs.devin.ai/enterprise/sdlc-integration)
- OpenAI's Codex GitHub code-review guide positions Codex review as "another review signal before human merge approval"; no explicit human-gate policy for Codex cloud PRs was found. — [OpenAI Codex use case (snippet)](https://developers.openai.com/codex/use-cases/github-code-reviews)
- Third-party tracker (snippet, numbers unverified): merge rates since 2025-05-15 — Copilot agent 77.44%, OpenAI Codex 93.38%, Google Jules 92.96%, Cursor Agent 83.74%, human-authored 90.34%. — [Logicstar "Agents in the Wild" (snippet)](https://insights.logicstar.ai)
- A July 2026 blog summarising an academic study: "Copilot and Devin PRs were more often embedded in reviewer-mediated workflows, whilst Codex and Cursor PRs were typically merged with minimal interaction." (secondary) — [codex.danielvaughan.com (snippet)](https://codex.danielvaughan.com/2026/07/09/agentic-pull-requests-merge-reject-conflict-data-codex-cli-auto-review-agents-md-pr-workflow/)
- Scale context: Octoverse 2025 reports 518.7M merged PRs (+29% YoY), 43.2M per month, and 80% of new developers using Copilot in their first week. — [GitHub Octoverse 2025 (snippet)](https://github.blog/news-insights/octoverse/octoverse-a-new-developer-joins-github-every-second-as-ai-leads-typescript-to-1/)

### Inferences
- The platform-level gates (Copilot's draft+approve, Claude's "user creates the PR") are merge-time gates on a single vendor's agent; none of them covers the deploy step, and none produces a portable signed record. A team running two or more agents (plus humans) has no uniform gate, which is the gap a vendor-neutral approval record fills.
- GitHub's decision to make the Actions-approval step skippable (Mar 2026) is the first sign of the platform trading safety for throughput; teams that keep a gate will need to own it themselves.
- Evidence that agents increase *merge* scrutiny is strong (vendor docs); evidence that they drive purchases of *deploy* gates is absent.

### Gaps
- The `agent-approval-check` component in claude-code-action could not be read (only a folder name was visible); its behaviour is unknown.
- No primary-source numbers on Copilot coding agent PR volume; Octoverse figures are platform-wide.
- The academic study behind the "reviewer-mediated vs minimal interaction" claim could not be identified (page unreachable).

## Key question: How many small teams are plausibly affected, and what is the cheapest current way they solve it?

### Takeaway
No public figure exists for Team-plan orgs or private repos using environments; the affected population is every GitHub Team/Pro org with a private repo that wants a human deploy gate. The cheapest solution today is free in licence cost but not in minutes: trstringer/manual-approval (burns runner time, 6-hour ceiling, 60-minute ceiling for team approvers) or a workflow_dispatch trigger (no gate record, anyone with write access can fire it).

### Cited Findings
- Team plan includes 3,000 Actions minutes/month; Enterprise 50,000. — [GitHub pricing](https://github.com/pricing)
- A paused manual-approval job "will continue to incur costs", fails after 6 hours, and team-approver tokens expire after 1 hour. — [Marketplace listing](https://github.com/marketplace/actions/manual-workflow-approval); [repo README](https://github.com/trstringer/manual-approval)
- Native wait timers do not count toward billable time, but are public-only on Free/Pro/Team. — [GitHub docs source](https://raw.githubusercontent.com/github/docs/main/content/actions/reference/workflows-and-actions/deployments-and-environments.md)
- Community answers recommend "use a public repo, upgrade to Enterprise, or move the approval step outside the native rule". — [discussion #202852](https://github.com/orgs/community/discussions/202852)
- Octoverse 2025: a new developer joins GitHub every second; 518.7M merged PRs in the year. — [Octoverse 2025 (snippet)](https://github.blog/news-insights/octoverse/octoverse-a-new-developer-joins-github-every-second-as-ai-leads-typescript-to-1/)

### Inferences
- For a 2–30 person team the alternatives are: (1) $17/user/month extra for Enterprise ($34–$510/month), (2) a free action costing up to 360 minutes per gate, (3) a trigger with no approver identity or record. A $9–29/org product undercuts (1) for any team of 2+ and removes the minute burn of (2).

### Gaps
- Number of GitHub Team-plan organizations or private repos with environments: not published anywhere found.

## Key question: Would a $9–29/month/org gate with Slack/phone approval, no wasted runner minutes and a signed approval record be bought, and has anyone bought something like it?

### Takeaway
No evidence of anyone selling or buying a product at that price point was found; the closest paid analogues are enterprise-priced (Sleuth median $36.3k/year via Vendr), and the two Marketplace apps in the niche are free with 3–4 installs each. The demand signal is a steady trickle of identical forum questions and a 677-star workaround, not a vote-heavy campaign.

### Cited Findings
- Vendr: Sleuth annual spend $12,800–$41,580, median $36,300, 12 purchases. — [Vendr (snippet)](https://vendr.com/marketplace/sleuth-io)
- Greenlight Actions: 3 installs, free. ApprovalOps: 4 installs, free tier. — [Greenlight](https://github.com/marketplace/greenlight-actions); [ApprovalOps](https://github.com/marketplace/approvalops)
- trstringer/manual-approval: 677 stars, 158 forks. — [repo](https://github.com/trstringer/manual-approval)
- Discussion vote counts on the specific complaint: 0, 1–5 reactions, 2. — [#202852](https://github.com/orgs/community/discussions/202852); [#170307](https://github.com/orgs/community/discussions/170307); [#114537](https://github.com/orgs/community/discussions/114537)
- Vercel's own Slack deploy-approval template has 1 star and 2 commits. — [chat-sdk-deploy-bot](https://github.com/vercel-labs/chat-sdk-deploy-bot)

### Inferences
- Willingness to pay is unproven at the low end; the strongest argument is cost avoidance ($170/month Enterprise delta for a 10-seat team) plus the AI-agent angle, where a signed record is the differentiator no free action offers.
- The low install counts of Greenlight/ApprovalOps could mean weak demand or merely poor discoverability of new Marketplace apps; they are too new and small to be conclusive either way.

### Gaps
- No pricing experiments, waitlists or purchase data for a small-team deploy gate were found.

## Key question: Has GitHub signalled any plan to unbundle required reviewers from Enterprise?

### Takeaway
No. As of October 2026 the docs source still restricts required reviewers, wait timers, custom protection rules and admin bypass to public repos on Free/Pro/Team, the plans page lists "Deployment protection rules … for private or internal repositories" as an Enterprise feature, and no changelog entry or staff statement about unbundling was found.

### Cited Findings
- Current docs source (main branch, fetched 16 Oct 2026) retains the public-only restriction for every protection rule type. — [deployments-and-environments.md](https://raw.githubusercontent.com/github/docs/main/content/actions/reference/workflows-and-actions/deployments-and-environments.md)
- Enterprise feature list includes "Deployment protection rules with GitHub Actions for private or internal repositories". — [githubs-plans.md](https://raw.githubusercontent.com/github/docs/main/content/get-started/learning-about-github/githubs-plans.md)
- A search for a 2026 changelog on the topic returned only docs and community threads, none describing a change. — search performed 16 Oct 2026 (no citable changelog page)
- The Jul 2026 and Aug 2026 community replies still state Enterprise is required. — [#202852](https://github.com/orgs/community/discussions/202852)
- GitHub did loosen an adjacent gate in Mar 2026 (optional skip of Actions approval for Copilot agent workflows), in the opposite direction from adding gates to lower tiers. — [changelog 2026-03-13 (snippet)](https://github.blog/changelog/2026-03-13-optionally-skip-approval-for-copilot-coding-agent-actions-workflows/)

### Inferences
- Platform risk remains: GitHub could move required reviewers to Team at any time (it moved environments to Team/Pro earlier), which would remove the "cheaper than Enterprise" argument but not the Slack/phone channel, the signed record or the multi-platform (GitLab/Railway/Render) coverage.

### Gaps
- The "GitHub Actions 2026 security roadmap" discussion (#190621) was not read; it may contain relevant signals.

## Key question: Does the GitHub Marketplace allow a small publisher to sell such an app, and what are the requirements and fees?

### Takeaway
Yes, provided the app is owned by an organization that passes publisher verification (2FA, verified domain, profile, support email); GitHub recommends 100 installations before a paid GitHub App listing, requires both monthly and annual prices, and keeps 5% of sales.

### Cited Findings
- "only apps owned by organizations can sell their app"; "To publish paid plans for your app and display a marketplace badge, you must complete the publisher verification process"; each paid app "goes through a financial onboarding process to enable payments"; GitHub Apps are preferred; Marketplace apps are not supported for GHE.com data residency. — [about-github-marketplace-for-apps.md](https://raw.githubusercontent.com/github/docs/main/content/apps/github-marketplace/github-marketplace-overview/about-github-marketplace-for-apps.md)
- Paid-app requirements: "your app must be owned by an organization that is a verified publisher"; "GitHub Apps should have a minimum of 100 installations"; "All paid apps must handle GitHub Marketplace purchase events for new purchases, upgrades, downgrades…"; "Apps must support both monthly and annual billing"; listing needs privacy policy, support link, logo/feature card/screenshots, webhook events for plan changes. — [requirements-for-listing-an-app.md](https://raw.githubusercontent.com/github/docs/main/content/apps/github-marketplace/creating-apps-for-github-marketplace/requirements-for-listing-an-app.md)
- Publisher verification: org 2FA enabled, verified domain with "Verified" badge, accurate profile, support email, requester must be org owner; request via Developer settings → Publisher Verification. — [applying-for-publisher-verification-for-your-organization.md](https://raw.githubusercontent.com/github/docs/main/content/apps/github-marketplace/github-marketplace-overview/applying-for-publisher-verification-for-your-organization.md)
- Pricing plans: "three types of pricing plan: free, flat-rate, and per-unit"; "up to 10 pricing plans"; "You must set a price for both monthly and yearly subscriptions in US dollars"; a published plan cannot be edited, only removed and recreated; non-verified publishers cannot change plans. — [setting-pricing-plans-for-your-listing.md](https://raw.githubusercontent.com/github/docs/main/content/apps/github-marketplace/listing-an-app-on-github-marketplace/setting-pricing-plans-for-your-listing.md)
- Developer Agreement (effective May 27 2025): "GitHub will remit 95% of the sale price in USD without reduction for Taxes"; "The remaining 5% of the sales price will be allotted to and retained by GitHub"; first remittance within 90 days of completed registration, then monthly. — [github-marketplace-developer-agreement.md](https://raw.githubusercontent.com/github/docs/main/content/site-policy/github-terms/github-marketplace-developer-agreement.md)

### Inferences
- Path for a small publisher: list free first (no verification needed), reach ~100 installs, then verify the org and add flat-rate $9–29 plans with annual variants. The 5% fee is lower than typical app-store cuts, but Marketplace billing only covers GitHub orgs; GitLab/Railway/Render customers need separate billing.
- Install counts of 3–4 for comparable new apps suggest reaching 100 installs will depend on distribution outside the Marketplace.

### Gaps
- Whether the 100-install threshold is enforced strictly or waived case-by-case is not stated ("should", not "must").

## What the reader would still need to build (given an existing approval-gate API with signed receipts and Slack/Teams/phone/web delivery)

### Takeaway
Because custom deployment protection rules are Enterprise-only on private repos, the GitHub integration cannot ride GitHub's native pending-deployment hook for the target segment; it must split the pipeline and resume the deploy job from outside, or block via a required status check.

### Cited Findings
- Custom deployment protection rules (the GitHub App `deployment_protection_rule` mechanism) are public-only on Free/Pro/Team. — [deployments-and-environments.md](https://raw.githubusercontent.com/github/docs/main/content/actions/reference/workflows-and-actions/deployments-and-environments.md)
- Required PR reviewers and protected branches are available on Team for private repos. — [githubs-plans.md](https://raw.githubusercontent.com/github/docs/main/content/get-started/learning-about-github/githubs-plans.md)
- Vercel's template resumes by dispatching a workflow with a fine-grained PAT holding Actions read/write and a `GITHUB_WORKFLOW_ID`. — [chat-sdk-deploy-bot](https://github.com/vercel-labs/chat-sdk-deploy-bot)
- Marketplace paid listing needs a GitHub App owned by a verified org, purchase-event webhooks, monthly+annual plans. — [requirements-for-listing-an-app.md](https://raw.githubusercontent.com/github/docs/main/content/apps/github-marketplace/creating-apps-for-github-marketplace/requirements-for-listing-an-app.md)

### Inferences (build list)
- A GitHub App (not a PAT) with Actions: write, Checks: write, Contents/Deployments: read, installed per org, so approvals resume a `workflow_dispatch`/`repository_dispatch` deploy workflow without any runner waiting; or alternatively post a check-run that a ruleset's required status check consumes for merge gating (available on Team private repos).
- A "resume-dispatch" mechanism: the gate step ends the build job after registering the approval request (zero waiting minutes); on approval the service dispatches the deploy workflow with the pinned commit SHA and artifact reference, and writes the signed receipt URL into the run summary/commit status.
- Marketplace billing: purchase/upgrade/downgrade/cancel webhook handling, plan-to-org mapping, annual pricing; plus a non-GitHub billing path for GitLab/Railway/Render users.
- Optional on Enterprise: register as a custom deployment protection rule for teams that already have Enterprise and only want the Slack/phone channel and the receipt.

### Gaps
- No primary source on whether `workflow_dispatch` from a GitHub App token counts as the approver's identity in GitHub's audit log; receipt would carry identity instead.

## Opportunity rating

### Takeaway
Demand evidence: MODERATE — the question recurs annually with consistent "upgrade or use a third-party action" answers, a 677-star workaround with 158 forks exists, and AI-agent vendors are adding merge gates, but direct votes are low and no Stack Overflow/Reddit/HN threads were found. Competitive gap: OPEN — no paid product at $9–29/org with Slack/phone approval and a signed record was found; incumbents are free actions that burn minutes or enterprise platforms at $10k+/year; the main risk is GitHub unbundling the feature, for which there is no signal.

### Cited Findings
- Demand: [#202852](https://github.com/orgs/community/discussions/202852), [#170307](https://github.com/orgs/community/discussions/170307), [#114537](https://github.com/orgs/community/discussions/114537), [issue #95](https://github.com/trstringer/manual-approval/issues/95), [677 stars](https://github.com/trstringer/manual-approval), [Copilot gate (snippet)](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/cloud-agent/review-copilot-prs).
- Gap: [Marketplace apps search](https://github.com/marketplace?query=approval&type=apps), [Marketplace actions search](https://github.com/marketplace?query=manual+approval&type=actions), [Vendr Sleuth (snippet)](https://vendr.com/marketplace/sleuth-io), [Spacelift 2026 listing (snippet)](https://agentdeals.dev/vendor/spacelift).

### Inferences
- Cheapest wedge: ship as a free GitHub Action + GitHub App that ends the job instead of waiting (immediate, measurable saving in minutes versus trstringer), collect installs toward the 100 threshold, then charge for the signed receipt, phone escalation and multi-platform coverage.

### Gaps
- No evidence either way on conversion from free action users to paid.

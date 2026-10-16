# GitHub and GitLab plan paywalls that small teams hit, which ones a third party can unbundle, and evidence people pay for that (as of 16 Oct 2026)

**The question.** A solo founder has an approval-gate API:
- a named human approves in Slack, Teams, by phone or on the web;
- grants are one-time and time-boxed;
- every decision is sealed in a tamper-evident record with a signed receipt;
- a policy engine answers proceed, route or block.

Earlier research found three things:
- On private repos, GitHub keeps deployment required reviewers, wait timers and custom deployment protection rules on Enterprise only. Team costs $4/user/mo and Enterprise $21.
- The free workaround, trstringer/manual-approval (677 stars), burns runner minutes and is capped at about 60 minutes for team approvals.
- No paid app serves the $9–29 per org per month band.

This note asks how far the "friction arbitrage" goes beyond deploy approvals.

**Method and source quality.**
- **GitHub docs.** I read the docs from source. I made a sparse clone of `github/docs` at commit `dae83f7`, dated 16 Oct 2026 (`data/reusables/gated-features/*`, `data/features/*.yml`, and the content for repositories, organizations, actions, copilot and billing). URLs below point to raw.githubusercontent.com/github/docs/main/…
- **GitLab docs.** I read them from gitlab.com/gitlab-org/gitlab/-/raw/master/doc/…, and searched issue upvotes through the public GitLab issues API.
- **GitHub pages fetched directly with WebFetch.** Marketplace listings, community discussions, repository pages, the pricing page and the about page.
- **Unreachable sites.** These hosts did not resolve, so anything from them comes from search snippets and is marked **[snippet]**: mergify.com, aviator.co, kodiakhq.com, about.gitlab.com, github.blog, workos.com, octoverse.github.com, wellarchitected.github.com and capitalandcompute.net.
- **Code-search counts.** These came from the GitHub code search API. They count public repositories only; private usage is invisible. They are marked **[code search]**.
- **Unverified items.** Statements from my own background knowledge are marked **[prior knowledge, unverified]**.
- **Budget.** 25 web searches, 11 code searches, about 30 page fetches.

---

## Bottom line

- **GitHub has been moving governance down to Team for 18 months.** The moves so far:
  - Organization rulesets (16 Jun 2025).
  - GHAS products purchasable on Team (1 Apr 2025).
  - Push rulesets on Team.
  - The path-based required-reviewer rule (GA 17 Feb 2026).
  - Workflow execution protections. These cover actor and event allowlists per workflow, including Copilot and GitHub Apps as actors. Public preview came 18 Jun 2026 and GA on 17 Sep 2026, on private repos for Team.
- **Several things a small team wants are already native on Team.** CODEOWNERS, required PR reviews, org rulesets, push rules, path-based team approvals, environment secrets and deployment-branch policies, and "who and what can trigger this workflow". Unbundling those has no price headroom left.
- **What stays Enterprise-only on private repos** (Team $4 vs Enterprise $21, a $17/user/mo gap):
  1. Deployment protection rules: required reviewers, wait timers, custom protection-rule apps, and the admin-bypass setting.
  2. Merge queue.
  3. Required ("ruleset") workflows.
  4. Commit-metadata rules.
  5. Evaluate (dry-run) mode for rulesets and Actions policies.
  6. Audit log REST/GraphQL API and audit log streaming, including agent-session event streaming.
  7. IP allow list.
  8. SAML/SCIM, Enterprise Managed Users (EMU) and custom organization roles.
  9. Copilot Enterprise ($39, which requires Enterprise Cloud).
- **A third party can only replicate 1 to 4 on Team.** Items 6 to 8 are identity or network controls that no app can replace. On Free private repos almost nothing is enforceable: Free has no protected branches, rulesets, environments or execution protections, so no third-party check can ever be "required".
- **People do pay for unbundled GitHub gating, but only in two places:**
  - **Merge queue** (crowded): Mergify 10,593 installs, Aviator 3,873, Kodiak 1,368. Graphite was acquired by Cursor in Dec 2025. Prices run $12–40/user/mo.
  - **Flat-priced merge gating:** Merge Freeze, 1,538 installs, at $25 or $40 per month flat. Merge Freeze is the closest proof that a $25–40 per org per month gating app sells.
  - Review-policy apps sell too, but small: PullApprove has 523 installs at $4–6 per reviewer per month.
  - No paid deploy-approval app exists for the Team or Free band.
- **AI agents add need, but GitHub is covering the Copilot path natively.** Copilot pull requests need "Approve and run workflows". PRs opened under Copilot's own app identity need one extra approval. Actor rules can block any agent from privileged workflows.
- **The agent gaps left on Team are deploy-time ones:**
  - a human approval before an agent-initiated run touches production or secrets;
  - "approve-then-run" for agents other than Copilot (Claude, Codex, Devin bots);
  - an exportable record of which agent did what (audit API and streaming are Enterprise only).
  - Since 1 Sep 2026, Copilot code review can also count as a required approval (opt-in, preview), which creates a new need to prove that a named human approved.
- **Verdict.** Only one product is worth building as the core: deploy approvals, repositioned as "approval for privileged workflow runs, whether a human or an agent started them". Two add-ons raise revenue per customer on top of it:
  - (a) a sealed change and deploy ledger that stands in for the missing audit API;
  - (b) freeze windows and emergency-change approvals, benchmarked against Merge Freeze at $25–40/mo.

  Everything else falls into one of these groups:
  - native on Team now (CODEOWNERS, rulesets, path reviewers, actor restrictions);
  - crowded and a poor fit for an approval gate (merge queue);
  - impossible for a third party (IP allow list, SSO, Free-plan branch protection);
  - tied to Copilot seat plans rather than the GitHub plan (agent governance).
- **GitLab is a weak second market.** Free private namespaces are capped at 5 users, so most teams already pay Premium ($29), which includes approvals and protected environments.

---

## Q1. Inventory of plan-gated features for private repositories

### Takeaway
Two kinds of friction show up:
- **Free to Team:** basic protection. This gate is cheap at $4, and buyers on Free are often students with low willingness to pay.
- **Team to Enterprise ($17/user/mo more):** deployment protection, merge queue, required workflows, audit API and streaming, network and identity controls.

Direct community demand for any single Enterprise-only feature is a trickle: 0 to 11 votes per thread in 2024–2026. Usage of workarounds is larger: 1,080 public workflow files reference trstringer/manual-approval and 4,552 public repos have a `.mergify.yml`.

### Cited findings

**Prices and plan facts**
- Free is $0. Team is listed as "$4 USD per user/month for the first 12 months*" and Enterprise as "Starting at $21 USD per user/month for the first 12 months*". I could not retrieve the asterisk footnote. Actions minutes for private repos: 2,000 on Free, 3,000 on Team and 50,000 on Enterprise. — [GitHub pricing (fetched)](https://github.com/pricing)
- Enterprise "includes … Deployment protection rules with GitHub Actions for private or internal repositories … audit log streaming and IP allow list". — [githubs-plans.md](https://raw.githubusercontent.com/github/docs/main/content/get-started/learning-about-github/githubs-plans.md)
- Downgrading Team to Free loses required reviewers, protected branches, code owners, Pages, wikis, multiple assignees and reviewers, and autolinks in private repos. — [faq-about-changes-to-githubs-plans.md](https://raw.githubusercontent.com/github/docs/main/content/get-started/learning-about-github/faq-about-changes-to-githubs-plans.md)
- Secret Protection costs $19 and Code Security $30 per active committer per month. Team customers have been able to buy both since 1 Apr 2025. **[snippet]** — [changelog 2025-03-04](https://github.blog/changelog/2025-03-04-introducing-github-secret-protection-and-github-code-security/); [changelog 2025-04-01](https://github.blog/changelog/2025-04-01-github-advanced-security-is-here-for-github-team-organizations). The docs confirm "You must be on a GitHub Team or GitHub Enterprise plan in order to purchase". — [about-github-advanced-security.md](https://raw.githubusercontent.com/github/docs/main/content/get-started/learning-about-github/about-github-advanced-security.md)
- Copilot Business is $19 and Copilot Enterprise $39 per granted seat per month. Copilot Enterprise is "designed for enterprises using GitHub Enterprise Cloud". — [plans.md](https://raw.githubusercontent.com/github/docs/main/content/copilot/get-started/plans.md) and [data/variables/copilot.yml](https://raw.githubusercontent.com/github/docs/main/data/variables/copilot.yml). All Copilot plans moved to usage-based AI-credit billing (#197089, 27 May 2026, 87 votes, about 1k comments). — [discussion #197089](https://github.com/orgs/community/discussions/197089)
- Actions: GitHub cut hosted-runner prices by up to 39% from 1 Jan 2026 and postponed the self-hosted runner charge with no new date. #182186 has 264 thumbs-up and 68 comments plus 71 replies. — [discussion #182186](https://github.com/orgs/community/discussions/182186)

**Feature inventory.** Plan required for private repos, sourced from the docs unless marked.

| # | Feature | Free (org) | Team ($4) | Enterprise ($21) | Source | Demand signal (date, votes) |
|---|---|---|---|---|---|---|
| 1 | Environment **required reviewers** | ✗ | ✗ (public repos only) | ✓ | [deployments-and-environments.md](https://raw.githubusercontent.com/github/docs/main/content/actions/reference/workflows-and-actions/deployments-and-environments.md) | #202852 (24 Jul 2026, 0 votes); #170307 (19 Aug 2025, 2 votes, 5 on the answer); #114537 (Mar 2024, 2); #43150 (2 Jan 2023, 4: "wait time … is table stakes for any CI/CD system"); trstringer/manual-approval: 677 stars, **1,080 public workflow files [code search]**, plus 594 other "manual-approval" workflow refs [code search] |
| 2 | **Wait timer** | ✗ | ✗ (public only) | ✓ | same | folded into #43150 |
| 3 | **Custom deployment protection rules** (Datadog, Honeycomb, Sentry and similar apps) | ✗ | ✗ (public only) | ✓ | same | No threads found. The Marketplace "Deployment Protection Rules" category lists only 4 apps. — [category](https://github.com/marketplace?type=apps&category=deployment-protection-rules) |
| 4 | Admin bypass control on environments | ✗ | ✗ (public only) | ✓ | same | none found |
| 5 | Environments, environment secrets, deployment branches and tags | ✗ (Free: public only) | ✓ | ✓ | same; [gated-features/environments.md](https://raw.githubusercontent.com/github/docs/main/data/reusables/gated-features/environments.md) | #15379 "Organization-wide Environments and Environment Secrets" (22 Apr 2022, **799 votes**). This asks for a missing feature, not a paywalled one. |
| 6 | Protected branches / branch protection | ✗ | ✓ | ✓ | [protected-branches.md](https://raw.githubusercontent.com/github/docs/main/data/reusables/gated-features/protected-branches.md) | #174419 "Please Make Basic Branch Protection Free for Private Repositories" (23 Sep 2025, **102 votes**, 71 👍; last +1 on 15 Oct 2026; no staff reply; OP is a student group that "doesn't have money") — [#174419](https://github.com/orgs/community/discussions/174419) |
| 7 | Repo rulesets (branch and tag) | ✗ | ✓ | ✓ | [repo-rules.md](https://raw.githubusercontent.com/github/docs/main/data/reusables/gated-features/repo-rules.md) | #184363 (Jan 2026, 2 votes): a Team user still hit "won't be enforced … until you upgrade" (from earlier notes) |
| 8 | **Org-level rulesets** | ✗ | ✓ since 16 Jun 2025 | ✓ | [creating-rulesets-for-repositories-in-your-organization.md](https://raw.githubusercontent.com/github/docs/main/content/organizations/managing-organization-settings/creating-rulesets-for-repositories-in-your-organization.md) ("For customers on GitHub Team or GitHub Enterprise plans"); [changelog 2025-06-16 [snippet]](https://github.blog/changelog/2025-06-16-organization-rulesets-now-available-for-github-team-plans/) | n/a. No longer paywalled. |
| 9 | **Push rulesets** (file path, extension, size) | ✗ | ✓ (private and internal) | ✓ | [repo-rules.md](https://raw.githubusercontent.com/github/docs/main/data/reusables/gated-features/repo-rules.md) | n/a |
| 10 | **Required ("ruleset") workflows** | ✗ | ✗ | ✓ | `repo-rules-required-workflows` is versioned `ghec` only. — [feature flag](https://raw.githubusercontent.com/github/docs/main/data/features/repo-rules-required-workflows.yml); [available-rules-for-rulesets.md](https://raw.githubusercontent.com/github/docs/main/content/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/available-rules-for-rulesets.md) | No Team-specific thread found |
| 11 | **Commit-metadata restrictions** (message, author email, branch-name regex) | ✗ | ✗ | ✓ | "Organizations on a GitHub Enterprise plan can access additional rules…" (same page); `repo-rules-enterprise` flag is `ghec` only — [flag](https://raw.githubusercontent.com/github/docs/main/data/features/repo-rules-enterprise.yml) | none found |
| 12 | Ruleset **Evaluate** mode and rule insights | ✗ | ✗ | ✓ | same flag; [rulesets-about-enforcement-statuses.md](https://raw.githubusercontent.com/github/docs/main/data/reusables/repositories/rulesets-about-enforcement-statuses.md) | none |
| 13 | **Path-based required-reviewer rule** (up to 15 teams, file patterns) | ✗ | ✓ | ✓ | flag `repo-rules-required-reviewer` (fpt, ghec); GA 17 Feb 2026 **[snippet]** — [changelog](https://github.blog/changelog/2026-02-17-required-reviewer-rule-is-now-generally-available/) | n/a |
| 14 | CODEOWNERS (enforced via protection) | ✗ | ✓ | ✓ | [code-owners.md](https://raw.githubusercontent.com/github/docs/main/data/reusables/gated-features/code-owners.md) | n/a |
| 15 | **Merge queue** | ✗ | ✗ | ✓ | "private repositories owned by organizations using GitHub Enterprise Cloud" — [merge-queue.md](https://raw.githubusercontent.com/github/docs/main/data/reusables/gated-features/merge-queue.md) | #201908 "Request: Make merge queues available to Teams plan" (14 Jul 2026, 11 votes, 4 👍; one user said on 24 Aug 2026 it "seems to be working" on their Team org, contradicting the docs) — [#201908](https://github.com/orgs/community/discussions/201908); 36,736 public workflow files use `merge_group:` [code search] |
| 16 | Repository custom properties | ✓ | ✓ | ✓ (plus enterprise-level) | [managing-custom-properties…md](https://raw.githubusercontent.com/github/docs/main/content/organizations/managing-organization-settings/managing-custom-properties-for-repositories-in-your-organization.md) (versions fpt, ghec, ghes; no plan gate) | n/a |
| 17 | **Audit log API** (REST/GraphQL) | ✗ | ✗ | ✓ | "To use the audit log API, your organization must use GitHub Enterprise Cloud." The UI keeps 180 days with manual JSON/CSV export on all plans. — [reviewing-the-audit-log-for-your-organization.md](https://raw.githubusercontent.com/github/docs/main/content/organizations/keeping-your-organization-secure/managing-security-settings-for-your-organization/reviewing-the-audit-log-for-your-organization.md) | No Team-plan thread found in two searches |
| 18 | **Audit log streaming**, API-request events, source IP disclosure | ✗ | ✗ | ✓ | githubs-plans.md; same audit page | none found |
| 19 | **IP allow list** | ✗ | ✗ | ✓ | versioned `ghec` only — [managing-allowed-ip-addresses…md](https://raw.githubusercontent.com/github/docs/main/content/organizations/keeping-your-organization-secure/managing-security-settings-for-your-organization/managing-allowed-ip-addresses-for-your-organization.md) | none found |
| 20 | SAML SSO, SCIM, EMU, custom org roles, verified domains, private Pages | ✗ | ✗ | ✓ | githubs-plans.md; [managing-custom-organization-roles.md](https://raw.githubusercontent.com/github/docs/main/content/organizations/managing-peoples-access-to-your-organization-with-roles/managing-custom-organization-roles.md) | not researched |
| 21 | Secret scanning and push protection (private repos) | ✗ | Paid add-on: Secret Protection $19/committer | add-on | [secret-protection.md](https://raw.githubusercontent.com/github/docs/main/data/reusables/gated-features/secret-protection.md); [delegated-bypass.md](https://raw.githubusercontent.com/github/docs/main/data/reusables/gated-features/delegated-bypass.md) (delegated bypass is on Team with Secret Protection) | #186254 "surprise github security charges" (Feb 2026, 1 vote) |
| 22 | **Workflow execution protections** (actor and event allowlists per workflow, including Copilot and GitHub Apps as actors) | ✗ on private (all public repos ✓) | ✓ | ✓ (plus Evaluate mode) | "All public repositories, and private repositories on GitHub Team or GitHub Enterprise." Evaluate mode is Enterprise Cloud only. — [control-workflow-execution.md](https://raw.githubusercontent.com/github/docs/main/content/actions/how-tos/administer/control-workflow-execution.md); [about-actions-policies.md](https://raw.githubusercontent.com/github/docs/main/content/actions/concepts/about-actions-policies.md); GA 17 Sep 2026 **[snippet]** — [changelog](https://github.blog/changelog/2026-09-17-workflow-execution-protections-in-github-actions-generally-available/) | Roadmap thread #190621 (26 Mar 2026, 32 votes, 111 comments) |
| 23 | Workflow dependency lockfiles, "Require lockfile" policy | public preview | Policy page says Team+; the enforcement guide's product line says Enterprise Cloud (docs conflict) | ✓ | [enforce-workflow-lockfiles.md](https://raw.githubusercontent.com/github/docs/main/content/actions/how-tos/administer/enforce-workflow-lockfiles.md) | none |
| 24 | Allowed-actions and SHA-pinning policies | ✓ | ✓ | ✓ | no plan gate in [actions-use-policy-settings.md](https://raw.githubusercontent.com/github/docs/main/data/reusables/actions/actions-use-policy-settings.md) | n/a |
| 25 | Larger runners, Azure private networking, runner groups | ✗ | ✓ | ✓ (plus static IPs) | [manage-larger-runners.md](https://raw.githubusercontent.com/github/docs/main/content/actions/how-tos/manage-runners/larger-runners/manage-larger-runners.md) | n/a |
| 26 | Copilot cloud agent governance: org policy toggle, repo access list, automations toggle, firewall, MCP, org custom instructions | needs Copilot Business ($19) on any org plan | same | Copilot Enterprise ($39) adds enterprise policies; agent-session events can be streamed via enterprise audit streaming | [add-copilot-cloud-agent.md](https://raw.githubusercontent.com/github/docs/main/content/copilot/how-tos/administer-copilot/manage-for-organization/add-copilot-cloud-agent.md); [add-organization-instructions.md](https://raw.githubusercontent.com/github/docs/main/content/copilot/how-tos/copilot-on-github/customize-copilot/add-custom-instructions/add-organization-instructions.md) ("Organizations with a Copilot Business or Copilot Enterprise plan"); [monitor-agentic-activity.md](https://raw.githubusercontent.com/github/docs/main/content/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/monitor-agentic-activity.md) | #159749 "Allow us to block Copilot-generated issues (and PRs)" (19 May 2025, **1,837 votes**, 159 comments + 280 replies, no staff reply). This is an OSS-maintainer complaint, not a paywall complaint. — [#159749](https://github.com/orgs/community/discussions/159749) |

Adjacent high-vote threads that are not paywalls:
- #13836, "allow Actions to bypass branch protection": 1,868 votes, 30 Mar 2022. Largely addressed by ruleset bypass lists on Team.
- #10809, "Disable squash on specific branches": 1,548 votes.

— [#13836](https://github.com/orgs/community/discussions/13836); [#10809](https://github.com/orgs/community/discussions/10809)

### Inferences
- **The friction GitHub keeps is mostly "deploy-time" and "evidence-time":** environments, the audit API and streaming, Evaluate mode. "Merge-time" friction has been almost entirely released to Team. The exception is merge queue, and even that may be loosening (the user report on #201908).
- **Low vote counts on paywall threads do not mean low demand.** Paying customers tend to open support tickets or upgrade quietly (#43150: "I upgraded for this"), and people asking for Free features are often unwilling to pay.
- **Team pricing may change.** The "first 12 months*" label on the pricing page suggests GitHub may be preparing a price change or promotional framing for Team and Enterprise. That would change the headroom maths either way.

### Gaps
- The pricing-page footnote behind "first 12 months*" is unknown.
- I could not find demand threads specific to the audit API, IP allow list, required workflows or metadata rules (two searches; GitHub's discussion search is noisy).
- Whether merge queue now works on Team (the #201908 report) is unverified.

---

## Q2. Can a third party replicate each paywalled feature on Team or Free?

### Takeaway
On Team, a GitHub App can replicate the deploy gate robustly. Since Sep 2026, workflow execution protections can restrict a deploy workflow so that only the approval app can trigger it, and only via `workflow_dispatch`. Deployment-branch policies and environment secrets, both on Team, keep secrets bound to protected refs.

On Free private repos nothing can be made "required". The only robust pattern there is that the gate holds the deploy credential and releases it, one time and time-boxed, against an OIDC-verified workflow run. This pattern is also the best fit for the founder's "one-time, time-boxed grant".

### Mechanisms (docs-backed)
- Required status checks can be pinned to a specific GitHub App source on Team rulesets ("you can select an app as the expected source of status updates"). Org-level rulesets on Team can require an app's check across every repo. — [available-rules-for-rulesets.md](https://raw.githubusercontent.com/github/docs/main/content/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/available-rules-for-rulesets.md)
- Actor rules "control who can trigger workflows, including individual users, repository roles, GitHub Apps, Copilot, and Dependabot". Event rules restrict `push`, `pull_request`, `pull_request_target` and `workflow_dispatch`. At GA, policies can target specific workflow files. — [about-actions-policies.md](https://raw.githubusercontent.com/github/docs/main/content/actions/concepts/about-actions-policies.md)
- Environment secrets and deployment branches are on Team for private repos. "If the environment requires approval, a job cannot access environment secrets until one of the required reviewers approves it" applies only where reviewers are available, which on private repos means Enterprise. — [deployments-and-environments.md](https://raw.githubusercontent.com/github/docs/main/content/actions/reference/workflows-and-actions/deployments-and-environments.md)
- OIDC tokens for Actions have no plan gate in the docs, so a gate can verify the repo, workflow, ref, environment and run_id before releasing a short-lived credential. — [openid-connect.md](https://raw.githubusercontent.com/github/docs/main/content/actions/concepts/security/openid-connect.md)

### Feasibility table

| Paywalled feature | On Team | On Free (private) | Mechanism and caveats |
|---|---|---|---|
| Required reviewers, wait timer, custom protection rule | **Feasible** | **Partial** | **Team:** the deploy workflow listens only on `workflow_dispatch`. A Team actor rule allows only the gate's GitHub App to trigger it. The environment's deployment-branch rule limits it to `main` and holds the environment secrets. The gate dispatches after a named approval; a wait timer is a scheduled dispatch. No runner idles while waiting, unlike trstringer. **Free:** there are no environments or execution protections on private repos, so the gate must broker the credential via OIDC. Without that, anyone with write access can copy the step. |
| Admin-bypass control | Feasible (policy lives in the gate) | Partial | The gate decides who may break glass and seals the record. |
| Merge queue | Feasible (proven) | Impossible to enforce | Speculative merge-group branches plus a required check (Mergify, Aviator, Trunk, Graphite, Kodiak). |
| Required workflows | **Feasible** | Impossible to enforce | The app runs a central check (or dispatches a central workflow via `repository_dispatch`) and posts a check run. An org ruleset (Team) requires that app's check in every repo. |
| Commit-metadata rules | Feasible | Impossible to enforce | A check run validates messages, emails and branch names (commitlint-style). It blocks the merge but cannot block a push; push rulesets cover only paths, extensions and sizes. |
| Evaluate (dry-run) mode | Feasible | Partial | Run the policy as a non-required check, or have the gate record "would have blocked". |
| Audit log API and streaming | **Partial** | Partial | Org webhooks (all plans) give a live feed of many events: repository, member, team, `branch_protection_rule`, `repository_ruleset`, deployment, `workflow_run`, pull_request. The audit-log-only events (token use, git and API events, many settings changes) are unreachable without the API. The fallback is the manual UI export (180 days; Git events limited). The gate can be the system of record for everything it mediates. |
| IP allow list | **Impossible** | Impossible | Network access to github.com cannot be intercepted by an app. |
| SAML / SCIM / EMU / custom roles | **Impossible** | Impossible | Identity-plane features. |
| Branch protection and CODEOWNERS on Free private | n/a (native on Team) | **Impossible** | Nothing can be made required. A bot could only alert or revert after the fact, which is hacky. |
| Secret push protection | Native add-on ($19) | Impossible at push time | GitHub.com has no pre-receive hooks; third parties scan after the push (GitGuardian, Gitleaks). |
| Copilot agent governance | Mostly native with Copilot Business | n/a | Enterprise-only parts (agent-session audit streaming, enterprise policies) are partial via webhooks or session logs. |
| "Approve-then-run" for non-Copilot agents (Claude, Codex, Devin bots) | **Partial to feasible** | Partial | An actor rule blocks the agent's GitHub App from triggering privileged workflows. The gate re-dispatches after a human approves. Unprivileged CI can still run on agent PRs. |

### Inferences
- **Workflow execution protections (GA 17 Sep 2026) are the key new enabler.** Before that, a dispatch-based gate on Team could be bypassed by any writer who manually ran `workflow_dispatch`. Now the gate can be made the only allowed actor. The best moment to launch is right after GitHub shipped this, before anyone else notices.
- **The same feature erodes the "restrict which workflows agents can trigger" pitch.** That is now native on Team, so the product must sell approval plus receipts, not restriction.

### Gaps
- I did not test whether a re-run by a human of an agent-triggered `pull_request` run inherits the agent's actor for actor-rule evaluation.
- I did not test whether `workflow_dispatch` by an app installation token counts as the app actor in actor rules. Both are **[prior knowledge, unverified]** and need a hands-on test.

---

## Q3. Who already sells unbundled versions, with traction and price

### Takeaway
There are paid unbundlers in merge queue (crowded, $12–40/user/mo), merge freezes (Merge Freeze, $25 or $40 flat) and review policy (PullApprove, $4–6/reviewer). There are none in Team-band deploy approvals or Team-band audit export. Policy-as-code tools are open source and self-hosted; OpenSSF's hosted Allstar app was retired. Agent-governance tools are enterprise or early-access (WorkOS Airlock, Endor Labs).

### Cited findings

**GitHub Marketplace listings (fetched 16 Oct 2026)**

| App | Installs | Listed price | Notes | Source |
|---|---|---|---|---|
| Mergify | **10,593** | Marketplace shows only a $0 open-source plan. Off-Marketplace: free up to 5 users on private repos; paid tier $21/seat/mo ("Max"), though the live pricing snippet shows a $14/seat "Advanced" tier **[snippet; conflict]** | Merge queue, CI Insights, "Merge Protections"; verified publisher; **4,552 public `.mergify.yml` files [code search]** | [Marketplace](https://github.com/marketplace/mergify); [mergify.com/pricing (snippet)](https://mergify.com/pricing); [toolradar (snippet)](https://toolradar.com/tools/mergify/pricing) |
| Aviator | **3,873** | $0 on Marketplace; paid from about $12/active dev/mo, free under about 15 devs **[snippet, aggregator]** | MergeQueue; verified | [Marketplace](https://github.com/marketplace/mergequeue); [toolradar (snippet)](https://toolradar.com/tools/aviator/pricing) |
| Kodiak | **1,368** | $0 Base plan | Auto-merge and queue; not verified; 770 public `.kodiak.toml` [code search] | [Marketplace](https://github.com/marketplace/kodiakhq) |
| **Merge Freeze** | **1,538** | **Reactive $25/mo; Proactive $40/mo** (scheduled freezes, API, up to 50 repos); free for OSS | Slack `/mergefreeze`, one-off and recurring freezes; verified | [Marketplace](https://github.com/marketplace/merge-freeze) |
| **PullApprove** | **523** | **$4/reviewer/mo** (Team, up to 10 reviewers); **$6/reviewer/mo** (Organization) | "Code review assignment and custom workflows"; verified; 2,040 public `.pullapprove.yml` [code search] | [Marketplace](https://github.com/marketplace/pullapprove) |
| Branch Guard | 7 | $0 | YAML approval_gate check run by file paths; listed under Deployment Protection Rules | [Marketplace](https://github.com/marketplace/branch-guard) |
| Astriguard | 8 | $0 (1 repo, 50 scans) | "Audit Writer" timeline for auditors; status-check gate | [Marketplace](https://github.com/marketplace/astriguard) |
| Greenlight Actions / ApprovalOps | 3 / 4 | $0 | from earlier notes | [Greenlight](https://github.com/marketplace/greenlight-actions); [ApprovalOps](https://github.com/marketplace/approvalops) |

**Off-Marketplace, open source or acquired**
- Graphite: $20/user/mo (Starter) and $40/user/mo (Team), billed annually **[snippet]**. Acquired by Cursor in Dec 2025, reportedly above its last $290M valuation, after a $52M Series B **[snippet, secondary]**. — [graphite.dev/pricing (snippet)](https://graphite.dev/pricing); [learncursor.dev (snippet)](https://www.learncursor.dev/learn/cursor-origin/graphite-acquisition). GitHub's own "Stacked pull requests" entered public preview on 9 Jul 2026 (#201439, 44 votes, 719 comments). — [#201439](https://github.com/orgs/community/discussions/201439)
- Trunk Merge Queue: unlimited merges, with "pricing begins if >5 private repo committers"; per-seat Team plan **[snippet]**. — [docs.trunk.io billing (snippet)](https://docs.trunk.io/setup-and-administration/billing)
- palantir/policy-bot: about 1.1k stars, 160 forks; approval policies posted as a status check; self-hosted; 137 public `.policy.yml` with `approval_rules` [code search]. — [repo](https://github.com/palantir/policy-bot)
- github/safe-settings: 925 stars, 228 forks; "Policy-as-Code for GitHub Organizations"; self-hosted. — [repo](https://github.com/github/safe-settings)
- ossf/allstar: about 1.5k stars; "the OpenSSF-hosted Allstar GitHub App has been retired"; now self-hosted only. — [repo](https://github.com/ossf/allstar)
- repository-settings/app: about 1.1k stars, 189 forks; hosted version offered; settings sync via `.github/settings.yml`. — [repo](https://github.com/repository-settings/app)
- Deploy-approval workarounds: trstringer/manual-approval (677 stars; 1,080 public workflow refs); the sede-open/manual-approval fork; Vercel's Slack deploy-bot template (updated Jun 2026, a reference implementation, not a product). — [trstringer](https://github.com/trstringer/manual-approval); [sede-open fork](https://github.com/sede-open/manual-approval); [Vercel KB (snippet)](https://vercel.com/kb/guide/run-and-track-deploys-from-slack)

**Agent-governance tools**
- WorkOS Airlock is in early access. It "evaluates every call an agent makes against its intent and then allows it, denies it, or sends it to a human", with approvals in Slack. It works with Claude Code, Codex and MCP gateways. No public price. **[snippet]** — [workos.com/airlock (snippet)](https://workos.com/airlock); [WorkOS blog: AI coding agent GitHub merge policies (snippet, Sep 2026)](https://workos.com/blog/ai-coding-agent-github-merge-policies)
- Endor Labs "AI Code Governance": policy and an audit trail on every agent PR. CodeRabbit publishes a guide to governing coding agents at the merge gate. **[snippet]** — [Endor Labs](https://www.endorlabs.com/learn/ai-code-governance-secure-ai-generated-code-and-govern-the-agents-that-write-it); [CodeRabbit guide](https://www.coderabbit.ai/guides/ai-governance-coding-agents)

### Inferences
- **Merge Freeze is the most relevant comparable.** It has a flat org price ($25/$40), a Slack control surface, about 1.5k installs, and it does gating rather than a merge queue. It shows small teams will pay a flat monthly fee for "a human decides when code ships".
- **Merge queue unbundling is real but heavily contested.** It has venture-funded players, an acquisition (Graphite by Cursor), and GitHub's own stacked PRs, and it fits an approval-receipt product poorly.
- **Review-policy unbundling (PullApprove, Policy Bot) has been compressed** by GitHub's native required-reviewer rule (GA Feb 2026) and CODEOWNERS on Team.
- **Install counts are an upper bound on payers.** Most listings show a $0 plan on Marketplace and bill off-Marketplace, so paid conversion is unknown.

### Gaps
- Revenue, ARR and paying-customer counts for Mergify, Aviator, Merge Freeze and PullApprove were not found.
- Kodiak's paid price is unknown (its site was unreachable).
- Airlock pricing is unknown.

---

## Q4. GitLab: Free vs Premium ($29) vs Ultimate

### Takeaway
On GitLab.com the friction is shaped differently. Free private top-level namespaces are capped at **5 users**, so any team larger than 5 already pays Premium, which includes required approvals, protected environments, deployment approvals, code owners, push rules, merge trains and group audit events. The unbundle market is therefore (a) teams of up to 5 people on Free, or (b) Premium customers who want Ultimate-only items. Demand to move approvals to Free is near zero in GitLab's issue tracker. GitLab has used required approvals as a conversion experiment, exposing Free users to the feature to drive upgrades.

### Cited findings
- **Free user limit:** "You can add up to five users to newly created top-level namespaces with private visibility on GitLab.com". It has applied to older namespaces since 13 Jun 2023, and namespaces over the limit become read-only. — [free_user_limit.md](https://gitlab.com/gitlab-org/gitlab/-/raw/master/doc/user/free_user_limit.md)
- **Tier badges (GitLab docs source, 16 Oct 2026):**
  - Deployment approvals: Premium, Ultimate — [deployment_approvals.md](https://gitlab.com/gitlab-org/gitlab/-/raw/master/doc/ci/environments/deployment_approvals.md)
  - Protected environments: Premium, Ultimate — [protected_environments.md](https://gitlab.com/gitlab-org/gitlab/-/raw/master/doc/ci/environments/protected_environments.md)
  - "Protect manual jobs": Premium, Ultimate. Plain `when: manual` with `manual_confirmation` is Free. — [job_control.md](https://gitlab.com/gitlab-org/gitlab/-/raw/master/doc/ci/jobs/job_control.md)
  - MR approvals are Free but optional: "These approvals are optional and don't prevent merging without approval". Required approvals and approval rules are Premium, Ultimate. Security approvals are Ultimate. — [approvals/_index.md](https://gitlab.com/gitlab-org/gitlab/-/raw/master/doc/user/project/merge_requests/approvals/_index.md); [approvals/rules.md](https://gitlab.com/gitlab-org/gitlab/-/raw/master/doc/user/project/merge_requests/approvals/rules.md)
  - Code owners: Premium, Ultimate. Push rules: Premium, Ultimate. Merge trains: Premium, Ultimate. Compliance frameworks: Premium, Ultimate. — [codeowners](https://gitlab.com/gitlab-org/gitlab/-/raw/master/doc/user/project/codeowners/_index.md); [push_rules.md](https://gitlab.com/gitlab-org/gitlab/-/raw/master/doc/user/project/repository/push_rules.md); [merge_trains.md](https://gitlab.com/gitlab-org/gitlab/-/raw/master/doc/ci/pipelines/merge_trains.md); [compliance_frameworks](https://gitlab.com/gitlab-org/gitlab/-/raw/master/doc/user/compliance/compliance_frameworks/_index.md)
  - Audit events: project-level are Free and "retained indefinitely"; group audit events are Premium, Ultimate; audit event streaming is Ultimate. — [audit_events.md](https://gitlab.com/gitlab-org/gitlab/-/raw/master/doc/user/compliance/audit_events.md); [audit_event_streaming.md](https://gitlab.com/gitlab-org/gitlab/-/raw/master/doc/user/compliance/audit_event_streaming.md)
  - **External status checks** (GitLab's native hook for outside approval systems): **Ultimate**. MR approval policies: Ultimate. "Pipelines must succeed": Free, and it works with "external CI integration" pipelines. — [status_checks.md](https://gitlab.com/gitlab-org/gitlab/-/raw/master/doc/user/project/merge_requests/status_checks.md); [merge_request_approval_policies.md](https://gitlab.com/gitlab-org/gitlab/-/raw/master/doc/user/application_security/policies/merge_request_approval_policies.md); [auto_merge.md](https://gitlab.com/gitlab-org/gitlab/-/raw/master/doc/user/project/merge_requests/auto_merge.md)
- **Prices:** Premium is $29/user/mo billed annually ($348/user/yr). Ultimate is "contact sales", estimated at $60–99/user/mo **[snippet, aggregators]**. — [spendbase (snippet)](https://www.spendbase.co/blog/saas-management/gitlab-pricing/); [costbench (snippet)](https://costbench.com/software/developer-tools/gitlab)
- **Demand (GitLab issues API, upvotes):**
  - #288809 "gitlab.com: Move 'required approval' to the free tier": 0 upvotes, closed (2020).
  - #432865 "Move reset approvals on new commits to GitLab Free": 1 upvote, open (Nov 2023).
  - #27426 "Show 'approve' button on merge requests in Core": 123 upvotes, closed (2019).
  - Conversion experiments #224185 and #346176: "expose free users to required merge request approvals" (2020–2021).

  — [#288809](https://gitlab.com/gitlab-org/gitlab/-/work_items/288809); [#432865](https://gitlab.com/gitlab-org/gitlab/-/work_items/432865); [#27426](https://gitlab.com/gitlab-org/gitlab/-/work_items/27426); [#224185](https://gitlab.com/gitlab-org/gitlab/-/work_items/224185); [#346176](https://gitlab.com/gitlab-org/gitlab/-/work_items/346176)

### Feasibility on GitLab Free and Premium
- **Merge gating: feasible on Free.** Post a pending or failed external commit status and turn on "Pipelines must succeed".
- **Deploy approvals: feasible on Free if the gate holds the credential or trigger token.** Anyone with the Developer role can run unprotected manual jobs.
- **Premium to Ultimate items:** a third party can stand in for external status checks (via commit statuses) and for audit streaming (partially, via webhooks and the audit events API).

### Inferences
- **The GitLab friction is $29/user, but it bites mainly teams of 5 or fewer.** Five users on Premium cost $145/mo. The 5-user cap also pushes larger free teams to pay Premium anyway, which already bundles approvals.
- **GitLab is a "support it later" channel, not a launch market.** The one interesting wedge: Ultimate-only external status checks and approval policies are exactly where an approval gate plugs in, and Premium shops (not Ultimate) cannot use GitLab's native hook.

### Gaps
- I did not see GitLab's live pricing page or any 2026 price change.
- I did not research GitLab Duo Agent Platform governance tiers.
- I found no third-party GitLab approval apps with traction.

---

## Q5. The AI-agent angle: new paywalled needs?

### Takeaway
Agents create real new needs. GitHub has shipped native answers on all plans for Copilot, and on Team for actor restrictions. That leaves three Team-plan gaps:
1. **Human approval at deploy or secret-access time** for any agent-initiated run, because environment reviewers are Enterprise-only on private repos.
2. **Approve-then-run for agents other than Copilot.** GitHub's "Approve and run workflows" button covers Copilot (and fork PRs), not Claude, Codex or Devin bots; actor rules can only block or allow them.
3. **Exportable proof of which agent did what, and that a named human (not an AI) approved.** The audit API and streaming are Enterprise-only, and since 1 Sep 2026 Copilot can optionally count as a required approver.

### Cited findings

**Native mitigations (docs)**
- Copilot cloud agent:
  - Workflows on its PRs "are not triggered until Copilot cloud agent's code is reviewed and a user with write access … clicks the **Approve and run workflows** button". This can be disabled per repo.
  - The person who asked for the PR cannot approve it.
  - "Requires an additional approval when a pull request isn't attributed to a person." This is on by default in rulesets and always applies in branch protection.
  - Commits are signed, session logs exist, and audit log events are available to admins.

  — [risks-and-mitigations.md](https://raw.githubusercontent.com/github/docs/main/content/copilot/concepts/security-governance-and-network-settings/risks-and-mitigations.md); [configuring-agent-settings.md](https://raw.githubusercontent.com/github/docs/main/content/copilot/how-tos/use-copilot-agents/cloud-agent/configuring-agent-settings.md) ("Allowing GitHub Actions workflows to run without approval may allow unreviewed code written by Copilot to gain write access to your repository or access your GitHub Actions secrets.")
- Unattributed-Copilot-PR extra approval is in public preview, enabled for fpt and ghec (so Team rulesets get it). — [flag](https://raw.githubusercontent.com/github/docs/main/data/features/repo-rules-copilot-extra-approval.yml); [available-rules-for-rulesets.md](https://raw.githubusercontent.com/github/docs/main/content/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/available-rules-for-rulesets.md)
- **Copilot automations** run the agent on schedules or events "without a person initiating each task". Work is attributed to the automation's creator, and Actions workflows still need human approval. Automations require paid Copilot and private or internal repos. A new "Automation level" setting (Full control, Cautious, Balanced, Full automation) routes low-confidence issue changes to an approvals panel. — [copilot-automations gate](https://raw.githubusercontent.com/github/docs/main/data/reusables/gated-features/copilot-automations.md); [manage-rationale-confidence-approvals.md](https://raw.githubusercontent.com/github/docs/main/content/copilot/how-tos/use-copilot-agents/cloud-agent/manage-rationale-confidence-approvals.md)
- Third-party agents inside GitHub (Anthropic Claude and OpenAI Codex coding agents) are available on all paid Copilot plans, so they fall under Copilot governance. — [third-party-coding-agents.md](https://raw.githubusercontent.com/github/docs/main/data/reusables/gated-features/third-party-coding-agents.md); [anthropic-claude.md](https://raw.githubusercontent.com/github/docs/main/data/reusables/gated-features/anthropic-claude.md)
- **Copilot code review can approve PRs** (1 Sep 2026, public preview; off by default). "When Copilot approvals are enabled … Copilot can submit an approving review that satisfies your repository's required-approval rule the same way a teammate's approval would." — [code-review.md](https://raw.githubusercontent.com/github/docs/main/content/copilot/concepts/agent-capabilities/agent-powered-workflows-and-operations/code-review.md); [changelog (snippet)](https://github.blog/changelog/2026-09-01-copilot-code-review-can-now-approve-pull-requests)
- **Workflow execution protections** (Team, GA 17 Sep 2026): actor rules include Copilot and GitHub Apps. A default policy blocks `pull_request_target` on public repos, enforced from **2 Nov 2026**. — [about-actions-policies.md](https://raw.githubusercontent.com/github/docs/main/content/actions/concepts/about-actions-policies.md); [pull-request-target-policy.md](https://raw.githubusercontent.com/github/docs/main/data/reusables/actions/workflows/pull-request-target-policy.md)
- Agent-session events can be streamed only through enterprise audit-log streaming. — [monitor-agentic-activity.md](https://raw.githubusercontent.com/github/docs/main/content/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/monitor-agentic-activity.md)

**Incidents and pressure (2026), all [snippet]**
- **"Comment and Control"** (Apr 2026, Johns Hopkins). Claude Code Security Review, Gemini CLI Action and GitHub Copilot were induced to leak runner secrets. Bounties were $100 (Anthropic) and $500 (GitHub); no CVEs.
- **Claude Code Action permission bypass** (Jun 2026, RyotaK / GMO Flatt). The action trusted any GitHub App actor; CVSS 7.8, patched in 4 days.
- **Cline** (Feb 2026). A prompt-injected issue title against a Claude Code Action triage workflow stole an npm token, which was used to publish cline@2.3.0.
- **CSA research note** on agent CI/CD secrets (8 Aug 2026).

— [CSA research note](https://labs.cloudsecurityalliance.org/research/csa-research-note-claude-code-github-action-prompt-injection/); [CSA PDF](https://labs.cloudsecurityalliance.org/wp-content/uploads/2026/08/CSA_research_note_ai_coding_agent_cicd_secrets_20260808-csa-styled.pdf); [OODA Loop](https://oodaloop.com/briefs/technology/three-ai-coding-agents-leaked-secrets-through-a-single-prompt-injection-one-vendors-system-card-predicted-it/); [letsdatascience](https://letsdatascience.com/news/researchers-hijack-ai-coding-agents-steal-credentials-d508cbeb)

**Usage scale of agents in public repos [code search, 16 Oct 2026]**
- `anthropics/claude-code-action` in workflows: about **20,032** files
- `copilot-setup-steps.yml`: **5,216**
- `openai/codex-action`: **1,299**

**Community signals**
- #193727 "What security headaches has AI introduced… (2026 edition)": 13 votes, 36 comments, 24 Apr 2026.
- #190621 Actions 2026 security roadmap: 32 votes, 111 comments.
- #159749: 1,837 votes, from OSS maintainers wanting to block Copilot contributions.

— [#193727](https://github.com/orgs/community/discussions/193727); [#190621](https://github.com/orgs/community/discussions/190621)

**Practitioner guidance.** Developer Toolkit and OneUptime (Jan 2026) recommend environment approvals as the production gate for agent changes. The DEV article "Human approval before an AI agent deploys code" argues for gating the deploy call itself. All [snippet]. — [developertoolkit.ai](https://developertoolkit.ai/en/shared-workflows/ai-native-sdlc/deploy/); [oneuptime](https://oneuptime.com/blog/post/2026-01-30-approval-workflows/view); [dev.to](https://dev.to/sekeraradim/human-approval-before-an-ai-agent-deploys-code-5gkp)

### Inferences
- **The new agent need is "someone accountable said yes, at the moment of privilege", not "restrict the agent".** GitHub now sells restriction on Team. The accountable human approval at deploy time is still Enterprise-only on private repos, which matches the founder's gate exactly.
- **AI approvals counting toward required reviews (opt-in) will collide with SOC 2 and ISO change-management controls.** Those controls assume a human approver. A signed "named human approved this SHA and this deploy" receipt becomes evidence that auditors can rely on. This is an inference; no auditor statement was found.
- **Agent PR volume increases merge-queue and freeze demand.** That helps the merge-queue incumbents more than an approval product.

### Gaps
- I found no GitHub statement on plans to bring environment reviewers to Team.
- I found no survey quantifying Team-plan orgs running agents that touch deploy workflows.
- I did not check Devin's or Codex cloud's own approval controls.

---

## Q6. Size of the friction

### Takeaway
GitHub discloses no Team-org or Team-seat counts. Available anchors:
- 4M+ organizations;
- 225M+ developers;
- 800M+ repositories, about 37% private per Octoverse 2025;
- about 140k organizations using Copilot (snippet).

Public-repo proxies show deploy-from-Actions is common: 79,360 public workflow files say `environment: production`. They also show the approval workaround is a niche: 1,080 + 594 files. Any revenue estimate here is assumption-driven.

### Cited findings
- github.com/about (fetched 16 Oct 2026): "225M+ Developers", "4M+ Organizations", "800M+ Repositories", "90% Fortune 100". — [github.com/about](https://github.com/about)
- Octoverse 2025: 180M+ developers, about 630M repositories, "Public repositories make up 63% of all projects", 43.2M PR merges per month **[snippet]**. — [GitHub blog Octoverse 2025](https://github.blog/news-insights/octoverse/octoverse-a-new-developer-joins-github-every-second-as-ai-leads-typescript-to-1/); [itsfoss (snippet)](https://itsfoss.com/news/github-octoverse-2025/)
- Copilot figures **[snippet, secondary]**:
  - about 50M users (Microsoft FY26 Q4, Jul 2026);
  - 4.7M paid subscribers (Jan 2026);
  - "nearly 140,000 organizations" (FY26 Q3).

  An aggregator claims about 29K Enterprise customers (unsourced). — [tech-insider (snippet)](https://tech-insider.org/ie/github-copilot-signup-freeze-2026/); [secondtalent (snippet)](https://secondtalent.com/resources/github-copilot-statistics); [skillademia (snippet)](https://skillademia.com/statistics/github-statistics)
- Code-search proxies (public repos, 16 Oct 2026):

  | Pattern | Public files |
  |---|---|
  | `"environment: production"` in workflows | 79,360 |
  | `repository_dispatch` + `deploy` | 15,936 |
  | `trstringer/manual-approval` | 1,080 |
  | other `manual-approval` refs | 594 |
  | `merge_group:` | 36,736 |
  | `.mergify.yml` | 4,552 |
  | `.pullapprove.yml` | 2,040 |

### Inferences (assumption-driven)
- **Headroom per org.** The $17/user/mo Team-to-Enterprise gap means a 10-person team saves $170/mo ($2,040/yr) by not upgrading, and a 25-person team saves $425/mo. A $19–49/org/mo gate is 3–9 times cheaper than the upgrade.
- **Illustrative paying-customer range (assumptions, not data):**
  - Team-plan orgs: 200k–500k (unknown).
  - Share with private repos deploying from Actions to production: about 20–30%.
  - Share that want a gate and would pay a third party: 1–2%.
  - Result: roughly 400–3,000 paying orgs × $30/mo ≈ **$0.15M–1.1M ARR**.

  Marketplace ceilings are consistent with this: the most-installed gating apps have 1.5k–10.6k installs, and most of those are free or OSS.
- **Revenue estimate.** This is solo-founder lifestyle scale from GitHub alone. Venture scale needs the agent-approval and evidence story to pull in buyers beyond GitHub.

### Gaps
- Number of Team organizations and seats.
- Share of private repos using environments.
- Primary Microsoft earnings text.
- The Octoverse Actions-minutes figure (not found this session).

---

## Ranked unbundling opportunities

Scoring: H = high, M = medium, L = low. "Fit" means fit with approval gate + sealed receipt + policy engine. "Headroom" means price headroom against the native upgrade and existing alternatives.

| Rank | Opportunity | Demand signal | Feasibility (Team / Free) | Paid competition | Fit | Headroom | Verdict |
|---|---|---|---|---|---|---|---|
| **1** | **Deploy approvals for private repos**: named approver in Slack, Teams, phone or web; wait timers; break-glass; one-time, time-boxed credential via OIDC | **M**: a steady trickle of "missing on Team" threads (2023–2026, 0–4 votes each); 677-star workaround with 1,080 + 594 public workflow refs; "I upgraded for this" | **Feasible / Partial**. Team: dispatch-only deploy workflow + actor rule (GA 17 Sep 2026) + deployment-branch secrets. Free: the gate must broker credentials. | **None** in the $9–49/org band. Enterprise tools (Sleuth, Spacelift, Harness, Octopus) are $10k+/yr. | **H** | **H**: $17/user/mo upgrade avoided | **Build. This is the core.** |
| **2** | **Agent-initiated privileged-run approvals**: approve-then-run for non-Copilot agent bots; human sign-off before an agent-triggered deploy; "named human, not AI" approval receipt as a required check | **M and rising**: 2026 incidents (Cline, Comment and Control, Claude Action CVSS 7.8); 20k public workflows use claude-code-action; Copilot approvals can count as reviews from 1 Sep 2026 | **Partial to feasible / Partial**. Actor rules block agents and the gate re-dispatches. A human-attestation check is a required status check on Team. | Native GitHub for Copilot only. WorkOS Airlock (early access, no price). Endor Labs and CodeRabbit (enterprise or review-centric). | **H** | **M** | **Build as the positioning of #1**, not a separate product |
| **3** | **Sealed change and deploy ledger**, a stand-in for the missing audit API and streaming: every gate decision, merge, deploy and agent run as a signed receipt; exportable for SOC 2 | **L–M**: no Team-plan threads found; need is implied by SOC 2 (see sibling notes) | **Partial / Partial**. Webhooks cover much but not audit-only events; the gate is authoritative for what it mediates. | Vanta and Drata (indirect, $$); Astriguard (8 installs); nothing direct | **H** | **M**: Enterprise upgrade avoided, but buyers expect this inside compliance suites | **Add-on tier** for #1 (e.g. +$20–50/mo) |
| **4** | **Freeze windows and emergency-change approvals**: scheduled freezes, "approve to merge or deploy during freeze", sealed record of exceptions | **M**: Merge Freeze has 1,538 installs at a paid flat price | **Feasible / Impossible to enforce**. Required app check via org ruleset on Team. | Merge Freeze ($25/$40/mo); Mergify Merge Protections | **H** | **L–M**: $25–40 benchmark | **Include as a feature of #1**; it proves flat pricing works |
| 5 | **Central required checks and policy-as-code merge gate** (a stand-in for required workflows, metadata rules and Evaluate mode) | **L**: no threads found | **Feasible / Impossible**. Org ruleset requires the app's check. | Policy Bot and safe-settings (free, self-hosted); Mergify; CI vendors | **M** | **L** | Only as a policy-engine extension later |
| 6 | Path-based approver policies / CODEOWNERS-plus (PullApprove-style) | **L–M**: PullApprove 523 installs; 2,040 public configs | Feasible on Team; impossible on Free | PullApprove $4–6/reviewer; Policy Bot free; **native** required-reviewer rule GA Feb 2026 | **M** | **L**: native now | **Skip** |
| 7 | Merge queue for private repos on Team | **M–H** (installs) but weak vote signal (#201908: 11) | Feasible on Team | Mergify (10.6k installs), Aviator (3.9k), Trunk, Graphite/Cursor, Kodiak; GitHub may open it to Team | **L** | **L**: crowded | **Skip** |
| 8 | Commit-metadata rules | **L** | Feasible on Team | commitlint and similar (free) | **L** | **L** | Skip |
| 9 | Branch protection and CODEOWNERS on Free private repos | **M** (#174419: 102 votes) but buyers can't or won't pay (students) | **Impossible** to enforce | n/a (Team is $4) | **L** | **L** | Skip |
| 10 | IP allow list, SAML/SCIM, EMU, custom roles, native audit streaming | not measured | **Impossible** | n/a | **L** | n/a | Skip |
| 11 | Copilot agent governance (custom instructions, firewall, who can assign) | **L** as a paywall: tied to Copilot Business ($19), not to Enterprise | Mostly native | GitHub native | **L** | **L** | Skip (fold the agent-receipt piece into #2) |
| 12 | GitLab: approvals and protected environments for Free (≤5 users) or external-status-check-style gates for Premium shops | **L**: GitLab issues 0–1 upvotes; 5-user cap | Feasible on Free via "Pipelines must succeed" + external commit status + gate-held trigger token | GitLab Premium ($29) bundles it | **M** | **M** for ≤5-user teams ($145/mo Premium) | Later channel, not launch |

**Bottom line on the ranking.**
- **Only deploy approvals clearly clear every bar:** demand, enforceability on Team (newly robust since 17 Sep 2026), no paid competitor in the band, perfect fit, and high headroom.
- **Agent-initiated runs are the story, not a second product.** Use them to make #1 timely and defensible.
- **The ledger and freeze windows are upsell features that lift revenue per customer** (benchmark: Merge Freeze at $25–40/mo flat).
- **Everything else is native on Team, crowded, impossible, or tied to Copilot seats.**

**Main risk:** GitHub has moved at least four governance features down to Team since April 2025 (GHAS purchase, org rulesets, the required-reviewer rule, workflow execution protections; push rulesets also run on Team, date not checked). If environment reviewers follow, #1 loses its arbitrage. The product must therefore stand on what GitHub won't do natively: multi-channel named approvers, one-time grants, signed receipts, and cross-platform coverage (GitLab, Railway, Render and others).

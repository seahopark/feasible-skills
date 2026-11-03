---
name: repo-investigation
description: |
  Use when a user asks whether a feature is implemented or usable now in a named environment.
  Trace UI, client calls, API, persistence, branches, deployment, configuration, and access gates.
  Report implementation status separately from availability for a specific user, tenant, role,
  plan, or region. Use for multi-repository feature checks and endpoint reports.
---

# Repository Investigation

Answer the user's practical question first: **Can this user use the feature in the requested environment now?** Source code can establish implementation, but not deployment or access on its own.

## Workflow

1. **Set the question boundary.** Identify the expected user action, environment, date, user/role, tenant, plan, and region relevant to the question. Identify the source repositories and their production or staging branches; do not assume a branch name denotes an environment. If the environment is missing, report code status and label availability unverified. Done when the answer target is a specific user action and environment, or the missing context is explicit.
2. **Refresh source facts without disturbing working copies.** Check repository status, fetch relevant remotes when permitted, record remote tip SHAs, and inspect candidate branches and history. Use `git show`, read-only repository APIs, or an isolated temporary worktree; do not switch a user's active checkout. An absence conclusion requires a search across relevant repositories and branches. Done when each inspected source has a recorded revision and search scope.
3. **Trace the user path end to end.** Find the entry point and access gate, visible control and handler, client call, API route, domain rule, persistence or external integration, and contract. Note mock data, disabled controls, feature flags, and generated SDK calls without a consumer. Inspect migrations or schema and enforcement layers when the question is about a policy or constraint. Done when every required link is found or its absence is named.
4. **Establish environment availability.** Identify the revision deployed to each requested environment, its relevant flags/configuration, role/tenant/plan/region gates, required integrations and data, and an accessible runtime observation. A commit reachable from `main` is code ancestry, not deployment evidence. Do not perform a mutating action solely to test availability without authorization. Done when each availability condition has evidence or is marked unknown.
5. **Give two verdicts.** First classify implementation: **not found in searched scope**, **disabled/gated**, **mock/local only**, **API only**, or **end-to-end connected**. Then classify availability for each named environment: **available now**, **conditional**, **not available**, or **unverified**. Use the [decision table](#availability-decision-table). Done when the user can see both verdicts and the evidence separating them.
6. **Report and verify.** Lead with one sentence answering whether the user can use the feature. Give a concise evidence chain, exact environment and access conditions, the smallest missing check, and an endpoint table only if requested. Verify file/line, SHA, configuration, and runtime citations against their original sources. Remove temporary worktrees. Done when the report contains no unsupported claim of production use and leaves no temporary checkout behind.

## Availability decision table

| Verdict | Use when | User-facing wording |
|---|---|---|
| Available now | Required code is deployed, the requested user's gates and dependencies are satisfied, and the user path is supported by runtime or equivalent operational evidence | “You can use it in [environment] as [role] through [entry point].” |
| Conditional | The path exists, but a named condition such as enablement, role, plan, or integration must be met; distinguish a condition already met for some users from a change still required | “It is usable only for [eligible context] / after [specific condition]. It is not available to the requested user until then.” |
| Not available | A required link is absent or explicitly disabled in that environment | “It is not available in [environment] because [specific missing link or gate].” |
| Unverified | Deployment, flags, access, dependency, or runtime facts needed for a definite verdict could not be checked | “Code is [implementation status], but availability in [environment] is unverified. Check [specific fact].” |

Do not convert an unknown gate into a conditional verdict. “Conditional” requires an identified condition; “unverified” means a necessary fact is missing.

## Report shape

1. **Can the user use it?** Environment, user context, availability verdict, and direct reason.
2. **What is implemented?** Entry point → client → API → domain/persistence, with repository and revision evidence; implementation verdict.
3. **What controls availability?** Deployment revision, flags, roles/tenant/plan/region, dependencies, and runtime evidence; checked vs unknown.
4. **What remains to verify?** One concrete next check, not a speculative root cause.
5. **Endpoints, if requested:** method, path, authentication, parameters, request/response, errors, and actual consumer.

Example: An API route exists on a development branch, but production's deployed revision and flag state are unknown. Report **API only or end-to-end connected on the inspected branch**, as evidence permits, and **production availability unverified**. Do not call it shipped.

## Pitfalls

- A generated client method is not proof of a connected UI.
- A visible control may use local mock data or fail behind a gate.
- Branch ancestry, deployment, enabled configuration, and a successful user path are separate facts.
- Tests read from one revision cannot establish behavior on another revision.
- An endpoint can be live but unavailable to the requested role, tenant, plan, or region.

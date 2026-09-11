# feasible-skills

PM/QA workflows I've built and used while working on [Feasible Research](https://research.feasible.kr), packaged as [Claude Code / OpenClaw Skills](https://code.claude.com/docs/en/skills). Anything tied to a specific client or project has been generalized out — what's here is meant to be dropped into other products and teams as-is.

## Skills

| Skill | What it does | When to use it |
|---|---|---|
| [`user-story-mapping`](skills/user-story-mapping/SKILL.md) | Builds a user story map around the user journey (Jeff Patton framework) | When you need to organize a backlog/MVP scope around user workflows |
| [`product-spec`](skills/product-spec/SKILL.md) | Reverse-engineers a product from screenshots/usage into a Business Discovery doc + a Product Spec doc | When an existing product's feature definitions aren't documented anywhere |
| [`feasible-lab-prd`](skills/feasible-lab-prd/SKILL.md) | Generates an Acceptance-Criteria-centric PRD draft | When you need to quickly draft a Markdown PRD for a new feature/project |
| [`functional-tc-authoring`](skills/functional-tc-authoring/SKILL.md) | Writes manual-QA functional test-case sheets grounded in the actual frontend code | When you need TCs pulled accurately from the code, not guessed from the UI |
| [`browser-agent-smoketest`](skills/browser-agent-smoketest/SKILL.md) | Writes a full-cycle smoke test as a prompt an AI browser agent (e.g. Claude in Chrome) can execute on its own | When you need a quick "is the whole service still working" pass after a deploy, run by an AI instead of a human |
| [`policy-doc`](skills/policy-doc/SKILL.md) | Generates and maintains service policy docs (PRD/TC → policy.md), verified against code | When planning and actual implementation have drifted and you need to reconcile the policy doc |
| [`ai-usage-retro`](skills/ai-usage-retro/SKILL.md) | Evaluates your actual AI usage from real artifacts (not self-report) and turns it into an improvement loop matched against new capabilities | On a recurring cadence, to check whether your AI workflow is keeping up with what's now possible |
| [`idea-prioritization`](skills/idea-prioritization/SKILL.md) | Filters ideas through a 4-axis qualitative check, then scores survivors with ICE/RICE | When you have several candidate ideas/backlog items and need to decide what to do first |
| [`org-diagnosis-pivot`](skills/org-diagnosis-pivot/SKILL.md) | Two-phase: a 40-question diagnostic bank to turn a vague complaint into facts, then a pivot-decision frame to judge direction | Evaluating a company to join, clarifying a vague in-tenure complaint, or deciding whether a stalled product should pivot |

Each skill's `SKILL.md` has the full procedure; some also ship templates under `references/`.

## Usage

If you use Claude Code or OpenClaw, copy the skill folder you want straight into your workspace's skills directory.

```bash
git clone https://github.com/seahopark/feasible-skills.git
cp -r feasible-skills/skills/product-spec ~/your-workspace/skills/
```

## License

MIT — use it, fork it, adapt it.

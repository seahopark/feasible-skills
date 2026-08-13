# feasible-skills

PM/QA workflows I've built and used while working on [Feasible Research](https://research.feasible.kr), packaged as [Claude Code / OpenClaw Skills](https://code.claude.com/docs/en/skills). Anything tied to a specific client or project has been generalized out — what's here is meant to be dropped into other products and teams as-is.

## Skills

| Skill | What it does | When to use it |
|---|---|---|
| [`user-story-mapping`](skills/user-story-mapping/SKILL.md) | Builds a user story map around the user journey (Jeff Patton framework) | When you need to organize a backlog/MVP scope around user workflows |
| [`product-spec`](skills/product-spec/SKILL.md) | Reverse-engineers a product from screenshots/usage into a Business Discovery doc + a Product Spec doc | When an existing product's feature definitions aren't documented anywhere |
| [`feasible-lab-prd`](skills/feasible-lab-prd/SKILL.md) | Generates an Acceptance-Criteria-centric PRD draft | When you need to quickly draft a Markdown PRD for a new feature/project |
| [`functional-tc-authoring`](skills/functional-tc-authoring/SKILL.md) | Writes manual-QA functional test-case sheets grounded in the actual frontend code | When you need TCs pulled accurately from the code, not guessed from the UI |
| [`policy-doc`](skills/policy-doc/SKILL.md) | Generates and maintains service policy docs (PRD/TC → policy.md), verified against code | When planning and actual implementation have drifted and you need to reconcile the policy doc |

Each skill's `SKILL.md` has the full procedure; some also ship templates under `references/`.

## Usage

If you use Claude Code or OpenClaw, copy the skill folder you want straight into your workspace's skills directory.

```bash
git clone https://github.com/seahopark/feasible-skills.git
cp -r feasible-skills/skills/product-spec ~/your-workspace/skills/
```

## License

MIT — use it, fork it, adapt it.

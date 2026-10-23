# performance-review-project

## Goal

Creates a project directory where an AI agent can repeatedly connect work records to hypotheses,
release dates, outcome metrics, and evidence. The project files are agent-agnostic; Claude Code is
used as the concrete example, but the same workspace can be opened by Codex, OpenClaw, or another
agent.

## When to use

- You want to explain what a project changed, not only what the team did.
- Work records and outcome metrics live in different tools.
- You want a repeatable weekly or monthly review instead of a review-season data hunt.
- You want to start with one project directory and add MCP or automation after the first manual run.

## Files

- [`SKILL.md`](SKILL.md): the workflow and evidence rules
- [`references/project-files.md`](references/project-files.md): starter files for one project
- [`references/prompts.md`](references/prompts.md): reusable first-run, weekly, and retrospective prompts

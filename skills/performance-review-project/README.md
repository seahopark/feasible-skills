# performance-review-project

## Goal

Creates a project directory where an AI agent can independently review work records or analytics,
then optionally compare them using explicit project mappings. The project files are agent-agnostic;
Claude Code is used as the concrete example, but the same workspace can be opened by Codex,
OpenClaw, or another agent.

## When to use

- You want to explain what a project changed, not only what the team did.
- Work records and outcome metrics live in different tools.
- You want to review only the work records or only the analytics when the other source is not ready.
- You want a repeatable weekly or monthly review instead of a review-season data hunt.
- You want to start with one project directory and add MCP or automation after the first manual run.

## Files

- [`SKILL.md`](SKILL.md): the workflow and evidence rules
- [`references/project-files.md`](references/project-files.md): starter files for one project
- [`references/prompts.md`](references/prompts.md): reusable first-run, weekly, and retrospective prompts
- [`references/ga4-analysis.md`](references/ga4-analysis.md): event-based analytics review rules
- [`references/linear-review.md`](references/linear-review.md): work-record review rules
- [`references/source-join.md`](references/source-join.md): optional mapping when both sources are used

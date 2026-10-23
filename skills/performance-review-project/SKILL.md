---
name: performance-review-project
description: |
  Creates and runs a project-level performance review workspace that connects work records,
  hypotheses, releases, metrics, and evidence into a repeatable review loop. Use when someone
  wants to track what a project changed, verify its results, prepare a review, or set up a
  weekly/monthly performance check across an AI agent. The workflow is agent-agnostic and can
  use MCP-connected sources, local files, exports, or manual inputs; it is not for monitoring
  service health or reviewing AI-tool usage itself.
---

# Project Performance Review

Create one small workspace per project so that the project’s hypothesis, work record, metric
definitions, and review history stay together. The workspace is the durable artifact. The agent
is interchangeable: Claude Code, Codex, OpenClaw, or another agent can read the same files and
follow the same prompts.

## Start here

When the user asks to start this workflow, do not begin by collecting every available metric.
First identify:

1. The project and the decision it is meant to support.
2. The problem, hypothesis, and expected change.
3. The release or change date, if there is one.
4. The two or three metrics that would show whether the expected change occurred.
5. The sources available for work records and metrics.

If the user has not chosen a directory, recommend a new project directory and offer to initialize
it. Do not overwrite an existing directory without inspecting it and getting confirmation.

## Project workspace

Initialize this shape, adapting names to the project:

```text
project-performance-review/
├── README.md
├── PROJECT.md
├── metrics.md
├── sources.md
├── prompts/
│   ├── weekly-review.md
│   └── project-retro.md
└── reports/
```

Use the templates in:

- [references/project-files.md](references/project-files.md) when creating or repairing the files
- [references/prompts.md](references/prompts.md) when writing the review prompts

Keep credentials, tokens, raw customer-level data, and generated secrets out of the workspace.
Record only the MCP/source name, the permitted operation, the query or filter needed to reproduce
the result, and the location of the original data.

## Source and MCP setup

MCP is an optional transport layer, not the method itself. The workflow can read from an MCP
server, a local export, an API wrapper, or a manually supplied report. When MCP is available:

1. Connect the work-record source first. Identify projects, hypothesis notes, work dates, and
   release dates.
2. Connect the analytics source second. Identify only the aggregate metrics needed by
   `metrics.md`.
3. Prefer read-only access. State what is read and what is not read before querying.
4. Run a small test query from each source and save the returned metric name, time window, filter,
   and retrieval date in the report.
5. If the sources cannot be joined reliably, stop at a partial review and list the missing join
   key or event. Do not invent a project-to-metric relationship.

Never treat a tool being connected as evidence that the data is correct. Check the source,
definition, period, filters, aggregation, and timezone for every material number.

## Running the workflow

### First run

Run interactively in the agent environment that the user already uses. The first run is for
calibrating the project definition, not automation. Read `PROJECT.md`, `metrics.md`, and
`sources.md`; ask only for missing facts that change the interpretation; then write the first
report under `reports/`.

### Weekly or monthly review

Use the same prompt and metric definitions each time. The default cadence is weekly when the
project changes quickly and monthly when the signal is slow. Each report should include:

- what changed in the work record;
- the agreed metrics, periods, filters, and values;
- comparison with the previous equivalent period;
- evidence that supports the observation;
- unknowns, data gaps, and competing explanations;
- a small number of decisions or next checks.

Do not rewrite the project hypothesis merely because a metric moved. Record a changed hypothesis
as a project decision with its date and reason.

### Automation

Automation is optional and comes after at least one or two successful interactive runs. A Claude
Code project, Codex workspace, or agent skill supplies instructions and files; it does not by
itself schedule execution. Use the host’s scheduler, CI, OpenClaw automation, or another explicit
runner for recurring execution. The runner should invoke the agent with the project directory
and prompt, and should write a report or an error log. Keep the first scheduled version read-only
and silent when no meaningful change exists.

For a concrete Claude Code setup, open the project directory in Claude Code and place the workflow
instructions in `CLAUDE.md` or the equivalent project instruction file. The same `PROJECT.md`,
`metrics.md`, and prompts remain usable from Codex or another agent; only the invocation file and
runner command change.

## Evidence and interpretation rules

- Separate an observation (a measured change) from an explanation (why it may have happened).
- A post-release increase is not proof of causation. Check campaigns, seasonality, traffic mix,
  pricing, incidents, and other concurrent changes when relevant.
- Compare equivalent windows. State whether the comparison is pre/post, week-over-week,
  month-over-month, or cohort-based.
- Include denominator, population, timezone, and filters for rates and counts.
- If a number cannot be reproduced from the stated source and conditions, label it unverified.
- Never expose individual or customer-identifying data in the report unless the user explicitly
  needs it and the source policy permits it.
- When data is unavailable, say what is missing and what can still be concluded. Do not fill gaps
  with plausible estimates.

## Output

Write a concise report to `reports/YYYY-MM-DD.md` or the project’s existing naming convention.
Keep the report useful to a decision-maker: conclusion first, then the evidence and caveats. Do
not claim success merely because activity was recorded. The report should make clear what changed,
what did not, and what remains unknown.

## Relationship to nearby skills

- Use `ai-usage-retro` when the subject is how a person uses AI tools and agents.
- Use `service-delta-review` when the subject is daily read-only monitoring of newly arrived
  service data and alerting on anomalies.
- Use this skill when the subject is a project’s hypothesis, work, outcome metrics, and review
  history. These skills can be used together, but one should remain the primary workflow.

---
name: performance-review-project
description: |
  Creates and runs a project-level outcome review workspace for hypotheses, work records,
  releases, metrics, and evidence. Use when someone wants to understand what a project changed,
  inspect work records or analytics independently, optionally compare the two, or prepare a
  repeatable review. It is agent-agnostic and can use MCP-connected sources, local files,
  exports, or manual inputs; it is not for personal HR evaluation, service-health monitoring,
  or reviewing AI-tool usage itself.
---

# Project Performance Review

Create one small workspace per project so that the project’s hypothesis, work record, metric
definitions, and review history stay together. A work-record review and an analytics review can
run independently; a cross-source comparison is optional and requires an explicit mapping. The
workspace is the durable artifact. The agent is interchangeable: Claude Code, Codex, OpenClaw,
or another agent can read the same files and follow the same prompts.

## Start here

The central workflow is hypothesis → success behavior → suitable events → repeatable analysis
view → results and learning. Independent use means independent sources, not analysis without a
hypothesis. Keep the workflow small; ask only questions needed for this loop.

First choose Linear/work records, GA/analytics, or both. Read the selected source's reference.
Find the hypothesis in the existing project page or notes before asking the user. If absent,
ask what change is expected and why; confirm it before interpreting success. Never invent a
historical hypothesis or present a newly agreed hypothesis as a past decision.

For Linear, the project page covers the problem, hypothesis, execution, release, and role.
For GA, ask what customer behavior represents success and which events could indicate it.
Recheck event meaning and actual collection, then build a reusable analysis view in `metrics.md`.
GA needs no Linear mapping to do this. A work-record-only review needs no GA metrics to proceed.

When the user asks to start this workflow, do not begin by collecting every available record or metric.
First identify:

1. The project and the decision it is meant to support.
2. The problem, hypothesis, and expected change.
3. The release or change date, if there is one.
4. For analytics: the success behavior, candidate events, and the few metrics needed to test the
   hypothesis. For work-record-only review, do not require analytics definitions.
5. Which review mode is needed:
   - **Work-record review:** inspect Linear, another task system, local files, or manual notes.
   - **Analytics review:** inspect GA4, another analytics source, exports, or manual reports.
   - **Combined review:** run both reviews and compare them only when a project/change mapping is
     available.
6. The sources available for the selected mode.

If the user has not chosen a directory, recommend a new project directory and offer to initialize
it. Do not overwrite an existing directory without inspecting it and getting confirmation.

## Project workspace

Initialize the smallest useful shape, adapting names to the project:

```text
project-performance-review/
├── PROJECT.md
├── metrics.md
├── sources.md
└── reports/
```

Create `metrics.md` when analytics is in scope. README and saved prompt files are optional.
On subsequent runs read only files relevant to the selected mode; missing analytics files must
not block a Linear-only review.

Use the templates in:

- [references/project-files.md](references/project-files.md) when creating or repairing the files
- [references/prompts.md](references/prompts.md) when writing the review prompts
- [references/ga4-analysis.md](references/ga4-analysis.md) when the analytics source is GA4 or a
  similar event-based product analytics system
- [references/linear-review.md](references/linear-review.md) when the work-record source is Linear
  or a similar issue/project tracker
- [references/source-join.md](references/source-join.md) only when the user wants to compare work
  records with analytics

Keep credentials, tokens, raw customer-level data, and generated secrets out of the workspace.
Record only the MCP/source name, the permitted operation, the query or filter needed to reproduce
the result, and the location of the original data.

## Source and MCP setup

MCP is an optional transport layer, not the method itself. The workflow can read from an MCP
server, a local export, an API wrapper, or a manually supplied report. Configure only the source
needed for the selected review mode. A combined review is not required for either source to be
useful.

For any source:

1. Prefer read-only access and state what is read and what is not read before querying.
2. Run a small test query or inspect a small sample before a full review.
3. Save the source name, query or filter, time window, retrieval date, and result status in the
   report.
4. Distinguish tool access failure, invalid query, no data, and an event or field that was never
   collected. Do not record all of these as “0”.

For a combined review, identify the project/change mapping before comparing sources. If the
mapping is missing or ambiguous, keep the two sections independent and list the missing join key
or event. Do not invent a project-to-metric relationship.

Never treat a tool being connected as evidence that the data is correct. Check the source,
definition, period, filters, aggregation, and timezone for every material number.

## Running the workflow

### First run

Run interactively in the agent environment that the user already uses. The first run is for
calibrating the project definition, not automation. Read `PROJECT.md`, `metrics.md`, and
`sources.md`; run the selected independent review first; ask only for missing facts that change
the interpretation; then write the first report under `reports/`. If both sources are available,
do not compare them until each source section is independently understandable.

### Weekly or monthly review

Use the same prompt and metric definitions each time. The default cadence is weekly when the
project changes quickly and monthly when the signal is slow. Include the following where relevant
to the selected mode; do not require unavailable sources:

- what changed in the work record;
- the agreed metrics, periods, filters, and values;
- comparison with the previous equivalent period;
- evidence that supports the observation;
- unknowns, data gaps, and competing explanations;
- customer or user learning, including failed or incomplete attempts;
- a small number of decisions or next checks.

Do not rewrite the project hypothesis merely because a metric moved. Record a changed hypothesis
as a project decision with its date and reason.

### Automation

Automation is optional and comes after at least one or two successful interactive runs. A Claude
Code project, Codex workspace, or agent skill supplies instructions and files; it does not by
itself schedule execution. Use the host’s scheduler, CI, OpenClaw automation, or another explicit
runner for recurring execution. The runner should invoke the agent with the project directory
and prompt, and should write a report or an error log. Keep the first scheduled version read-only.
Do not treat a query error as “no meaningful change”; write an error status or notify the owner.
Silence is appropriate only after the source was successfully checked and the result is genuinely
unchanged.

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
- Separate work completed, product change, observed outcome, interpretation, and learning.
- Treat a work item’s completion date as distinct from the customer-facing release date unless
  the source explicitly establishes they are the same.
- If a number cannot be reproduced from the stated source and conditions, label it unverified.
- Use aggregate data by default. Do not expose individual or customer-identifying data; if a
  user says it is necessary, confirm the permitted data boundary before retrieving it.
- When data is unavailable, say what is missing and what can still be concluded. Do not fill gaps
  with plausible estimates.

## Output

Write a concise report to `reports/YYYY-MM-DD-mode.md` or the project’s existing naming convention.
Preserve earlier reports when rerunning. Close with what was expected, what was achieved, what
failed or remains unknown, what was learned, and the next hypothesis or check. Ground learning in
the records, not generic lessons or inferred customer motives.
Keep the report useful to a decision-maker: conclusion first, then the evidence and caveats. Do
not claim success merely because activity was recorded. The report should make clear what changed,
what did not, and what remains unknown.

## Relationship to nearby skills

- Use `ai-usage-retro` when the subject is how a person uses AI tools and agents.
- Use `service-delta-review` when the subject is daily read-only monitoring of newly arrived
  service data and alerting on anomalies.
- Use this skill when the subject is a project’s hypothesis, work, outcome metrics, and review
  history. These skills can be used together, but one should remain the primary workflow.

# Project starter files

Use these as starting points. Replace bracketed values; do not leave assumptions that have not
been confirmed by the project owner.

## README.md (optional)

```markdown
# [Project name] — Performance Review

This directory keeps the project's hypothesis, work evidence, metric definitions, and review
reports together.

## Start a review

Read `PROJECT.md` and `sources.md`, plus `metrics.md` for analytics. Use the relevant skill prompt
or a saved prompt in `prompts/` if that optional folder exists.
Write the result to `reports/`.

## Review cadence

- Cadence: [weekly/monthly]
- Preferred day and timezone: [value]
- Primary agent: [Claude Code/Codex/OpenClaw/other]
```

## PROJECT.md

```markdown
# Project definition

- Project: [name]
- Owner: [name or role]
- Review period: [dates]
- Problem: [what problem this project addresses]
- Hypothesis: [if we do X, Y will change because Z]
- Hypothesis source: [project page / user confirmation; original or retrospective]
- Expected change: [observable behavior or business outcome]
- Change/release date: [date or unknown]
- Relevant work sources: [tools, documents, exports]
- Relevant metric sources: [tools, documents, exports]
- Known competing changes: [campaigns, pricing, seasonality, incidents, or unknown]
- Review mode: [work-record / analytics / combined]

## Decisions

| Date | Decision | Reason | Evidence |
| --- | --- | --- | --- |
```

## metrics.md (analytics only)

```markdown
# Metric definitions

## Repeatable analysis view

- Hypothesis: [confirmed hypothesis]
- Success behavior: [customer behavior that represents success]
- Selected events and rationale: [why each event indicates success or a step toward it]
- Event recheck: [meaning and collection checked; gaps or proxy limitations]
- View: [trend / step comparison / breakdown]
- Reproducible query or instructions: [request or retrieval steps]
- Saved GA exploration: [link if actually created; otherwise not created]

| Metric | Why it matters | Definition | Population / denominator | Comparison window | Source | Owner |
| --- | --- | --- | --- | --- | --- | --- |

## Retrieval rules

- Timezone: [timezone]
- Include/exclude: [filters]
- Aggregation: [count, average, rate, cohort, etc.]
- Equivalent comparison: [pre/post, WoW, MoM, cohort]
- Known instrumentation gaps: [gaps]
- Retrieval status convention: [access failure / query failure / no data / not instrumented / observed zero]
```

## sources.md

```markdown
# Data sources

| Source | Contains | Access | Read/write | Join key | Reproduction note |
| --- | --- | --- | --- | --- | --- |

## Safety and privacy

- Read-only sources: [list]
- Sensitive fields excluded: [list]
- Approved data boundary: [description]

## Optional project-to-metric mapping

Use only when a combined review is needed. If the sources cannot be joined reliably, leave this
section empty and report the sources independently.

| Work-record project | Customer-facing change date | Analytics event or metric | Expected change | Evidence |
| --- | --- | --- | --- | --- |
```

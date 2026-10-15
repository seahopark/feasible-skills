---
name: linear-sheets-gantt
description: |
  Use when someone wants a date-based timeline (Gantt) of Linear issues and Linear's built-in
  views are not enough. Explains the workaround of keeping Linear as the source of truth and
  rendering a per-project Google Sheets Gantt on a schedule, states its limits up front, and gives
  the build procedure when the user asks for it.

  Use this when:
  - Asked how to see when each issue in a Linear project starts and ends on a timeline
  - Asked to visualize schedule or workload in a spreadsheet Gantt kept in sync with Linear
  - Asked to refresh an existing Linear-to-Sheets Gantt ("refresh the Gantt")
---

# Linear Schedule Visualization: Google Sheets Gantt Workaround

For people who want to visualize work, especially schedules. Linear is good at issue management, but it is limited when you want to see, on a date axis, when each issue of one project starts and ends. A workaround is to keep Linear as the source and have a Google Spreadsheet draw the Gantt automatically. This skill explains that option and builds it if asked.

Guidance comes first. Do not create sheets or automations until the user asks to build.

## Guidance order

1. Ask what the user wants to see first: issue-level schedule, project or initiative roadmap, or workload per assignee. If it is roadmap level, Linear's built-in timeline may be enough.
2. Do not assert what Linear supports today. Check Linear's official documentation first; features change.
3. If an issue-level Gantt is needed and the built-in views fall short, explain the approach below.
4. Tell the user the limits (next section) before building anything.
5. If they want it built, follow "Building it".

## How it works

- One project = one spreadsheet. Rows are issues, columns are days.
- Linear is the source of truth and the sheet is a view. Change issues and dates in Linear only; do not edit work data in the sheet.
- Two tabs: `Gantt` (the timeline users read) and `Linear Raw` (fetched Linear data and the last sync time).
- Sub-issues appear indented under their parent, groups are sorted by start date, and bars are colored by status.
- A bar is drawn only when both a start date and a due date exist. Never estimate missing dates.
- A deterministic script refreshes it on a schedule (for example twice a day), stays silent on success, and alerts only on failure. A manual refresh is one sentence: "refresh the Gantt".

## Limits to state first

- Linear issues have no planned start date. What you can use is the actual start time (`startedAt`) and the due date (`dueDate`). Issues not yet started have no start date, so they get no bar. In the real case, most issues were in that state and the sheet looked mostly empty.
- Decide with the user how to handle it: (A) keep the rule as is and show a marker on the due date for issues that only have a due date (recommended); (B) estimate start dates and mark them as estimates by color or pattern; (C) define a planned start date as a custom field or label convention the team maintains.
- It refreshes on a schedule, not in real time. The sheet is read-only in practice: editing dates there does not change Linear.
- The organization's data passes through the sheet and the automation account. Where it is stored and who can access it is the user's call.

## Building it

Collect first:
- The target Linear project and read access (API key or MCP connection).
- A new Google spreadsheet, shared as Editor with the Google account used for writing. If the project already links a sheet, do not overwrite it.
- The choice among A, B, or C above.
- The refresh schedule and time zone.

Steps:
1. Confirm the Google Cloud project has the Sheets API (`sheets.googleapis.com`) and Drive API (`drive.googleapis.com`) enabled. The similarly named "Sheets MCP API" and "Drive MCP API" are not substitutes.
2. Authenticate with OAuth. If organization policy blocks service-account keys, do not work around the policy; use OAuth.
3. Create the `Gantt` and `Linear Raw` tabs and apply the date range and formatting.
4. Read the issues, rebuild parent-child links, sort by group, replace `Linear Raw`, then regenerate the `Gantt` date axis and bar formulas. Status colors: In Progress blue, Done green, Todo gray, Canceled light gray.
5. Write the last successful sync time (in the user's time zone) to `Linear Raw`, for example label in Q1 and value in Q2.
6. Read the sheet back to verify: issue count equals `Linear Raw` row count, sub-issues sit under the right parent, and bars appear only on issues with the required dates.
7. Register the refresh as a deterministic script, not an LLM-driven job. State the time zone in the cron expression (if the server runs in UTC, convert carefully).

Linear fields to fetch: id, identifier, title, parent, startedAt, dueDate, completedAt, state, assignee, team, priorityLabel, url, createdAt, updatedAt.

Fixed columns on `Gantt`: issue type, identifier, title, assignee, status, actual start, due date, link; then one column per day.

## Notes

- The original case was built with a Hermes agent; its sync script is not included here. Write one from the design above.
- When handing the build to another agent, restate this design in a prompt and pass the Linear project URL, the Google Sheet URL, and the desired refresh time.

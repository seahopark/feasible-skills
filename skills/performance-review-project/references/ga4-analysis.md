# Analytics review

Use this reference when the selected source is GA4 or another event-based product analytics
system. It supports an independent analytics review; Linear or another work-record source is not
required.

## Review sequence

Before querying, read the recorded hypothesis or confirm one with the user. Ask:

- What customer behavior would represent success for this product or hypothesis?
- Which event could show that behavior, and why is it more useful than a general visit or click?
- Is it the success itself or only a step that might lead to success?

Do not select events merely because they are available or have increased. Check their actual
meaning, parameters, and collection against the intended behavior. If the event is unsuitable,
propose an explicitly labeled proxy or record a measurement gap. Do not claim an event causes
success merely because they occur together.

1. Confirm the property, timezone, date range, population, and internal-traffic or other filters.
2. Confirm that the metric and event definitions in `metrics.md` match the source.
3. Check that the events and parameters exist in the selected period. Do not infer collection
   from application code alone when the analytics system or tag manager is the source of truth.
4. For a funnel, record the ordered events, the denominator for every conversion rate, and
   whether the count is events, sessions, or users.
5. Run a small query first, then the full query.
6. Save the query or a reproducible description, retrieval date, and result status in the report.

## Result status

Use separate labels:

- **access failure:** the agent or credential could not reach the source;
- **query failure:** the source was reachable but the request was invalid or failed;
- **no data:** the query was valid and returned no records;
- **not instrumented:** the event or parameter was not available in the source;
- **observed zero:** the valid query returned a measured zero;
- **unverified:** the value cannot be reproduced from the recorded conditions.

Do not interpret a zero attributed source or campaign as proof that there was no traffic. Check
attribution gaps, direct traffic, in-app browsers, and an independent source when the conclusion
matters.

## Minimum metric record

```text
Metric or event:
Meaning:
Population / denominator:
Date range:
Timezone:
Dimensions and filters:
Aggregation: event count / sessions / users / rate / cohort
Source and property:
Retrieved at:
Status:
```

For a funnel, add the previous and next step and the expected change from the project hypothesis.

## Reusable analysis view

GA can be used independently to create a view for this hypothesis. Save the definition in
`metrics.md`: hypothesis, success behavior, selected events and why they fit, core metrics,
period, comparison, filters, and a reproducible query or query instructions. Choose a simple trend,
step comparison, or breakdown that answers the question; do not build a dashboard by default.

Run it once and show the resulting table or chart. Recheck with the user whether it actually
answers the hypothesis, then reuse the definition for later reviews. Changing the event selection
requires recording why; it must not silently break comparisons.

A saved analysis definition and report are not a saved GA interface exploration. If the user
wants a view inside GA, use an available authorized UI capability and verify it was saved; if the
current tools only query data, provide the exact exploration setup and state that GA-side creation
has not been completed. Do not claim to have created a GA view from an API report alone.

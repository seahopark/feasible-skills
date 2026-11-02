# Optional source mapping

Use this reference only when the user wants to compare a work-record review with an analytics
review. Do not require this file for either independent workflow.

## Mapping rule

Map the sources using an explicit project or change identifier where possible. If no shared key
exists, use a manually confirmed mapping that records why the two records are being compared.
Date overlap alone is a candidate for investigation, not proof of a relationship.

## Mapping table

| Field | Value |
| --- | --- |
| Work-record project / link |  |
| Analytics property / report |  |
| Problem and hypothesis |  |
| Product change |  |
| Customer-facing release date and timezone |  |
| GA event or metric |  |
| Expected change |  |
| Comparison method and windows |  |
| Observed change |  |
| Concurrent changes |  |
| Evidence links or query |  |
| Interpretation level | observation / possible explanation / unverified |
| Missing join key or event |  |

## Interpretation

Keep these statements separate:

- “The metric increased after the release” is an observation.
- “The release caused the increase” is a causal claim.
- “The release may have contributed” is an interpretation that still needs competing changes
  and evidence.

When the mapping cannot be established, report both source reviews independently and state what
additional project identifier, release record, event, or comparison would be needed.

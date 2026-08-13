# Guide: writing a policy doc from a TC spreadsheet

## Analysis order

### Step 1: Map out the sheet list
```python
from openpyxl import load_workbook
wb = load_workbook("tc-file.xlsx", read_only=True)
print(wb.sheetnames)  # check the per-domain sheets
```

### Step 2: Extract TC data
```python
ws = wb['{sheet name}']
tcs = []
for row in ws.iter_rows(min_row=14, values_only=True):
    if row[1] is not None:
        tcs.append({
            'id':       row[1],   # TC ID
            '2depth':   row[3],   # component/screen unit
            '3depth':   row[4],   # sub-item
            'step':     row[9],   # test step
            'expected': row[10],  # expected result → convert into a policy/UX rule
        })
```

**Note:** `min_row` can vary between TC files. Check the header row and adjust.

### Step 3: Group by the 2depth column
Use the 2depth value to identify component/screen boundaries,
then extract rules from the `expected` values within each group.

### Step 4: Rule-conversion criteria

| Shape of the TC's expected result | Converts to |
|---|---|
| "if ~, ~ is shown" | a policy.md rule |
| "clicking ~ navigates to ~" | a ux-spec.md behavior |
| "up to N characters allowed" | a ux-spec.md number |
| "button disabled when ~" | a ux-spec.md state |
| "toast shown on error" | reference common-ux.md + note the exception in ux-spec.md |

### Step 5: Evidence-status notation criteria

For evidence-status notation (`[confirmed]` / `[confirmed: absent]` / `[needs confirmation]` / `[contradiction]`), see "Evidence-status notation" in SKILL.md. For TC analysis specifically, use these criteria:

- No TC covers this case at all, so the rule is unknown → `[needs confirmation]`
- The TC explicitly shows "this behavior/constraint doesn't exist" (e.g. a TC covering the related scenario exists but never mentions the constraint) → `[confirmed: absent]`
- The TC's expected results contradict each other → `[contradiction]`
- Per-client behavior isn't specified in the TC → `[needs confirmation]`

## Output format

After finishing TC analysis, deliver results in this order:

1. **Analysis summary**: TC count per sheet, number of rules derived, count of items by status (needs confirmation / contradiction)
2. **List of documents to create**: path and document type
3. **List of [needs confirmation]/[contradiction] items**: if a connected issue tracker exists, confirm whether to create issues; otherwise just hand over the list
4. **Document bodies**: generate in order

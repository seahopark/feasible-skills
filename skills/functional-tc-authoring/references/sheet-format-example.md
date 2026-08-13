# Sheet format example

## Legends tab (define once, reuse across the whole document)

| Symbol | Type | Description |
|---|---|---|
| 🔴 | Inline error | Red error text shown below the field |
| 📢 | Toolbar/toast notification | Appears briefly at bottom-center and auto-dismisses (usually 5s), can also be closed with X |
| ⚠️ | Form error banner | Icon + red text showing a server error inline at the bottom of the form |
| 🚫 | Button disabled | Button itself is disabled when a condition isn't met |
| ➡️ | Page navigation | Redirects to a different route on success |

When no emoji fits, use free text: `screen state change`, `modal opens`, `modal closes`, `list refresh`, etc.

## Section structure within a tab

Within a single feature-area tab, repeat this pattern for each logical sub-area:

```
[blank row]
["9. Project list — sort/filter/search"]                                   ← section title (numbered)
["Toolbar above the All Projects > Projects/Drafts tabs. 4 filter types, search box"] ← one-line description (which component/screen)
["#", "Test Case", "Precondition", "Action", "Expected Feedback", "Feedback Type"]  ← header (repeats per section)
["L-01", "No sort control (fixed order)", "Enter Projects tab", "Check list",
 "No separate sort UI; always fixed to \"most recently modified\" order", "screen state change"]
["L-02", ...]
...
```

## ID prefix examples (coin per section topic on the fly; must be unique within the document)

- `L-` list/table screens
- `BA-` bulk actions
- `RA-` row-level actions
- `CB-` create button
- `AB-` / `PB-` board views by assignee / by status
- `FC-` / `FL-` / `FD-` / `FH-` folder-related sub-areas (card / full list / detail / header)
- `N-` notification toasts
- For a different feature area, coin a new prefix that fits (`QT-`, `QF-`, `QE-`, etc.) — there's no globally fixed prefix set, it only needs to make sense within its section's scope

## Example: flagging a warning in a section description row (for demo/mockup code)

```
["12. Flagged Q/A review modal (demo only, doesn't actually save)"]
["⚠️ Static demo UI — the code's own comments state \"No API calls are made.\"
 The cases below only verify local screen state; a page refresh resets everything.
 Don't mistake this for a real persistence bug."]
```

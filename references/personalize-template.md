# Blueprint: Personalize Email Template

## Purpose
Take the master HTML email template and a single user's milestone data, then
replace every placeholder with the user's real values to produce a ready-to-send
HTML email body.

---

## Inputs

### 1. HTML Template (string)
The contents of `scripts/email-template.html`. The template contains these placeholders:

| Placeholder          | Replaced With                                         |
|----------------------|-------------------------------------------------------|
| `{{Name}}`           | User's full name                                      |
| `{{Milestone}}`      | Name of the BCDR milestone                            |
| `{{DueDate}}`        | Formatted due date (see formatting rule below)        |
| `{{ApplicationName}}`| BCDR application name associated with the milestone   |
| `{{Status}}`         | Current milestone status (e.g. "Pending")             |
| `{{BcdrPortalUrl}}`  | URL to the BCDR portal (default: https://bcdr.syngenta.com) |
| `{{Year}}`           | Current 4-digit year (e.g. "2026")                    |

### 2. User Record (JSON object)
```json
{
  "name": "Priya Sharma",
  "email": "priya.sharma@syngenta.com",
  "milestone": "Risk Assessment",
  "applicationName": "SAP ERP",
  "status": "Pending",
  "dueDate": "2026-03-28T00:00:00Z"
}
```

---

## Steps

1. Read the HTML template string.
2. For each placeholder listed in the table above, do a **global find-and-replace**
   (all occurrences, case-sensitive) substituting the corresponding user value.
3. Format `dueDate` as: **"28 Mar 2026"** (day Month Year — no time component).
4. If `applicationName` is missing or null, use `"N/A"`.
5. If `BcdrPortalUrl` is not provided, use `"https://bcdr.syngenta.com"`.
6. Set `{{Year}}` to the current calendar year.
7. After replacement, verify that **no `{{...}}` placeholders remain** in the output.
   If any unreplaced placeholder is found, return an error.

---

## Output

### Success
```json
{
  "success": true,
  "html": "<full personalized HTML string>"
}
```

### Error (unreplaced placeholders found)
```json
{
  "success": false,
  "error": "Unreplaced placeholders found: {{SomePlaceholder}}",
  "html": null
}
```

---

## Hard Rules
- Never truncate or modify the HTML structure.
- Never HTML-encode the replacement values (they are plain text, not HTML).
- The `dueDate` must always appear in human-readable format ("28 Mar 2026"), not ISO.
- If the user record is missing the `name` field, substitute `"Team Member"`.
- Output must be valid JSON.

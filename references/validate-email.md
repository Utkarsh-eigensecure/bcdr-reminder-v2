# Blueprint: Validate Email

## Purpose
Perform a final quality-check on the assembled email payload before it is sent.
This is the last gate before calling the send API — catch any issues that would
cause a bad or unprofessional email to be delivered.

---

## Input
The assembled email payload before sending:

```json
{
  "to":       "priya.sharma@syngenta.com",
  "subject":  "[BCDR Reminder] Risk Assessment Due on 28 Mar 2026",
  "htmlBody": "<full HTML string>"
}
```

---

## Checks to Run

Run ALL checks below. Collect ALL issues found (do not stop at the first failure).

### Check 1 — Recipient Domain
- `to` must end with `@syngenta.com`.
- Failure message: `"Recipient {email} is not a @syngenta.com address."`

### Check 2 — Subject Format
- Subject must not be empty.
- Subject must start with `[BCDR Reminder]`.
- Subject must be ≤ 80 characters.
- Failure message: `"Subject fails format check: {reason}"`

### Check 3 — Unreplaced Placeholders
- Scan the `htmlBody` for any remaining `{{...}}` tokens (regex: `\{\{[^}]+\}\}`).
- If found, list each token.
- Failure message: `"Unreplaced placeholder(s) in body: {{Token1}}, {{Token2}}"`

### Check 4 — HTML Structure
- The `htmlBody` must contain `<html` (case-insensitive).
- The `htmlBody` must contain `<body` (case-insensitive).
- The `htmlBody` must not be empty or whitespace-only.
- Failure message: `"Body is missing valid HTML structure."`

### Check 5 — Required Personalized Content
- The `htmlBody` must NOT still contain `{{Name}}` or `{{Milestone}}` as literal text.
- Failure message: `"Body appears to be missing personalised content."`

---

## Output

### All checks passed
```json
{
  "valid": true,
  "issues": []
}
```

### One or more checks failed
```json
{
  "valid": false,
  "issues": [
    "Recipient john.doe@gmail.com is not a @syngenta.com address.",
    "Unreplaced placeholder(s) in body: {{ApplicationName}}"
  ]
}
```

---

## Hard Rules
- Run ALL checks, not just the first failing one.
- Return `valid: true` only when `issues` is an empty array.
- Output must be valid JSON.
- Never truncate or modify the payload — this step is read-only.

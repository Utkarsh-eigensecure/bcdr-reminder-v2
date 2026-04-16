# Blueprint: Generate Email Subject

## Purpose
Generate a professional, context-aware email subject line for a BCDR milestone
reminder. The subject must be concise, actionable, and clearly communicate urgency
without being alarmist.

---

## Input
A single user milestone record:

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

## Subject Line Rules

1. **Always start with** `[BCDR Reminder]` tag.
2. Include the milestone name.
3. Include the due date in short format: `DD MMM YYYY`.
4. Keep total length **under 80 characters**.
5. Use title case.
6. Do NOT include the user's name in the subject (it appears in the body).
7. Do NOT use exclamation marks or aggressive language.
8. Use urgency words sparingly — only add "Action Required" if the due date has passed.

## Subject Templates by Status

| Status                | Subject Pattern                                              |
|-----------------------|--------------------------------------------------------------|
| Pending / Not Started | `[BCDR Reminder] {Milestone} Due on {DD MMM YYYY}`          |
| In Progress           | `[BCDR Reminder] Please Complete: {Milestone} by {DD MMM YYYY}` |
| Overdue               | `[BCDR Reminder] Action Required: {Milestone} Overdue — Due {DD MMM YYYY}` |

### Examples
- `[BCDR Reminder] Risk Assessment Due on 28 Mar 2026`
- `[BCDR Reminder] Please Complete: BIA Sign-off by 05 Apr 2026`
- `[BCDR Reminder] Action Required: BCDR Review Overdue — Due 25 Mar 2026`

---

## Output

Return **only** a JSON object:
```json
{
  "subject": "[BCDR Reminder] Risk Assessment Due on 28 Mar 2026"
}
```

No explanation, no extra fields, no markdown fences outside the JSON.

---

## Hard Rules
- Subject must be a non-empty string.
- Must start with `[BCDR Reminder]`.
- Must be under 80 characters.
- Output must be valid JSON.

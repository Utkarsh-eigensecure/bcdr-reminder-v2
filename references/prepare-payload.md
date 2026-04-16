# Blueprint: Prepare API Payload

## Purpose
Assemble the final call to `mcp__bcdr-server__send_bcdr_reminder`, combining the
personalized HTML body, generated subject, and recipient address from the user record.

---

## Inputs

### 1. User Record
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

### 2. Subject (from generate-email-subject step)
```json
{ "subject": "[BCDR Reminder] Risk Assessment Due on 28 Mar 2026" }
```

### 3. Personalized HTML Body (from personalize-template step)
```json
{ "success": true, "html": "<full HTML string>" }
```

---

## Steps

1. Set `to` to `user.email`.
2. Set `subject` to the value from the generate-email-subject output.
3. Set `htmlBody` to the HTML string from the personalize-template output.
4. Validate that `to` ends with `@syngenta.com`. If not, return an error.
5. Validate that `htmlBody` is not empty and contains no unreplaced `{{...}}` tokens.
6. Validate that `subject` starts with `[BCDR Reminder]` and is not empty.

---

## MCP Tool Call

```
mcp__bcdr-server__send_bcdr_reminder(
  to:       "priya.sharma@syngenta.com",
  subject:  "[BCDR Reminder] Risk Assessment Due on 28 Mar 2026",
  htmlBody: "<full personalized HTML string>"
)
```

### Error
```json
{
  "valid": false,
  "error": "Recipient email 'priya.sharma@gmail.com' is not a @syngenta.com address."
}
```

---

## Hard Rules
- `to` must always end with `@syngenta.com`.
- `subject` must start with `[BCDR Reminder]`.
- `htmlBody` must be non-empty HTML with no remaining `{{...}}` placeholders.
- Never log or expose the full HTML body in error messages.
- Success response → mark user as **Sent**. Error response or exception → mark user as **Failed**, do NOT retry.

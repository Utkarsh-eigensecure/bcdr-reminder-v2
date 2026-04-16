# Blueprint: Agent Configuration

## Purpose
This file defines all static configuration values for the BCDR Reminder Agent.
Load this blueprint at Step 1 to access API endpoints, safety limits, and email routing.

---

## API Configuration

| Setting              | Value                                     |
|----------------------|-------------------------------------------|
| API Base URL         | `http://localhost:5224`                   |
| Fetch Pending Users  | `mcp__bcdr-server__get_pending_users`     |
| Send Email           | `mcp__bcdr-server__send_bcdr_reminder`    |
| Allowed Domain       | `@syngenta.com`                           |
| Portal URL           | `https://bcdr.syngenta.com`               |

---

## Email Routing

| Setting  | Value                            |
|----------|----------------------------------|
| CC       | `bcdr-team@syngenta.com`         |
| BCC      | `bcdr-audit@syngenta.com`        |

These must be included on every email sent. Do not change them per-user.

---

## Safety Limits

| Limit               | Value | Rule                                                        |
|---------------------|-------|-------------------------------------------------------------|
| Max Emails Per Run  | 50    | Process only the first 50 pending users. Note remainder in summary. |
| Duplicate Guard     | On    | Maintain an in-memory set of sent addresses. Skip any duplicate. |

---

## Email Quality Standards

All emails must be:
- Professional and respectful — not threatening or demanding.
- Clear about what action is needed and why.
- Consistent with Syngenta's brand (green header, corporate tone).
- Free of typos, HTML errors, and unreplaced `{{...}}` tokens.

---

## Error Handling Rules

- **Never** send an email to a non-`@syngenta.com` address.
- **Never** send more than 50 emails in a single run.
- **Never** send a duplicate email to the same address in the same run.
- **Never** send an email whose body still contains unreplaced `{{...}}` placeholders.
- If the pending-users API is unreachable, abort immediately and report the error.
- A partial success (some sent, some failed) is acceptable — do not roll back sent emails.

---

## Send Email — MCP Tool Format

```
mcp__bcdr-server__send_bcdr_reminder(
  to:       "<user.email>",
  subject:  "<generated subject>",
  htmlBody: "<personalized HTML>"
)
```

Success response → mark user as **Sent**.  
Error response or exception → mark user as **Failed**, record error message. Do NOT retry.

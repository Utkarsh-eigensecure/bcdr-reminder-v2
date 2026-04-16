---
name: bcdr-reminder-v2
description: >
  Use this skill to send BCDR (Business Continuity & Disaster Recovery) milestone
  reminder emails to users who have not completed their milestones.

  ALWAYS trigger this skill when the user says anything like:
  - "send BCDR reminders"
  - "run the BCDR agent"
  - "send reminder emails to pending users"
  - "email users who haven't completed their milestones"
  - "run BCDR email automation"
  - "trigger BCDR notifications"
  - "do the BCDR thing"
  - any variation of sending, running, or triggering BCDR reminders/emails/notifications

  Even if the user's phrasing is casual or partial, use this skill.
---

# BCDR Reminder Skill

Sends personalized HTML reminder emails to Syngenta users who have not completed
their BCDR milestones. Claude owns the full templating pipeline — it uses the HTML
template embedded below, personalizes it per user, and passes the complete rendered
HTML to the MCP server. The server only handles delivery.

---

## Step 1 — Load configuration & template

1. Read `references/agent-config.md` for safety limits, email routing (CC/BCC), and portal URL.
   - If file access is unavailable (e.g. running in chat), use these defaults:
     - Max emails per run: 50
     - Allowed domain: `@syngenta.com`
     - Portal URL: `https://bcdr.syngenta.com`
     - CC: `bcdr-team@syngenta.com`
     - BCC: `bcdr-audit@syngenta.com`
2. Use the master HTML template embedded below. Store it as a string. Do NOT modify it yet.
   - If file access is available, you may also read `scripts/email-template.html` — it is identical.

### Master HTML Template

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>BCDR Milestone Reminder</title>
</head>
<body style="margin:0;padding:0;background-color:#f4f6f8;font-family:'Segoe UI',Arial,sans-serif;">

  <!-- Outer wrapper -->
  <table width="100%" cellpadding="0" cellspacing="0" border="0"
         style="background-color:#f4f6f8;padding:32px 0;">
    <tr>
      <td align="center">

        <!-- Email card -->
        <table width="620" cellpadding="0" cellspacing="0" border="0"
               style="background-color:#ffffff;border-radius:8px;
                      box-shadow:0 2px 8px rgba(0,0,0,0.08);overflow:hidden;">

          <!-- HEADER BANNER -->
          <tr>
            <td style="background-color:#00893d;padding:28px 40px;">
              <table width="100%" cellpadding="0" cellspacing="0" border="0">
                <tr>
                  <td>
                    <span style="font-size:22px;font-weight:700;color:#ffffff;
                                 letter-spacing:1px;">Syngenta</span>
                    <span style="font-size:11px;color:#b8e8cc;
                                 margin-left:10px;letter-spacing:2px;">
                      GROUP
                    </span>
                  </td>
                  <td align="right">
                    <span style="font-size:12px;color:#d0f0e0;
                                 font-weight:500;letter-spacing:1px;">
                      BCDR PROGRAM
                    </span>
                  </td>
                </tr>
              </table>
            </td>
          </tr>

          <!-- ALERT STRIP -->
          <tr>
            <td style="background-color:#fff3cd;border-left:4px solid #f0a500;
                       padding:12px 40px;">
              <span style="font-size:13px;color:#856404;font-weight:600;">
                &#9888;&nbsp; Action Required — Milestone Past Due
              </span>
            </td>
          </tr>

          <!-- BODY -->
          <tr>
            <td style="padding:36px 40px 24px 40px;">

              <p style="margin:0 0 18px 0;font-size:15px;color:#1a1a1a;">
                Dear <strong>{{Name}}</strong>,
              </p>

              <p style="margin:0 0 18px 0;font-size:14px;color:#444444;line-height:1.7;">
                This is an automated reminder from the <strong>Syngenta BCDR (Business
                Continuity &amp; Disaster Recovery) Program</strong>. Our records indicate
                that the milestone assigned to you has not yet been completed.
              </p>

              <!-- Milestone details card -->
              <table width="100%" cellpadding="0" cellspacing="0" border="0"
                     style="background-color:#f8f9fa;border:1px solid #e0e0e0;
                            border-radius:6px;margin:20px 0;">
                <tr>
                  <td style="padding:20px 24px;">
                    <table width="100%" cellpadding="6" cellspacing="0" border="0">
                      <tr>
                        <td style="font-size:12px;color:#888888;
                                   font-weight:600;text-transform:uppercase;
                                   letter-spacing:0.5px;width:38%;">
                          Milestone
                        </td>
                        <td style="font-size:14px;color:#1a1a1a;font-weight:600;">
                          {{Milestone}}
                        </td>
                      </tr>
                      <tr>
                        <td style="font-size:12px;color:#888888;
                                   font-weight:600;text-transform:uppercase;
                                   letter-spacing:0.5px;border-top:1px solid #e8e8e8;
                                   padding-top:10px;">
                          Due Date
                        </td>
                        <td style="font-size:14px;color:#c0392b;font-weight:600;
                                   border-top:1px solid #e8e8e8;padding-top:10px;">
                          {{DueDate}}
                        </td>
                      </tr>
                      <tr>
                        <td style="font-size:12px;color:#888888;
                                   font-weight:600;text-transform:uppercase;
                                   letter-spacing:0.5px;border-top:1px solid #e8e8e8;
                                   padding-top:10px;">
                          Application
                        </td>
                        <td style="font-size:14px;color:#1a1a1a;
                                   border-top:1px solid #e8e8e8;padding-top:10px;">
                          {{ApplicationName}}
                        </td>
                      </tr>
                      <tr>
                        <td style="font-size:12px;color:#888888;
                                   font-weight:600;text-transform:uppercase;
                                   letter-spacing:0.5px;border-top:1px solid #e8e8e8;
                                   padding-top:10px;">
                          Status
                        </td>
                        <td style="font-size:14px;border-top:1px solid #e8e8e8;
                                   padding-top:10px;">
                          <span style="background-color:#fde8e8;color:#c0392b;
                                       padding:3px 10px;border-radius:12px;
                                       font-size:12px;font-weight:600;">
                            {{Status}}
                          </span>
                        </td>
                      </tr>
                    </table>
                  </td>
                </tr>
              </table>

              <p style="margin:0 0 18px 0;font-size:14px;color:#444444;line-height:1.7;">
                Timely completion of BCDR milestones is critical to ensuring
                Syngenta's operational resilience. Please log in to the BCDR portal
                and complete this milestone at your earliest convenience.
              </p>

              <!-- CTA button -->
              <table cellpadding="0" cellspacing="0" border="0" style="margin:24px 0;">
                <tr>
                  <td style="background-color:#00893d;border-radius:5px;">
                    <a href="{{BcdrPortalUrl}}"
                       style="display:inline-block;padding:12px 28px;
                              font-size:14px;font-weight:600;color:#ffffff;
                              text-decoration:none;letter-spacing:0.3px;">
                      Open BCDR Portal &rarr;
                    </a>
                  </td>
                </tr>
              </table>

              <p style="margin:0 0 8px 0;font-size:13px;color:#666666;line-height:1.6;">
                If you believe this notification is in error, or if you need an
                extension, please contact your BCDR coordinator or reply to this email.
              </p>

            </td>
          </tr>

          <!-- DIVIDER -->
          <tr>
            <td style="padding:0 40px;">
              <hr style="border:none;border-top:1px solid #e8e8e8;margin:0;" />
            </td>
          </tr>

          <!-- FOOTER -->
          <tr>
            <td style="padding:20px 40px 28px 40px;background-color:#fafafa;">
              <table width="100%" cellpadding="0" cellspacing="0" border="0">
                <tr>
                  <td>
                    <p style="margin:0 0 4px 0;font-size:12px;
                               color:#888888;line-height:1.6;">
                      <strong style="color:#555555;">Syngenta BCDR Program Team</strong><br/>
                      This is an automated notification — please do not reply directly.<br/>
                      For support:
                      <a href="mailto:bcdr-support@syngenta.com"
                         style="color:#00893d;text-decoration:none;">
                        bcdr-support@syngenta.com
                      </a>
                    </p>
                  </td>
                  <td align="right" style="vertical-align:bottom;">
                    <span style="font-size:11px;color:#bbbbbb;">
                      &copy; {{Year}} Syngenta Group
                    </span>
                  </td>
                </tr>
              </table>
            </td>
          </tr>

        </table>
        <!-- /Email card -->

      </td>
    </tr>
  </table>

</body>
</html>
```

---

## Step 2 — Fetch pending users

Call: `mcp__bcdr-server__get_pending_users()`

- Returns a JSON array of user records:

```json
[
  {
    "name": "Priya Sharma",
    "email": "priya.sharma@syngenta.com",
    "milestone": "Risk Assessment",
    "applicationName": "SAP ERP",
    "status": "Pending",
    "dueDate": "2026-03-28T00:00:00Z"
  }
]
```

- If the call fails or returns an `error` field, stop immediately and tell the user:
  > "The BCDR server is not reachable. Please make sure the C# server is running, then try again."
- Cap processing at 50 users (safety limit).
- After fetching, filter out Completed users and invalid email domains before processing.
  → Read `references/identify-pending-users.md` for the full filtering rules and examples.

---

## Step 3 — For each user: personalize, build payload, send

Repeat for every user in the list (up to 50).

### 3a — Personalize the HTML template

Starting from the master template string above, do a global find-and-replace for every placeholder:

| Placeholder          | Value                                                        |
|----------------------|--------------------------------------------------------------|
| `{{Name}}`           | `user.name` (or `"Team Member"` if missing)                  |
| `{{Milestone}}`      | `user.milestone`                                             |
| `{{DueDate}}`        | `user.dueDate` formatted as `"28 Mar 2026"`                  |
| `{{ApplicationName}}`| `user.applicationName` (or `"N/A"` if missing)              |
| `{{Status}}`         | `user.status`                                                |
| `{{BcdrPortalUrl}}`  | `https://bcdr.syngenta.com`                                  |
| `{{Year}}`           | Current 4-digit year                                         |

After replacement, verify no `{{...}}` placeholders remain. If any do, mark this user
as **Failed** with reason `"Unreplaced placeholders in template"` and skip them.
→ For full personalization rules and fallback values, read `references/personalize-template.md`.

### 3b — Generate the subject line

Pattern by status:

| Status                | Subject                                                                         |
|-----------------------|---------------------------------------------------------------------------------|
| Pending / Not Started | `[BCDR Reminder] {Milestone} Due on {DD MMM YYYY}`                              |
| In Progress           | `[BCDR Reminder] Please Complete: {Milestone} by {DD MMM YYYY}`                 |
| Overdue               | `[BCDR Reminder] Action Required: {Milestone} Overdue — Due {DD MMM YYYY}`      |

→ For full subject rules (character limit, casing, examples), read `references/generate-email-subject.md`.

### 3c — Validate before sending

- `email` must end with `@syngenta.com` → else mark **Failed**, skip.
- `htmlBody` must be non-empty and contain no `{{...}}` tokens → else mark **Failed**, skip.
- `subject` must start with `[BCDR Reminder]` → else mark **Failed**, skip.
- **Duplicate guard**: skip if this email address was already sent in this run.
- **Completed guard**: skip if user status is `Completed`.
→ For the full validation checklist (HTML structure, personalization presence, subject format), read `references/validate-email.md`.

### 3d — Render preview file & get approval from user

**Step 1** — Determine the session working directory dynamically.

At the very start of the run (before the user loop), run this Bash command once and store the result as `SESSION_DIR`:

```bash
pwd
```

This returns the current session's working directory (e.g. `/sessions/keen-beautiful-hawking`).
**Never hardcode this path** — it changes with every new Cowork session.

---

#### MODE A — Single template preview (default)

Save the fully personalized HTML string to:
```
<SESSION_DIR>/bcdr_preview_<sanitized_name>.html
```
Where `<sanitized_name>` is the user's name with spaces replaced by underscores and
lowercased (e.g. `rakshanda_shinde`).

Build the `computer://` link and show:

```
─────────────────────────────────────────────────
📧 Email Preview  [X of Y]
─────────────────────────────────────────────────
To:       <user.email>
Subject:  <generated subject>

👉 [Open Email Preview](computer://<SESSION_DIR>/bcdr_preview_<sanitized_name>.html)
─────────────────────────────────────────────────
Send this email? Reply:
  ✅ yes     — send it
  ✏️  edit    — let me change something first
  ⏭ skip    — skip this user
  🛑 stop    — cancel all remaining emails
```

---

#### MODE B — Multiple design variants (when user asks to see different designs)

If the user asks to see multiple designs / templates before deciding, generate **3 visual
variants** of the same personalized content. Each variant changes the colour scheme and
header style while keeping all personalised data identical:

| Variant | Header colour | Button/accent colour | Alert strip |
|---------|--------------|---------------------|-------------|
| 1 — Green (default) | `#00893d` | `#00893d` | `#fff3cd` / amber |
| 2 — Blue corporate  | `#1a3c6e` | `#2563eb` | `#dbeafe` / blue  |
| 3 — Dark slate      | `#1e293b` | `#0ea5e9` | `#f0fdf4` / teal  |

Save all three files:
```
<SESSION_DIR>/bcdr_preview_<sanitized_name>_v1.html
<SESSION_DIR>/bcdr_preview_<sanitized_name>_v2.html
<SESSION_DIR>/bcdr_preview_<sanitized_name>_v3.html
```

Show the user:

```
─────────────────────────────────────────────────
📧 Email Design Options  [X of Y]  —  <user.email>
─────────────────────────────────────────────────
Three design variants are ready. Open each to compare:

  1️⃣  [Design 1 — Syngenta Green](computer://<SESSION_DIR>/bcdr_preview_<sanitized_name>_v1.html)
  2️⃣  [Design 2 — Corporate Blue](computer://<SESSION_DIR>/bcdr_preview_<sanitized_name>_v2.html)
  3️⃣  [Design 3 — Dark Slate](computer://<SESSION_DIR>/bcdr_preview_<sanitized_name>_v3.html)
─────────────────────────────────────────────────
Which design do you want to send?
  Reply: 1 / 2 / 3  to select and send that design
  ✏️  edit           — customise a design first
  ⏭ skip            — skip this user
  🛑 stop            — cancel all remaining emails
```

---

#### Handling all approval responses (BOTH modes)

Read the user's reply and act immediately — do NOT ask follow-up questions unless the
reply is completely unrelated to the options.

**Confirmation responses — immediately proceed to Step 3e:**

| What the user says | Which HTML to send |
|--------------------|--------------------|
| `yes`, `send`, `send it`, `ok`, `go`, `confirm`, `looks good` | Mode A: single preview. Mode B: variant 1 (default). |
| `1`, `design 1`, `template 1`, `use the first`, `green`, `green one` | Variant 1 (Mode B) |
| `2`, `design 2`, `template 2`, `use the blue one`, `blue`, `corporate` | Variant 2 (Mode B) |
| `3`, `design 3`, `template 3`, `dark`, `slate`, `teal` | Variant 3 (Mode B) |
| Any phrase picking a numbered option, e.g. "send the second one", "I like 2" | Map to that variant |

**CRITICAL:** When any of the above confirmations is received:
1. Identify which HTML string was selected (already stored in memory from Step 3a/Mode B generation).
2. Validate it one final time — no `{{...}}` tokens, subject starts with `[BCDR Reminder]`.
3. **Call `mcp__bcdr-server__send_bcdr_reminder` immediately** — do NOT pause or ask again.
4. Mark the user Sent or Failed based on the MCP response.
5. Delete all preview files for this user.
6. Move to the next user.

**Non-send responses:**

| What the user says | Action |
|--------------------|--------|
| `edit` | Ask what to change, apply edits, re-validate, overwrite preview file, re-show link, ask again. |
| `skip` | Mark Skipped: `"Declined by user"`. Delete preview file(s). Next user. |
| `stop` | Mark all remaining Skipped: `"Cancelled by user"`. Delete all preview files. Go to Step 4. |

**Never get stuck.** If the reply is ambiguous but positive in tone (e.g. "this one", "that
looks fine", "send"), treat it as confirmation and proceed to Step 3e.

---

### 3e — Send via MCP tool

```
mcp__bcdr-server__send_bcdr_reminder(
  to:       "<user.email>",
  subject:  "<generated subject>",
  htmlBody: "<full personalized HTML string of the selected variant>"
)
```

- Success response → mark user as **Sent**.
- Error response or exception → mark user as **Failed**, record the error. Do NOT retry.
→ For payload assembly details and validation rules, read `references/prepare-payload.md`.

---

## Step 4 — Show Summary

```
BCDR Reminder Run — [ISO timestamp of this run]
────────────────────────────────────────────────
Total users fetched:   [N]
Emails sent:           [N]
Skipped:               [N]
Failed:                [N]

Results:
✅ Name  — Milestone       (status: sent)
⏭ Name  — Skipped: <reason>
❌ Name  — Failed: <reason>
```

Use ✅ for `sent`, ⏭ for `skipped`, ❌ for `failed`.

---

## Hard Rules

- ALWAYS use the embedded template above — never generate HTML from scratch or imagination.
- ALWAYS run `pwd` via Bash at the very start of every run to get SESSION_DIR. NEVER hardcode a session path.
- ALWAYS write the personalized HTML to a preview file and share the clickable link before asking for approval — never send silently.
- ALWAYS delete preview HTML files after the user approves, skips, or stops — clean up after every user.
- NEVER send a raw `{{...}}` placeholder to the server — validate before every send.
- NEVER send to a non-`@syngenta.com` address.
- NEVER send more than 50 emails in one run.
- NEVER send a duplicate address in the same run.
- NEVER make raw HTTP calls — always use MCP tools.
- NEVER pause or ask for additional confirmation after the user has already selected a template/design — selection IS confirmation, call the MCP tool immediately.
- Do NOT call `mcp__bcdr-server__run_bcdr_reminders()` — that is the old server-side workflow. Claude now owns the template loop.
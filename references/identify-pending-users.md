# Blueprint: Identify Pending Users

## Purpose
Filter the raw user-milestone list returned by the BCDR API and return only the
users who have NOT completed their assigned milestone and are therefore eligible
to receive a reminder email.

---

## Input
A JSON array of user milestone objects. Each object has the following fields:

| Field       | Type     | Description                                      |
|-------------|----------|--------------------------------------------------|
| name        | string   | Full name of the user                            |
| email       | string   | User's email address (must be @syngenta.com)     |
| milestone   | string   | Name of the BCDR milestone assigned to the user  |
| applicationName | string | BCDR application name associated with the milestone |
| status      | string   | Current completion status of the milestone       |
| dueDate     | string   | ISO 8601 due date (e.g. "2026-04-01T00:00:00Z")  |

### Example Input
```json
[
  {
    "name": "Priya Sharma",
    "email": "priya.sharma@syngenta.com",
    "milestone": "Risk Assessment",
    "applicationName": "SAP ERP",
    "status": "Pending",
    "dueDate": "2026-03-28T00:00:00Z"
  },
  {
    "name": "Rohan Mehta",
    "email": "rohan.mehta@syngenta.com",
    "milestone": "BCDR Review",
    "applicationName": "Oracle DB",
    "status": "Completed",
    "dueDate": "2026-03-25T00:00:00Z"
  },
  {
    "name": "Anjali Desai",
    "email": "anjali.desai@syngenta.com",
    "milestone": "BIA Sign-off",
    "applicationName": "Workday",
    "status": "In Progress",
    "dueDate": "2026-04-05T00:00:00Z"
  }
]
```

---

## Filtering Rules

1. Iterate through every object in the input array.
2. **Include** the user if their `status` is ANY value other than `"Completed"`
   (case-insensitive). This includes: `"Pending"`, `"In Progress"`, `"Not Started"`,
   `"Overdue"`, or any unrecognised value.
3. **Exclude** the user if their `status` is exactly `"Completed"` (case-insensitive).
4. **Exclude** any user whose email does NOT end with `@syngenta.com`.
5. Do NOT modify any field values — pass them through unchanged.
6. Return the filtered list as a JSON array (empty array `[]` if no pending users).
7. If the filtered list exceeds **50 users**, process only the first 50 and note the
   remainder in the summary.

---

## Output

Return **only** the JSON array. No explanation, no markdown fences, no extra text.

### Example Output (from the input above)
```json
[
  {
    "name": "Priya Sharma",
    "email": "priya.sharma@syngenta.com",
    "milestone": "Risk Assessment",
    "applicationName": "SAP ERP",
    "status": "Pending",
    "dueDate": "2026-03-28T00:00:00Z"
  },
  {
    "name": "Anjali Desai",
    "email": "anjali.desai@syngenta.com",
    "milestone": "BIA Sign-off",
    "applicationName": "Workday",
    "status": "In Progress",
    "dueDate": "2026-04-05T00:00:00Z"
  }
]
```

---

## Hard Rules
- Never include a user whose status is "Completed".
- Never include a user whose email domain is not @syngenta.com.
- Never alter field names or values.
- If the input array is empty, return `[]`.
- Output must be valid JSON.

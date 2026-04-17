# 📧 BCDR Reminder v2

Sends BCDR (Business Continuity & Disaster Recovery) reminder emails using a runtime-customizable HTML template.  
No server-side template changes required — full flexibility handled at execution time.

---

## 🚀 Features

### ✨ Runtime Template Customization
Modify email content without touching server code or local files.

### 🎯 Personalized Emails per User
Dynamic placeholders like name, milestone, due date, and application.

### 👀 Preview Before Sending
Generates HTML preview files for user approval before sending emails.

### 🔁 Multiple Design Variants
Supports different UI themes (Green, Blue, Dark) for email styling.

### 🛡️ Built-in Validation & Safety
- Domain restriction (`@syngenta.com`)
- Max 50 emails per run
- Duplicate prevention
- Placeholder validation

### ⚡ MCP-Based Email Delivery
Clean separation — templating handled by Claude, delivery handled by MCP server.

---

## 📦 What’s Included
```bash
bcdr-reminder-v2/
├── SKILL.md ← Direction & orchestration logic (required)
├── references/ ← Blueprints & rules (optional but recommended)
│ ├── agent-config.md
│ ├── generate-email-subject.md
│ ├── identify-pending-users.md
│ ├── personalize-template.md
│ ├── prepare-payload.md
│ └── validate-email.md
└── scripts/
└── email-template.html
```


---

## ⚙️ How It Works

### 1. Load Configuration & Template
- Uses embedded HTML template  
- Applies default config if external files are unavailable  

### 2. Fetch Pending Users
- Filters valid users (non-completed, valid domain)  

### 3. Personalize Email
- Replaces placeholders with user-specific data  
- Formats dates and fields  

### 4. Generate Preview
- Saves HTML file in session directory  
- Provides clickable preview link  

### 5. User Approval
Options:
- ✅ Send  
- ✏️ Edit  
- ⏭ Skip  
- 🛑 Stop  

### 6. Send Email
- Sends fully rendered HTML  

### 7. Summary Report
- Displays sent, skipped, and failed emails

##📥 Installation
- For Claude.ai Users
- Download the latest release: csv-data-summarizer.zip
- Go to Claude.ai → Settings → Capabilities → Skills
- Upload the zip file
- Enable the skill
- Done! ✨

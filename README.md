# 🛡️ Insurance Claims Triage Automation

AI-powered workflow that automatically triages insurance claims — built with **n8n**, **HKBU ChatGPT API**, and deployed on **GitHub Pages**.

---

## How It Works

```
User fills form (GitHub Pages)
        ↓
n8n Cloud Webhook receives claim
        ↓
HKBU ChatGPT API analyzes description
        ↓
Parse AI response (Code Node)
        ↓
Switch → route by priority
   High  →  24h SLA
   Medium →  3–5 day SLA
   Low    →  self-service
        ↓
Return result → displayed on form
```

---

## Setup (3 steps, ~3 hours total)

### Step 1 — n8n Cloud (~1 hour)

1. Go to [n8n.io](https://n8n.io) → **Start for free** → create account
2. Click **Add workflow** → **Import** → upload `n8n/workflow.json`
3. Click the **HKBU ChatGPT** node → update the `api-key` header with your key
4. Click **Activate** toggle (top right)
5. Copy the **Webhook URL** shown in the Webhook node (looks like `https://YOUR-NAME.app.n8n.cloud/webhook/claims`)

### Step 2 — Update the form (~5 minutes)

Open `web/index.html` and replace line:
```js
const N8N_WEBHOOK_URL = "https://YOUR-USERNAME.app.n8n.cloud/webhook/claims";
```
with your actual webhook URL from Step 1.

### Step 3 — GitHub Pages (~15 minutes)

1. Create a new GitHub repository (e.g. `insurance-claims-automation`)
2. Upload all project files
3. Go to **Settings → Pages → Source → main branch → /web folder** → Save
4. Your live URL will be: `https://YOUR-USERNAME.github.io/insurance-claims-automation`

---

## Tech Stack

| Component | Tool |
|---|---|
| Workflow automation | n8n Cloud |
| AI analysis | HKBU ChatGPT API (GPT-4o) |
| Frontend | HTML / JavaScript |
| Hosting | GitHub Pages |

---

## n8n Workflow — 8 Nodes

| Node | Type | Purpose |
|---|---|---|
| Webhook | Trigger | Receives claim POST request |
| HKBU ChatGPT | HTTP Request | Calls AI API to analyze claim |
| Parse AI Response | Code (JS) | Parses JSON, handles errors |
| Route by Priority | Switch | Routes High / Medium / Low |
| HIGH Priority | Code | Adds 24h SLA note |
| MEDIUM Priority | Code | Adds 3–5 day SLA note |
| LOW Priority | Code | Adds self-service note |
| Respond | Webhook Response | Returns structured JSON to form |

---

## What the AI Extracts

From a plain-text claim description, the workflow automatically produces:
- **Priority level** (High / Medium / Low)
- **Required documents** list
- **Estimated processing time** (days)
- **Plain-language summary**
- **Next steps** for the claimant

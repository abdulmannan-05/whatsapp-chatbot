# 🍕 WhatsApp AI Customer Support Chatbot

<div align="center">

![WhatsApp](https://img.shields.io/badge/WhatsApp-25D366?style=for-the-badge&logo=whatsapp&logoColor=white)
![Google Gemini](https://img.shields.io/badge/Google%20Gemini-4285F4?style=for-the-badge&logo=google&logoColor=white)
![Make.com](https://img.shields.io/badge/Make.com-6D00CC?style=for-the-badge&logo=make&logoColor=white)
![Google Sheets](https://img.shields.io/badge/Google%20Sheets-34A853?style=for-the-badge&logo=google-sheets&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)

**An intelligent, no-code WhatsApp chatbot that handles pizza store customer support — powered by Google Gemini AI, a live Google Sheets FAQ database, and automatic human escalation via email.**

[Features](#-features) · [Architecture](#-architecture) · [Getting Started](#-getting-started) · [Configuration](#-configuration) · [How It Works](#-how-it-works) · [FAQ](#-faq)

</div>

---

## 📖 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Architecture](#-architecture)
- [Tech Stack](#-tech-stack)
- [Prerequisites](#-prerequisites)
- [Getting Started](#-getting-started)
- [Configuration](#-configuration)
- [How It Works](#-how-it-works)
- [Project Structure](#-project-structure)
- [Customization Guide](#-customization-guide)
- [Limitations](#-limitations)
- [FAQ](#-faq)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🌟 Overview

This project is a **fully automated, AI-powered WhatsApp customer support bot** for a pizza store, built entirely on [Make.com](https://make.com) (formerly Integromat) — no backend code required.

When a customer sends a WhatsApp message, the bot:
1. Reads your live FAQ knowledge base from Google Sheets
2. Sends the question + FAQ context to Google Gemini AI
3. Replies instantly on WhatsApp in the customer's own language
4. Automatically escalates to a human agent via email if the customer requests it

> **Zero-code. Production-ready. Deploy in under 30 minutes.**

---

## ✨ Features

| Feature | Description |
|---|---|
| 🤖 **AI-Powered Replies** | Uses Gemini 2.5 Flash-Lite to generate natural, contextual responses |
| 📋 **Live FAQ Database** | FAQ answers are pulled from Google Sheets — update them anytime, no redeployment needed |
| 🌍 **Multilingual Support** | Automatically detects and replies in the customer's language |
| 🚨 **Human Escalation** | Detects escalation phrases and emails the support team automatically |
| 🔇 **Self-Message Filter** | Ignores messages sent by the bot itself — prevents infinite loops |
| ⚡ **Real-Time** | Responds in seconds via webhook-driven flow |
| 🍕 **Brand Personality** | Friendly, energetic pizza-themed voice with emoji support |
| 🔌 **No-Code** | Entire automation built in Make.com — importable with one click |

---

## 🏗 Architecture

```
Customer sends WhatsApp message
         │
         ▼
┌─────────────────────┐
│  Webhook Trigger    │  ← Whapi.cloud sends payload to Make.com
│  (Make.com)         │
└─────────────────────┘
         │
         ▼
┌─────────────────────┐
│  Self-Message       │  ← Filters out bot's own messages & empty texts
│  Filter             │    (prevents infinite loops)
└─────────────────────┘
         │
         ▼
┌─────────────────────┐
│  Google Sheets      │  ← Fetches all FAQ rows from the knowledge base
│  FAQ Lookup         │    (pizza_store_faq spreadsheet)
└─────────────────────┘
         │
         ▼
┌─────────────────────┐
│  Text Aggregator    │  ← Formats FAQ rows as:
│                     │    "Question: X | Answer: Y"
└─────────────────────┘
         │
         ▼
┌─────────────────────┐
│  Gemini 2.5         │  ← Generates a reply using:
│  Flash-Lite AI      │    • Customer's message
│                     │    • Full FAQ context (injected in system prompt)
│                     │    • Brand persona instructions
└─────────────────────┘
         │
         ▼
┌─────────────────────────────────────────────────────────┐
│                  Router (Branching)                     │
│                                                         │
│  Route A: Human Escalation    Route B: AI Auto-Reply    │
│  ─────────────────────────    ─────────────────────     │
│  Gemini detects escalation    Sends AI response back    │
│  → Sends email to support     to customer via           │
│    team with phone number     Whapi.cloud API           │
│    and original message                                 │
└─────────────────────────────────────────────────────────┘
```

---

## 🛠 Tech Stack

| Component | Service | Purpose |
|---|---|---|
| **Automation Platform** | [Make.com](https://make.com) | Orchestrates the entire flow |
| **WhatsApp API** | [Whapi.cloud](https://whapi.cloud) | Sends & receives WhatsApp messages |
| **AI Model** | Google Gemini 2.5 Flash-Lite | Generates intelligent customer replies |
| **Knowledge Base** | Google Sheets | Stores & serves the live FAQ database |
| **Escalation** | Gmail (Google Email) | Notifies support team when human help is needed |

---

## ✅ Prerequisites

Before you begin, make sure you have:

- [ ] A **Make.com** account (Free tier works for low volume)
- [ ] A **Whapi.cloud** account with a connected WhatsApp channel and API token
- [ ] A **Google account** with access to Google Sheets and Gmail
- [ ] A **Google Gemini API key** (available at [Google AI Studio](https://aistudio.google.com/))
- [ ] A copy of the FAQ spreadsheet in your Google Drive (see [Configuration](#-configuration))

---

## 🚀 Getting Started

### Step 1 — Clone this Repository

```bash
git clone https://github.com/YOUR_USERNAME/whatsapp-pizza-chatbot.git
cd whatsapp-pizza-chatbot
```

### Step 2 — Set Up the FAQ Google Sheet

1. Go to [Google Sheets](https://sheets.google.com) and create a new spreadsheet
2. Name it `pizza_store_faq`
3. Set up two columns in row 1 as headers:

| Column A | Column B |
|---|---|
| `Question` | `Answer` |

4. Add your FAQ rows below, for example:

| Question | Answer |
|---|---|
| What are your opening hours? | We are open Monday to Saturday, 11am–11pm. |
| Do you offer vegan options? | Yes! We have a full vegan menu available. |
| How long is delivery? | Delivery takes 30–45 minutes on average. |

5. Copy the **Spreadsheet ID** from the URL:
   `https://docs.google.com/spreadsheets/d/`**`YOUR_SPREADSHEET_ID`**`/edit`

### Step 3 — Import the Blueprint into Make.com

1. Log in to [Make.com](https://make.com)
2. Click **"Create a new scenario"**
3. Click the **three-dot menu (⋮)** at the bottom of the screen
4. Select **"Import Blueprint"**
5. Upload the file: `WHATSAPP_CHATBOT_blueprint.json`
6. Click **"Save"**

### Step 4 — Reconnect All Services

After importing, each module will show a connection error (expected — credentials don't transfer). Reconnect them one by one:

| Module | What to Reconnect |
|---|---|
| **Webhook (Module 1)** | Create a new webhook, then paste its URL into your Whapi.cloud dashboard |
| **Google Sheets (Module 6)** | Connect your Google account and point to your new spreadsheet |
| **Gemini AI (Module 7)** | Add your Gemini API key as a new connection |
| **Gmail (Module 9)** | Connect your Google account for email sending |
| **HTTP Request (Module 2)** | Update the `Authorization` header with your Whapi.cloud Bearer token |

### Step 5 — Update Your Email Address

In **Module 9 (Send Email)**, change the recipient email to your own support address:

- Open Module 9
- Replace `your-email@gmail.com` in the **"To"** field
- Click **"OK"**

### Step 6 — Configure Whapi.cloud Webhook

1. Go to your [Whapi.cloud dashboard](https://whapi.cloud)
2. Navigate to **Settings → Webhooks**
3. Paste the webhook URL copied from Make.com (Module 1)
4. Set the trigger event to **"messages"**
5. Save

### Step 7 — Test the Bot

1. Turn **ON** your Make.com scenario (toggle at bottom-left)
2. Send a WhatsApp message to your connected number
3. Watch the scenario run in Make.com's execution history
4. You should receive an AI-powered reply within seconds 🎉

---

## ⚙️ Configuration

### System Prompt Customization (Module 7 — Gemini AI)

The AI's behaviour is controlled by the **system instruction** in Module 7. You can edit it to:

- Change the business name or type
- Adjust the tone and personality
- Add or remove rules
- Change the escalation phrase

**Default system prompt:**
```
You are a professional WhatsApp customer support assistant.

Below is our Pizza FAQ database. If a user's question matches one of these topics,
use the answer provided. If they just say 'Hi' or ask something else, be a friendly
pizza chef and help them generally.

FAQ Database:
{{14.text}}

Rules:
- Be concise and professional
- Reply in the same language as the customer
- Keep answers under 80 words
- Never mention being an AI
- Use the provided FAQ context if available
- If the answer is unclear, politely ask for clarification
- If customer sounds angry or asks for human support, say:
  "I'm forwarding your request to a human support agent."

Your Voice: Energetic, polite, and helpful. Use pizza emojis (🍕, 🫓, 🍞) occasionally.
```

### Environment Variables / Secrets to Replace

| Location | Field | What to Replace |
|---|---|---|
| Module 2 (HTTP) | `Authorization` header | Your Whapi.cloud Bearer token |
| Module 6 (Sheets) | `spreadsheetId` | Your Google Sheets ID |
| Module 9 (Gmail) | `to` | Your support email address |
| Module 7 (Gemini) | Connection | Your Gemini API key |

> ⚠️ **Security Note:** Never commit real API keys, tokens, or email addresses to a public repository. The blueprint JSON included in this repo has placeholder/sanitized values.

---

## 🔍 How It Works

### Message Flow Walkthrough

**Scenario: Customer asks "Do you have gluten-free pizza?"**

1. Whapi.cloud receives the WhatsApp message and fires a `POST` request to the Make.com webhook
2. The **Self-Message Filter** checks `from_me = false` — passes ✅
3. **Google Sheets** fetches all rows from `pizza_store_faq` — retrieves the FAQ list
4. **Text Aggregator** formats rows: `"Question: Do you have gluten-free options? | Answer: Yes, we offer gluten-free bases on request."`
5. **Gemini AI** receives:
   - User message: *"Do you have gluten-free pizza?"*
   - System prompt: includes the formatted FAQ + behaviour rules
   - Generates: *"Great news! 🍕 Yes, we do offer gluten-free bases on request. Just mention it when ordering and we'll take care of it!"*
6. **Router** checks — no escalation phrase detected → takes **Route B**
7. **HTTP Request** sends the reply to Whapi.cloud API → customer receives the message on WhatsApp

---

**Scenario: Customer says "I want to speak to a human"**

1–5. Same steps as above
6. **Router** takes **Route A** — Gemini's response contains the escalation phrase
7. **Gmail** sends an email to the support team with:
   - Customer's phone number
   - Original message body

---

## 📁 Project Structure

```
whatsapp-pizza-chatbot/
│
├── WHATSAPP_CHATBOT_blueprint.json   # Make.com importable scenario blueprint
├── README.md                          # This file
└── assets/
    └── flow-diagram.png               # (Optional) Screenshot of the Make.com flow
```

---

## 🎨 Customization Guide

### Adapting to a Different Business

This chatbot is built for a pizza store but can be adapted to any business in minutes:

1. **Update the Google Sheet** — Replace FAQ rows with your own Q&A pairs
2. **Edit the system prompt** — Change "pizza store" and the AI persona to match your brand
3. **Adjust the escalation trigger** — Modify the phrase that routes to human support
4. **Update the reply email** — Point Module 9 to your support team's inbox

### Adding More Modules

Make.com supports hundreds of app integrations. Consider extending this bot with:
- **Google Calendar** — Let customers book a table
- **Airtable / Notion** — Use a more powerful knowledge base
- **Slack** — Route escalations to a Slack channel instead of email
- **Stripe** — Handle order payments directly in chat

---

## ⚠️ Limitations

- **Make.com Free Plan**: Limited to 1,000 operations/month. High-volume stores should upgrade to a paid plan.
- **Gemini 2.5 Flash-Lite**: The model has a context window limit — extremely large FAQ sheets may need to be trimmed or chunked.
- **WhatsApp Policy**: Ensure your use case complies with [WhatsApp's Business Policy](https://www.whatsapp.com/legal/business-policy/). Automated messaging has restrictions.
- **Whapi.cloud**: A third-party WhatsApp gateway. Review their pricing and terms before going to production.
- **No Conversation Memory**: Each message is processed independently. The bot does not remember previous turns in the same conversation.

---

## ❓ FAQ

**Q: Can I use this without a Whapi.cloud account?**
> You'll need a WhatsApp API provider. Alternatives include [360dialog](https://360dialog.com) or the official [Meta Cloud API](https://developers.facebook.com/docs/whatsapp/cloud-api/), but you'd need to adjust the HTTP module's endpoint and headers accordingly.

**Q: The bot replies to its own messages — how do I fix it?**
> Check that the **Self-Message Filter** (Module 6) is properly connected and enabled. The condition `from_me ≠ true` must be active.

**Q: How do I update the FAQ without touching Make.com?**
> Just edit your Google Sheet directly. The bot reads it live on every message — no changes needed in Make.com.

**Q: Can I switch from Gemini to a different AI model?**
> Yes. Make.com has native integrations for OpenAI, Anthropic Claude, and others. Replace Module 7 with the relevant AI module and update the system prompt format.

**Q: Is this GDPR compliant?**
> You are responsible for ensuring compliance in your region. Avoid logging sensitive customer data, and review Make.com's and Google's data processing agreements.

---

## 🤝 Contributing

Contributions are welcome! Here's how:

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature-name`
3. Commit your changes: `git commit -m "feat: add your feature"`
4. Push to your branch: `git push origin feature/your-feature-name`
5. Open a Pull Request

Please open an issue first to discuss major changes.

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

<div align="center">

Built with ❤️ and 🍕 using [Make.com](https://make.com) · [Whapi.cloud](https://whapi.cloud) · [Google Gemini](https://deepmind.google/technologies/gemini/)

⭐ If this project helped you, please give it a star!

</div>

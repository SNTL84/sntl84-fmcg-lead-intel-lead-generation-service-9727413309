# 🧠 FMCG Lead Intelligence Engine

**By Milan · SNTL 84 · [desidevloper.com](https://desidevloper.com)**

> *Automate What's Costing You Money.*

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![n8n](https://img.shields.io/badge/Built%20with-n8n-orange)](https://n8n.io)
[![Claude AI](https://img.shields.io/badge/AI-Claude%20Sonnet-blueviolet)](https://console.anthropic.com)
[![FMCG](https://img.shields.io/badge/Domain-FMCG%20India-green)](#)

**Topics:** `n8n` · `automation` · `workflow-automation` · `claude-ai` · `anthropic` · `ai-automation` · `fmcg` · `lead-generation` · `sales-automation` · `google-sheets` · `whatsapp-api` · `gmail`

An **n8n workflow** that automates FMCG distributor/stockist outreach using Claude AI — built by someone who has actually done this work on the ground across Gujarat, Maharashtra, Rajasthan, and MP.

---

## 💸 The Problem This Solves

Manual FMCG lead outreach is expensive in time. A territory sales rep typically spends:
- **2–3 hours/day** identifying potential distributors from IndiaMART, Sulekha, trade fairs
- **1–2 hours/day** qualifying leads manually (right category? right size? competitor lock-in?)
- **30–60 min/day** writing personalised WhatsApp/email messages that actually get replies

That's **4–6 hours of high-cost human time per day** for work that is highly repetitive but requires FMCG domain knowledge to do well.

This workflow reduces that to **one n8n run per day** and a **10-minute human review**.

> If a territory sales rep costs ₹40,000/month, this workflow recovers ₹20,000+ of that cost in reclaimed time — every month.

---

## 🚀 What It Does

```
📥 Trigger (daily cron or webhook)
    ↓
🌐 Fetch leads from IndiaMART / trade directories
    ↓
📊 Deduplicate against existing Google Sheet
    ↓
🤖 Claude API: Score each lead 1–10 (FMCG territory logic baked in)
    ↓
🚦 Filter: score ≥ 7 → outreach queue / score < 7 → review queue
    ↓
🤖 Claude API: Draft personalised WhatsApp + email per lead
    ↓
💬 Send WhatsApp / 📧 Create Gmail drafts
    ↓
📊 Save enriched lead record to Google Sheet
    ↓
📩 Email run summary to Milan
```

---

## 💰 Monetization & Use Cases

This engine is **production-ready for three revenue models**:

### 1. 🏢 Done-For-You Agency
Sell this as a managed service to FMCG brands entering new territories.
> Retainer: ₹15,000–₹40,000/month per brand. One workflow, multiple clients.

### 2. 🛒 Sell the Workflow (Digital Product)
List on Gumroad, Lemon Squeezy, or direct. FMCG founders pay for plug-and-play n8n exports.
> One-time: ₹2,500–₹8,000 per sale. No support overhead if docs are solid.

### 3. 🤝 Territory Outreach-as-a-Service
Charge per qualified lead delivered — hot leads (score ≥ 8) at ₹500–₹1,500 each.
> 50 hot leads/month = ₹25,000–₹75,000 revenue per brand.

**Who buys this?**
- New FMCG brands entering Gujarat / Maharashtra / Rajasthan distribution
- Mid-size brands that need to expand channels without hiring 5 more reps
- Export houses looking to build India distribution networks fast
- Supply chain consultants who want to add an AI automation layer to their offering

---

## 🔥 Why This Is Different From Generic Lead Scrapers

Most lead gen tools give you a spreadsheet of names and phone numbers. This gives you:

1. **FMCG-native scoring** — Claude's scoring prompt is built with actual distributor channel knowledge: margin structures, territory tier logic, competitor lock-in patterns, seasonal signals. Not generic B2B scoring.

2. **Channel-aware messaging** — Different messages for Gujarat stockists (Hinglish WhatsApp) vs. Maharashtra metro distributors (formal email). Because that's how it actually works on the ground.

3. **Deduplication memory** — Doesn't re-contact leads you've already approached. Tracks full status in Google Sheets.

4. **Actionable output** — Every lead comes with a `wa_url` (click-to-send WhatsApp), Gmail draft, score rationale, and a follow-up note. Zero copy-pasting.

---

## 🛠 Tech Stack

| Component | Tool |
|-----------|------|
| Workflow orchestration | n8n (self-hosted or cloud) |
| AI scoring + messaging | Claude Sonnet 4 (Anthropic API) |
| Lead source | IndiaMART / Sulekha (HTTP node) |
| Lead storage | Google Sheets |
| Outreach — WhatsApp | WhatsApp Business API / Twilio |
| Outreach — Email | Gmail (n8n native) |
| Notifications | Gmail |

---

## 📁 Repository Structure

```
sntl84-fmcg-lead-intel/
├── workflow.json              ← Import directly into n8n
├── README.md
├── prompts/
│   ├── score-lead.txt         ← Claude scoring prompt (full system + user template)
│   └── draft-message.txt      ← Claude message drafting prompt (with examples)
├── sample-data/
│   └── leads-sample.csv       ← 10 demo leads to test the workflow
└── docs/
    └── setup.md               ← Google Sheets column setup + credential guide
```

---

## ⚡ Quick Start

### Prerequisites
- n8n instance (cloud or self-hosted)
- Anthropic API key ([console.anthropic.com](https://console.anthropic.com))
- Google account (for Sheets + Gmail)
- WhatsApp Business API account (or Twilio — optional)

### Setup in 15 Minutes

**1. Import the workflow**
```
n8n → Workflows → Import → upload workflow.json
```

**2. Set up Google Sheet**  
Follow `docs/setup.md` for exact column headers.

**3. Add credentials in n8n**
- `Anthropic API` — paste your API key
- `Google Sheets OAuth2` — follow the OAuth flow
- `Gmail OAuth2` — same Google account
- `WhatsApp Business API` — add credentials (or skip, use Gmail only)

**4. Update Sheet IDs**  
Replace all 3 instances of `YOUR_GOOGLE_SHEET_ID_HERE` in `workflow.json` with your actual Google Sheet ID.

**5. Update your notification email**  
In the `📩 Notify Milan` node, update `sendTo` to your address.

**6. Test with sample data**  
Load `sample-data/leads-sample.csv` and run manually. Check the n8n execution log.

**7. Activate**  
Toggle ON. Runs daily. Review in 10 minutes.

---

## 🧪 Sample Output

**Input lead:**
```json
{
  "company_name": "Rajesh Beverages Distributors",
  "city": "Surat", "state": "Gujarat",
  "years_in_business": 8,
  "annual_turnover": "50-100 Cr",
  "brands_currently_carried": ["Coca-Cola", "Pepsi", "Sting"]
}
```

**Claude score output:**
```json
{
  "score": 8,
  "tier": "hot",
  "score_rationale": "Large, established Surat distributor with 8 years experience and proven beverage category expertise. Turnover range confirms serious operation.",
  "opportunity_angle": "Home market launch partner — Exiito from Surat, Rajesh based in Surat. Local brand story is powerful.",
  "risk_flag": "Carries Coca-Cola and Pepsi — confirm no exclusivity clause before deep pitch",
  "best_channel": "whatsapp",
  "suggested_time": "Monday or Tuesday 10–11am before market rounds"
}
```

**Claude WhatsApp draft:**
```
Hi Rajesh bhai, Milan here from Exiito Beverages — Surat wale.
Aapka Surat mein beverage distribution dekha, bahut solid operation hai.

Hum ek nayi energy drink brand launch kar rahe hain Gujarat mein,
aur Surat ke liye ek experienced partner dhundh rahe hain jo territory own kare.

Exiito ek alag segment hai (energy/functional) — conflict nahi hoga,
bas ek new revenue stream add hogi.

Kya 15 minutes ke liye call possible hai is week?
```

---

## 🔧 Customising the AI Prompts

**To score for a different brand or category:**  
Edit `prompts/score-lead.txt` → update brand name, target margin, territory priorities, and scoring criteria.

**To change the message tone:**  
Edit `prompts/draft-message.txt` → update voice, brand story, and example outputs.

**To add a new territory:**  
In `score-lead.txt`, update the CHANNEL LOGIC section with the new state and its communication preferences.

---

## 👤 Built By

**Milan · SNTL 84**  
AI Workflow Developer · Ex-FMCG Territory Sales · Surat, India  
*AI Systems | Full-Stack Builds | Supply Chain Business Intelligence*  
*Built for founders who move fast and waste nothing.*

🌐 **Website:** [desidevloper.com](https://desidevloper.com)  
💬 **WhatsApp:** [+91 97274 13309](https://wa.me/919727413309)  
🔗 **LinkedIn:** [linkedin.com/in/sntl2784](https://www.linkedin.com/in/sntl2784)  
💻 **GitHub:** [@SNTL84](https://github.com/SNTL84)

*This tool is used live for FMCG distributor outreach across Gujarat and Maharashtra.*

---

## ⭐ If This Saves You Time

Star the repo. Share it with a founder who's wasting money on manual outreach. Fork it for your own brand/category.

If you build something useful on top of this — tag me. I want to see it.

---

## 📄 License

MIT — fork it, adapt it, use it for your own brand.

---

*Part of the SNTL84 automation portfolio. "Automate What's Costing You Money."*

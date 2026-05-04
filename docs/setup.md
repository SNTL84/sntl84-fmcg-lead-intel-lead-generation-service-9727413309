# Google Sheets Setup Guide
# FMCG Lead Intelligence Engine — SNTL84

---

## Step 1: Create Your Google Sheet

1. Go to sheets.google.com → New spreadsheet
2. Name it: **FMCG Lead Intel — Exiito**
3. Create two sheets (tabs):
   - `Leads` — all processed leads
   - `Review Queue` — leads scoring below 7

---

## Step 2: Set Up "Leads" Sheet Columns

Create these exact column headers in Row 1:

| Column | Header |
|--------|--------|
| A | company_name |
| B | contact_name |
| C | phone |
| D | email |
| E | city |
| F | state |
| G | category |
| H | sub_category |
| I | annual_turnover |
| J | years_in_business |
| K | stockist_or_distributor |
| L | brands_currently_carried |
| M | score |
| N | tier |
| O | score_rationale |
| P | opportunity_angle |
| Q | risk_flag |
| R | best_channel |
| S | whatsapp_message |
| T | email_subject |
| U | email_body |
| V | follow_up_note |
| W | wa_url |
| X | status |
| Y | created_at |
| Z | sent_at |
| AA | reply_received |

---

## Step 3: Get Your Sheet ID

Your Google Sheet URL looks like:
`https://docs.google.com/spreadsheets/d/YOUR_SHEET_ID_HERE/edit`

Copy the ID (the long string between `/d/` and `/edit`).

---

## Step 4: Update workflow.json

Search for `YOUR_GOOGLE_SHEET_ID_HERE` in workflow.json and replace all 3 occurrences with your actual Sheet ID.

---

## Step 5: Connect Google Sheets in n8n

1. In n8n, go to **Settings → Credentials**
2. Add **Google Sheets OAuth2** credentials
3. Follow the Google OAuth flow
4. Name the credential: `Google Sheets — SNTL84`
5. In each Google Sheets node in the workflow, select this credential

---

## Useful Sheet Formulas

### Color-code by score tier (conditional formatting)
Apply to column M (score):
- Green: value ≥ 7
- Yellow: value 5–6
- Red: value ≤ 4

### Count hot leads today
```
=COUNTIFS(N:N,"hot",Y:Y,">="&TODAY())
```

### Filter unsent hot leads
Create a filter view: `status = ready_to_send` AND `tier = hot`

---

## Pivot Table Suggestion

Create a pivot table on a 3rd sheet:
- Rows: state
- Columns: tier
- Values: COUNT of company_name

This gives you your territory heat map at a glance.

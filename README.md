# Pricing_Agent_N8N

## Overview
This project automates the pricing process for B2B RFP responses using an n8n workflow.

It processes RFP inputs, matches recommended SKUs with pricing data, computes test costs, and generates a structured pricing summary.

---

## Workflow Features
- Webhook-triggered RFP ingestion  
- SKU-based product pricing using Google Sheets data  
- Test cost computation  
- Selection of highest spec-match SKU for final material cost  
- Structured JSON output generation  

---

## How to Use

### 1. Start n8n (Docker)
```bash
docker run -it --rm -p 5678:5678 n8nio/n8n
```

Then open:

```
http://localhost:5678
```

---

### 2. Import Workflow

* Open n8n UI
* Click **Import**
* Upload `workflow.json`

---
## 3. Google Sheets Requirement (MANDATORY)

This workflow uses a pre-configured Google Sheet for pricing data.

To run the workflow successfully:

- You MUST sign in using Google OAuth2 in n8n  
- You MUST have access to the configured Google Sheet  

If you do not have access, the workflow will fail with an authentication error (EAUTH).

---

## 4. Setup Google Authentication

- Go to **n8n → Credentials**  
- Create new credential:  
  - **Google Sheets OAuth2 API**  
- Click **Connect OAuth2 Account**  
- Sign in with your Google account and allow permissions  

Then:

- Open both nodes:  
  - `Extract Test Prices`  
  - `Extract product Prices`  
- Select the newly created credential  

---

## 5. (Optional) Use Your Own Google Sheet

If you want to use your own data:

1. Open your Google Sheet  
2. Copy the Sheet ID from the URL:
```
 https://docs.google.com/spreadsheets/d/<SHEET_ID>/edit
```

3. Replace the Sheet ID in both Google Sheets nodes  

4. Ensure sheet names match exactly:
   - `Product_Prices`  
   - `Test_Prices`
     
---

### 6. Execute Workflow (Test Mode)

* Click **Execute Workflow**
* Ensure the Webhook node is active

---

### 7. Trigger the Webhook

```bash
curl -X POST http://localhost:5678/webhook-test/pricing-job ^
  -H "Content-Type: application/json" ^
  -d @sample_input.json
```

---

## Sample Input

Refer to `sample_input.json` for the request payload structure.

---

## Output

The workflow returns a structured JSON response containing:

* Product pricing (all recommended SKUs)
* Selected SKU (highest spec-match) for cost aggregation
* Test pricing
* Total material, test, and overall project cost

---

## Tech Stack

* n8n
* Google Sheets (pricing data source)

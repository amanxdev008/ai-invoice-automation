# AI-Powered Invoice Processing & Alert System

AI-powered n8n workflow that automates invoice processing — extracts data from PDFs using GPT-4o-mini, validates it, logs it to Google Sheets, and sends email notifications, with automatic manual-review alerts for incomplete invoices.

---

## 📌 The Problem

Small businesses that receive invoices by email or upload often process them manually — someone has to open each PDF, type the details into a spreadsheet, and notify the accounts team. This is slow, repetitive, and prone to human error, especially when a business receives dozens of invoices a month.

## 💡 The Solution

I built an automated workflow (using **n8n + OpenAI**) that:

- 📂 **Watches** a Google Drive folder for new invoice PDFs
- 🤖 **Extracts** key details (invoice number, vendor, dates, amounts) automatically using AI
- 📊 **Logs** every valid invoice into a Google Sheet as a permanent record
- 📧 **Sends** an automatic confirmation email to the billing team
- ✅ **Validates** the data before logging it — if a required field (like invoice number or total amount) is missing, the system skips the spreadsheet and instead sends an alert email flagging the file for manual review, so no incomplete or incorrect data ever enters the business's records

## 🎯 The Result

A hands-off system that turns a manual, error-prone task into an automatic one — while still catching problem invoices instead of silently logging bad data. Tested against both complete and incomplete invoice formats to confirm reliable behavior in both cases.

**Tools used:** n8n, OpenAI (GPT-4o-mini), Google Drive, Google Sheets, Gmail

---

## 🎥 Demo Video

[![Watch the demo](screenshot-workflow-execution.png)](https://youtu.be/g7x9yuP7bNI?si=EWnZjn19mTkokEXh)

*(Click the image above to watch the full walkthrough on YouTube)*

## 🖼️ Screenshots

### Workflow Execution
![Workflow running successfully](screenshot-workflow-execution.png)

### AI-Extracted Invoice Data
![Extracted invoice output](screenshot-invoice-data.png)

---

## ⚙️ How It Works

1. **Google Drive Trigger** — Polls a specific Drive folder every minute for newly uploaded invoice files.
2. **Download File** — Pulls the new file from Drive.
3. **Extract from File** — Extracts raw text from the invoice PDF.
4. **Information Extractor (AI, GPT-4o-mini)** — Reads the extracted text and pulls out structured fields:
   - Vendor name
   - Invoice number
   - Invoice date
   - Due date
   - Bill-to name
   - Subtotal
   - Tax amount
   - Total amount
5. **IF Node (Validation)** — Checks whether the invoice number and total amount were actually found.
   - ❌ **Missing data** → sends a "Manual Review Needed" email flagging the file for a human to check.
   - ✅ **Data complete** → proceeds to log and notify.
6. **Append Row in Google Sheets** — Saves the structured invoice data to a tracking spreadsheet.
7. **Send Email** — Notifies the billing team with a formatted summary of the processed invoice.

## 🧩 Tech Stack

- **n8n** — workflow orchestration
- **Google Drive** — invoice intake (trigger + file download)
- **OpenAI (GPT-4o-mini)** — AI-powered data extraction via LangChain Information Extractor node
- **Google Sheets** — structured data storage / invoice log
- **Gmail** — automated notifications (success + failure alerts)

## 🚀 Setup

1. Import `AI_Invoice_Processing_Management.json` into your n8n instance.
2. Connect your own credentials for:
   - Google Drive OAuth2
   - Google Sheets OAuth2
   - Gmail OAuth2
   - OpenAI API
3. Update these placeholders with your own values:
   - `YOUR_FOLDER_ID` — the Google Drive folder to watch for invoices
   - `YOUR_SHEET_ID` — the Google Sheet where extracted data will be logged
   - `your-email@example.com` — the notification recipient
4. Activate the workflow.

## 🔒 Note

Credential IDs, folder IDs, sheet IDs, and personal email addresses have been removed from the shared workflow JSON. Replace the placeholders with your own before importing.

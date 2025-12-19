# 📄 Automated Invoice Page Creation & QuickBooks Integration (n8n)

<p align="center"><img src="https://github.com/Aryasarkar008/Invoice-Page-Creation-with-n8n-QuickBooks-integration/blob/main/full%20flow.png?raw=true" alt="project-image"></p>

## 🚀 Overview

This project implements a **fully automated invoice generation pipeline** using **n8n**, integrating Gmail, AI-based timesheet parsing, Google Sheets, Google Drive, and QuickBooks Online.

The workflow automatically converts emailed timesheets into **month-wise invoice-ready records**, organizes them in Google Drive, and creates invoices in QuickBooks — including support for **Customer Account Numbers** and **PO Numbers** using custom API-based fields.

---

## 🧩 Key Features

- 📧 Email-driven automation (Gmail trigger)
- 📄 PDF timesheet processing
- 🤖 AI-powered month-wise timesheet parsing
- 📊 Invoice staging in Google Sheets
- 📁 Automatic Google Drive folder creation
- 💼 QuickBooks customer lookup & creation
- 🧾 QuickBooks invoice creation with line items
- 🏷️ Custom fields support (Customer Account No. & PO Number)
- 🔁 Handles multi-attachment & multi-month timesheets
- 🛡️ Duplicate prevention & error-safe logic

---

## 🏗️ High-Level Architecture

---

## 🔁 Workflow Trigger

### 📩 Gmail Trigger
- Watches for incoming emails containing timesheets
- Automatically downloads all PDF attachments
- Supports multiple attachments in a single email

---

## 📎 Attachment Handling

### Split Binary Attachments
- Converts multiple email attachments into individual workflow items
- Ensures each timesheet is processed independently
- Prevents data mixing across files

---

## 📄 Text Extraction

- Converts PDF timesheets into raw text using an external API
- Supports multi-page documents
- Output is passed to the AI Agent for structured parsing

---

## 🤖 AI-Powered Timesheet Parsing

### AI Agent (LangChain / Gemini)

The AI agent:
- Extracts **only billable hours**
- Splits data **month-by-month**
- Handles **weeks spanning multiple months**
- Outputs **strict JSON only**

#### Extracted Fields:
- Employee Name
- Client Name
- Month
- Year
- Week Starting Date
- Week Ending Date
- Total Hours

---

## 🗂️ Monthly Processing Logic

### Prepare Month Array
- Converts AI output into structured month objects
- Identifies first and subsequent months for invoice creation

### Split Each Month
- Each month is processed independently
- Allows a single timesheet to generate multiple invoice entries

---

## 📊 Google Sheets – Invoice Staging

### Sheet Naming Convention

Example:

### Invoice Columns
- Customer Account Number
- Invoice Date
- Due Date
- PO Number
- Item Name
- Quantity (Hours)
- Unit Price
- Description

### Duplicate Prevention
- Checks if an invoice row already exists
- Deletes and replaces only when required
- Prevents duplicate billing

---

## 📁 Google Drive Folder Structure

Automatically creates and maintains the following structure:


Folder creation is **idempotent**, making the workflow safe to re-run.

---

## 🧾 QuickBooks Integration

### Customer Lookup
- Searches QuickBooks using **DisplayName**
- Matches against client names from Google Sheets

### Conditional Logic
- **Customer exists** → Create Invoice
- **Customer does not exist** → Create Customer → Create Invoice

---

## 👤 QuickBooks Customer Creation

Customer records are created using:
- Display Name
- Company Name
- Email (if available)

This ensures consistency with QuickBooks’ internal customer model.

---

## 🧮 QuickBooks Invoice Creation

### Invoice Line Items
- Description → From Google Sheets
- Quantity → Billable Hours
- Unit Price → Configured rate
- Amount → Auto-calculated

### Custom Fields (via API)
- **Customer Account Number**
- **PO Number**

These fields are injected using QuickBooks CustomField API support.

---

## 🛡️ Error Handling & Safeguards

- Skips zero-hour months
- Handles year boundary edge cases
- Prevents duplicate invoice creation
- Supports multi-file and multi-month inputs
- Uses wait nodes to avoid API throttling

---

## ⚙️ Configuration Requirements

### Required Credentials
- Gmail OAuth2
- Google Sheets OAuth2
- Google Drive OAuth2
- QuickBooks Online OAuth2
- Google Gemini API Key

---

## 🧪 Testing Recommendations

- Use pinned AI Agent outputs for dry runs
- Test with QuickBooks Sandbox company
- Enable execution logs for debugging
- Start with manual execution before enabling triggers

---

## 📌 Future Enhancements

- Multi-line invoices
- Tax & discount handling
- Automatic invoice emailing
- Client-specific rate configuration
- Slack / Email error notifications

---

## 📂 Workflow File

The complete workflow can be imported directly into n8n using the provided JSON file.

---

## 👨‍💻 Author

**Arya Sarkar**  
Automation • n8n • AI workflows • Accounting integrations


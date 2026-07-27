# 🏦 Universal Banking Mini Statement PDF Generator

## 📌 Project Overview

The **Universal Banking Mini Statement PDF Generator** is a MuleSoft application that automatically generates monthly mini statements for active bank customers.

The application runs on a scheduler, retrieves customer details and transaction history from Snowflake, generates a professional HTML bank statement, converts it into a PDF using the PDFShift API, stores the PDF locally, emails it to the customer, and finally updates the database to indicate that the statement has been sent.

---

# Features

- Automatic scheduler execution every 5 minutes
- Fetch active customers from Snowflake
- Generate professional HTML bank statement
- Convert HTML to PDF using PDFShift REST API
- Save PDF into local directory
- Send PDF through Gmail SMTP
- Update EMAIL_SENT status in Snowflake
- Detailed logging throughout the flow

---

# Architecture

```
Scheduler
      │
      ▼
Snowflake Database
      │
      ▼
For Each Customer
      │
      ├── Build HTML Statement
      │
      ├── Convert HTML → PDF (PDFShift)
      │
      ├── Save PDF locally
      │
      ├── Send Email
      │
      └── Update EMAIL_SENT Flag
```

---

# Technologies Used

| Technology | Version |
|------------|---------|
| Mule Runtime | Mule 4 |
| DataWeave | 2.0 |
| Snowflake | Database |
| Gmail SMTP | Email Service |
| PDFShift | HTML to PDF |
| File Connector | Local Storage |
| HTTP Connector | REST Calls |

---

# Connectors Used

- Scheduler
- Snowflake Connector
- HTTP Connector
- File Connector
- Email Connector
- DataWeave
- Logger

---

# Flow Name

```
universal-banking-ministatement-pdf-generate-flow
```

---

# Flow Execution

## Step 1

Scheduler starts every **5 minutes**.

```xml
<fixed-frequency frequency="5" timeUnit="MINUTES"/>
```

---

## Step 2

Retrieve active customers.

```sql
SELECT *
FROM BANK_CUSTOMER_DATA
WHERE STATUS='ACTIVE';
```

---

## Step 3

Loop through every customer using For Each.

---

## Step 4

Generate HTML Mini Statement.

The HTML contains:

- Customer Name
- Account Number
- Statement Month
- Generated Date
- Bank Name
- Balance
- Transaction Table
- Total Withdraw Amount
- Remaining Balance

---

## Step 5

Convert HTML into PDF.

REST Endpoint

```
POST https://api.pdfshift.io/v3/convert/pdf
```

Headers

```
X-API-Key
Content-Type : application/json
```

Request Body

```json
{
   "source":"<HTML>"
}
```

---

## Step 6

Save generated PDF.

Example Path

```
Desktop/ministatement pdfs/
```

Generated filename

```
CustomerName-UUID.pdf
```

---

## Step 7

Send Email

SMTP Server

```
smtp.gmail.com
```

Port

```
587
```

Email includes

- Professional HTML body
- Monthly Statement PDF attachment

---

## Step 8

Update customer record.

```sql
UPDATE BANK_CUSTOMER_DATA
SET EMAIL_SENT='TRUE'
WHERE ACCOUNTNUMBER=:accountNumber;
```

---

# Email Template

Customer receives

- Greeting
- Monthly Statement Information
- PDF Attachment
- Customer Support Details

---

# PDF Contents

The generated PDF includes

- Bank Header
- Customer Information
- Statement Month
- Account Status
- Available Balance
- Transaction History
- Account Summary
- Footer
- Customer Care Information

---

# Folder Structure

```
src
 ├── main
 │    ├── mule
 │    │      banking-flow.xml
 │    ├── resources
 │    │      log4j2.xml
 │    └── api
 │
 └── pom.xml
```

---

# Database

## BANK_CUSTOMER_DATA

Sample fields

- ACCOUNTNUMBER
- FULLNAME
- EMAIL
- BANKNAME
- BALANCE
- STATUS
- TRANSACTIONS
- EMAIL_SENT

---

# Configuration

## Snowflake

Configure

- Account
- Warehouse
- Database
- Schema
- Username
- Password

---

## SMTP

Configure

- Gmail Username
- App Password

---

## PDFShift

Configure

```
X-API-Key
```

---

# Logging

The application logs

- Scheduler Start
- Customer Processing
- HTML Generation
- PDF Generation
- Email Status
- Database Update
- Flow Completion

---

# Error Handling

Recommended improvements

- Retry PDF API failures
- Retry Email failures
- Global Error Handler
- Store failed customers
- Dead Letter Queue
- Notification alerts

---

# Future Enhancements

- Store PDFs in AWS S3
- Azure Blob Storage integration
- Password-protected PDFs
- Digital Signature
- Customer Portal Download
- SMS Notification
- Email Templates from external file
- Scheduler parameterization
- CloudHub deployment

---

# Project Workflow

```
Scheduler
      │
      ▼
Fetch Active Customers
      │
      ▼
For Each Customer
      │
      ▼
Generate HTML
      │
      ▼
Convert to PDF
      │
      ▼
Save PDF
      │
      ▼
Send Email
      │
      ▼
Update EMAIL_SENT
      │
      ▼
Complete
```

---

# Author

**Narsingarao Beesetti**

**Role:** MuleSoft Developer

**Technology Stack**

- MuleSoft
- DataWeave
- Snowflake
- REST APIs
- SMTP
- PDFShift
- Git
- Maven

---

# License

This project is intended for learning and demonstration purposes.

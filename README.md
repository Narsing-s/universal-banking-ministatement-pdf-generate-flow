# 🏦 Universal Banking Mini Statement PDF Generator

## 📌 Project Overview

The **Universal Banking Mini Statement PDF Generator** is a MuleSoft application that automatically generates monthly mini statements for active bank customers.

The application runs on a scheduler, retrieves customer details and transaction history from Snowflake, generates a professional HTML bank statement, converts it into a PDF using the PDFShift API, stores the PDF locally, emails it to the customer, and finally updates the database to indicate that the statement has been sent.

---

# Features

- Automatic scheduler execution every 5 minutes
- <img width="1171" height="755" alt="image" src="https://github.com/user-attachments/assets/d8db2005-44ad-42eb-b038-2a2f0a52a0a2" />
- Fetch active customers from Snowflake
- <img width="1287" height="827" alt="image" src="https://github.com/user-attachments/assets/14efcbc1-bc3e-4bb3-8ae7-af78cc6801d9" />
- Generate professional HTML bank statement
- <img width="471" height="686" alt="image" src="https://github.com/user-attachments/assets/513b5f96-17a3-4737-8c5d-f8ace1399355" />
- Convert HTML to PDF using PDFShift REST API
- <img width="1800" height="827" alt="image" src="https://github.com/user-attachments/assets/4b42d3ba-f46d-45a5-a6e7-203c21e965be" />
- Save PDF into local directory
- <img width="1072" height="377" alt="image" src="https://github.com/user-attachments/assets/fbc2a458-9d0e-4f59-9e2e-e267e734714b" />
- Send PDF through Gmail SMTP
- <img width="1660" height="932" alt="image" src="https://github.com/user-attachments/assets/d407b94f-3686-48f5-9f10-9e132e9ecc28" />
- Update EMAIL_SENT status in Snowflake
- <img width="917" height="177" alt="image" src="https://github.com/user-attachments/assets/13b12607-5e42-4dfe-a753-5b97bf01a9b1" />

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

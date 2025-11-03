# Project 03 – Contact Form with Lambda + API Gateway + SES

## Overview
This project adds a live contact form to my AWS S3-hosted resume website.  
The form lets users send messages directly to my email address via **API Gateway**, **Lambda**, and **Amazon SES**.

**Live Frontend:**  
http://resume-maruwathomas.s3-website-us-east-1.amazonaws.com/contact.html

**Backend API Endpoint:**  
https://zyzn9avvkl.execute-api.us-east-1.amazonaws.com/prod/contact

---

## Key AWS Services
- **S3:** Static website hosting for frontend pages.  
- **API Gateway:** Creates a POST endpoint `/contact` for Lambda.  
- **Lambda:** Sends email messages using SES.  
- **SES:** Handles verified sender and email delivery.  
- **IAM:** Grants Lambda permission to use `ses:SendEmail`.

---

## How It Works
1. User fills out the contact form on `contact.html`.  
2. JavaScript sends form data (name, email, message) to API Gateway.  
3. API Gateway triggers the Lambda function.  
4. Lambda uses SES to send the email.  
5. The user sees “Message sent successfully!” in the browser.

---

## Folder Structure

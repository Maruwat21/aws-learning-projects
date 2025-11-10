# Luxe VIP Express — Technical Handover Summary

## 📘 Project Overview
The **Luxe VIP Express** website was designed and built as a serverless AWS-hosted web application to promote and manage bookings for luxury transportation services (Sprinter Van and Black SUV).  
The system includes online payment integration through **Stripe Checkout**, serverless backend email notifications via **AWS Lambda**, **API Gateway**, and **Simple Email Service (SES)**.

---

## 🧩 Architecture Overview
This project leverages **AWS Serverless architecture**, ensuring scalability, low cost, and zero maintenance.

### **Key AWS Services Used**
| AWS Service | Purpose |
|--------------|----------|
| **Amazon S3** | Hosts the static website (`index.html`, `styles.css`, images). |
| **Amazon API Gateway** | Provides a secure REST endpoint `/contact` for booking submissions. |
| **AWS Lambda** | Processes incoming booking requests and triggers SES to send notification emails. |
| **Amazon SES (Simple Email Service)** | Sends booking confirmation emails to the admin address. |
| **IAM** | Defines roles and permissions for Lambda to access SES securely. |
| **Amazon Route 53 (Optional)** | Can be used later for custom domain mapping (`luxevipexpress.com`). |

---

## 🏗️ System Flow
1. **Visitor** fills out the booking form on the website.
2. **API Gateway** receives the JSON payload and routes it to the **Lambda function**.
3. **Lambda (sendContactEmail)** uses **SES** to email the booking details to the admin.
4. **Stripe Checkout** is used for processing payments (Sprinter Van – $125/hr, SUV – $100/hr).
5. **S3 Static Website Hosting** serves the front-end HTML/CSS from `https://luxe-vip-express-site.s3-website-us-east-1.amazonaws.com/`.

---

## 🧭 File Directory Summary


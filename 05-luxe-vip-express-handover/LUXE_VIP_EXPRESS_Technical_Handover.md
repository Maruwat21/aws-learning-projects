LUXE VIP EXPRESS – Website Technical Handover

Prepared by:
Maruwa Thomas – AWS Cloud Engineer
Region: us-east-1 (N. Virginia)

Project: LUXE VIP EXPRESS marketing & booking site (Sprinter van + black SUV)
Current environment: Proof-of-concept / demo (hosted in Maruwa’s AWS account)

1. Project Overview
1.1 Business Goal

LUXE VIP EXPRESS needs a simple, professional online presence where potential customers can:

View services, hourly rates, and vehicle types (Sprinter van, black SUV)

Submit booking requests via a contact form

Optionally proceed to Stripe Checkout for online pre-payment (test mode today)

This solution is designed as a serverless, low-maintenance architecture using AWS managed services, with minimal operational overhead and clear upgrade paths.

1.2 Scope of Delivery

Static marketing site with:

Hero section (branding, phone, email)

Fleet & rates section (Sprinter van, black SUV)

Service highlights

Booking request form (name, email, phone, vehicle type, date, time, trip details)

Backend email notification pipeline for booking requests:

API Gateway → Lambda → Amazon SES → inbox

Stripe test-mode checkout links for:

Sprinter Van – USD $125/hour

Black SUV – USD $100/hour

No custom domain or HTTPS (CloudFront) in this phase — the site uses the S3 static website endpoint for demo and validation.

2. High-Level Architecture
2.1 Components

Amazon S3

Bucket: luxe-vip-express-site

Purpose: Static website hosting (index.html, styles.css, image assets)

Public-read access via S3 static website endpoint.

Amazon API Gateway (HTTP API)

API name: ContactFormAPI

Route: POST /contact

Endpoint:
https://zyzn9avvkl.execute-api.us-east-1.amazonaws.com/prod/contact

Purpose: Accept HTTP POST requests from the booking form and trigger Lambda.

AWS Lambda

Function name: sendContactEmail

Runtime: Python 3.x

Purpose: Parse booking form payload, build email body, send email via SES, and return CORS-enabled responses for browser compatibility.

Amazon SES (Simple Email Service)

Region: us-east-1

From address: maruwathomas@gmail.com (verified)

To address: maruwathomas@gmail.com (same for sandbox/demo)

Purpose: Deliver booking email notifications.

Stripe (external SaaS)

Mode: Test mode

Payment links:

Sprinter Van: https://buy.stripe.com/test_bJe4gzg6ubI19XyesA8EM00

Black SUV: https://buy.stripe.com/test_cNi8wP8E2dQ9b1C0BK8EM01

Purpose: Demonstrate online payment flow; can be swapped to live links later.

2.2 Data Flow (Request a Booking)

User opens the site in their browser using the S3 website endpoint.

User fills out the Request a Booking form and clicks Submit.

Front-end JavaScript sends a POST request with JSON body to:
https://zyzn9avvkl.execute-api.us-east-1.amazonaws.com/prod/contact

API Gateway forwards the JSON body to the sendContactEmail Lambda function.

Lambda:

Parses name, email, phone, vehicleType, date, time, details

Builds a text email

Sends it via Amazon SES to TO_EMAIL

SES delivers the email to the mailbox (inbox or spam, depending on the provider).

Lambda returns a JSON response:
{"message": "Message sent successfully!"} with CORS headers.

Browser shows a green success message on the website and resets the form.

2.3 Architecture Diagram (for Excalidraw)

When building this in Excalidraw with AWS icons, use:

Client:

User / Browser icon

Frontend:

Amazon S3 icon labeled:
Amazon S3 – Static Website
(luxe-vip-express-site)

API Layer:

Amazon API Gateway (HTTP API) icon labeled:
API Gateway – ContactFormAPI (POST /contact)

Compute:

AWS Lambda icon labeled:
Lambda – sendContactEmail (Python)

Email:

Amazon SES icon labeled:
SES – Booking Notifications

External:

Generic external service icon labeled:
Stripe – Test Checkout Links

Flow arrows:

Browser → S3

Browser → API Gateway (POST /contact)

API Gateway → Lambda

Lambda → SES

SES → Email Inbox

Browser → Stripe (for pay buttons)

3. Deployment Details
3.1 Frontend

Bucket: luxe-vip-express-site

Region: us-east-1

Static Website Hosting: Enabled

Index document: index.html

Key files:

index.html – HTML structure, booking form, Stripe buttons

styles.css – Layout, colors, responsiveness

assets/ – Images (background, Sprinter van, SUV, etc.)

To update the site:

Edit index.html and/or styles.css locally.

Upload the changed files to S3 → luxe-vip-express-site (overwrite).

Open the website endpoint URL and hard-refresh.

3.2 Backend – Lambda

Function name: sendContactEmail

Runtime: Python 3.x

Key logic:

Handles CORS preflight (OPTIONS) requests

Parses JSON body from event["body"]

Builds email text from booking fields

Calls ses.send_email(...)

Returns JSON response with full CORS headers:

Access-Control-Allow-Origin: *

Access-Control-Allow-Headers: Content-Type

Access-Control-Allow-Methods: OPTIONS,POST

3.3 Backend – API Gateway

Type: HTTP API

Stage: prod

Route: POST /contact

Integration: Lambda proxy integration to sendContactEmail

CORS:

Allows origin *

Methods: OPTIONS, POST

Headers: Content-Type

4. Email System Configuration (SES)

SES Region: us-east-1

Mode: Sandbox (during demo/proof-of-concept)

Verified identity: maruwathomas@gmail.com (both sender and recipient)

In SES sandbox:

Only verified email addresses can receive messages.

This is fine for demo/testing, but production will require:

Moving SES account out of sandbox (Request production access)

Or verifying client’s email/domain.

5. Costs & Performance
5.1 Current Estimated Costs (in Maruwa’s account)

Amazon S3 (static hosting):

Very low traffic, small files → typically a few cents/month (often covered by Free Tier).

API Gateway (HTTP API):

Priced per million requests; this use case is extremely low volume.

Likely well under $1/month for a small transportation business.

AWS Lambda:

Charged by request and compute time; this function is short-lived (millisecond-level).

Under Free Tier for most demo / low-traffic usage.

Amazon SES:

Very low volume, text-only transactional emails.

May be covered by Free Tier or only a few cents.

Stripe:

No AWS cost; Stripe charges per successful live transaction (not relevant in test mode).

5.2 Performance Characteristics

No servers or EC2 instances needed.

Serverless components scale automatically with demand.

Regional services in us-east-1 keep latency low for US customers.

6. Security & Access
6.1 Public access

S3 website: Public read access for website files only.

API Gateway endpoint: Publicly callable URL (needed so the browser can call it).

6.2 IAM & Permissions

Lambda execution role must include:

ses:SendEmail permission for the configured identity.

SES is restricted to the configured account and region.

6.3 SES & Email Safety

Emails are plain-text transactional messages (booking requests).

No mass marketing / bulk sending implemented.

7. Maintenance & Updates
7.1 Updating Website Content

Common updates:

New hourly rates

New vehicle photos

A new service bullet (e.g., “Concert shuttles”)

Process:

Edit index.html or styles.css locally.

Upload updated file(s) to S3 bucket luxe-vip-express-site.

Test load via S3 website URL.

Submit a test booking to confirm form → email still works.

7.2 Updating Stripe Payment Links

When client is ready for real payments:

Create new live-mode payment links in Stripe for:

Sprinter Van hourly rate

Black SUV hourly rate

Replace the two href URLs in index.html under:

“Book Sprinter Van”

“Book Black SUV”

Re-upload index.html to S3.

8. Return Point – When Client Agrees to Buy

When LUXE VIP EXPRESS confirms that they want to adopt the site as their own:

Step 1 – Decide on Account Ownership

Two options:

Full transfer to client’s AWS account (recommended for long-term):

Client creates their own AWS account.

Resources are recreated there:

S3 bucket (with same files)

API Gateway HTTP API

Lambda sendContactEmail

SES identity & production access (with their email/domain)

Managed by Maruwa (consultant-hosted):

Site continues to run in Maruwa’s AWS account.

Client pays a monthly/annual hosting & maintenance fee.

Step 2 – Domain & Routing

Purchase luxevipexpress.com (if available) via:

Route 53, or

Third-party registrar pointing to Route 53.

Create Route 53 hosted zone for the domain.

For “quick and simple”:

Route 53 A/AAAA record → S3 website endpoint

For “production-grade”:

Add CloudFront distribution in front of S3 (HTTPS, caching)

Route 53 DNS → CloudFront

Attach ACM SSL certificate for the domain.

Step 3 – SES Production & Client Email

Move SES out of sandbox:

SES Console → Request production access

Configure:

Verified sender: e.g., booking@luxevipexpress.com

Recipient(s): dispatch/office email

Step 4 – Stripe Live Mode

Switch Stripe account to live mode.

Create Live payment links for:

Sprinter Van $125/hr

Black SUV $100/hr

Replace test URLs in index.html with live URLs.

Re-upload to S3.

Step 5 – QR Code & Marketing Updates

Generate new QR code (if domain changes) pointing to the new custom domain.

Update any printed materials or PDFs.

9. GitHub & Source of Truth

Local repo root: aws-learning-projects

Project folder for this site:
04-sprinter-van-app/

Handover docs folder:
05-luxe-vip-express-handover/

Recommended Git commit message:
"Add Luxe VIP Express technical handover and architecture notes"

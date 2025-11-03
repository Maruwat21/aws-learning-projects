# Project 01 – Static Resume Website (AWS S3)

## Overview
This project is my personal resume website hosted on Amazon S3.  
It’s a static site that showcases my professional experience and skills using simple HTML and CSS.

**Live endpoint:**  
http://resume-maruwathomas.s3-website-us-east-1.amazonaws.com

---

## What I Learned
- How to host a static website using Amazon S3.
- How to configure bucket policies for public access.
- How to use static website hosting features in AWS.
- How to troubleshoot common errors like AccessDenied and missing index files.

---

## Files
- `index.html` – main resume webpage  
- `styles.css` – basic page styling  
- `assets/` – folder for images and icons (currently empty)  
- `README.md` – project documentation

---

## Troubleshooting Notes
1. **AccessDenied error** – Fixed by disabling “Block all public access” and adding a public bucket policy.
2. **File downloaded instead of shown** – Fixed by setting metadata `Content-Type: text/html` when uploading.
3. **HTML opening as text** – Fixed by using TextEdit → “Make Plain Text” mode.

---

## Next Steps
- Add a personal photo and logo to the `assets/` folder.
- Create a small “About” section.
- Add navigation links to new projects as they go live.

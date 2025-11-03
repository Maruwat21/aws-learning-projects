# Project 1 — Resume Website on AWS (Mac zsh/bash Edition)

This folder documents *exactly* what we did to get your resume live on AWS, with copy‑paste commands and screenshots you can recreate.

**You’ll complete this in three phases:**
1. Build the site locally (HTML + CSS)
2. Host it on S3 (static website hosting on Free Tier)
3. Verify + troubleshoot if needed

---

## Prerequisites
- macOS Terminal (zsh/bash)
- TextEdit (Plain Text mode) or VS Code
- AWS account (Free Tier OK)
- Your planned bucket name: **resume-maruwathomas** (already created)
- Git (optional but recommended)

> Tip: Use single quotes in shell to avoid the `!doctype` history expansion issue in bash/zsh.

---

## Phase A — Build locally

### A1) Create project folder and init git
```bash
cd ~/Documents
mkdir resume-site && cd resume-site
git init
```

### A2) Create a minimal `index.html` (HTML renders test)
```bash
printf '<!doctype html><html><body><h1 style="color:red">Hi Maruwa ✅</h1></body></html>' > index.html
open index.html  # Should show a red heading locally
```

### A3) Create clean resume HTML (final `index.html`)
Using **nano** (guaranteed plain text):

```bash
nano index.html
```
Paste your resume HTML (see `examples/index.full.html` provided in this docs pack), then:
- Save: **Ctrl+O**, Enter
- Exit: **Ctrl+X**

### A4) Create `styles.css`
```bash
nano styles.css
```
Paste the CSS (see `examples/styles.css` provided), then save (Ctrl+O) and exit (Ctrl+X).

### A5) Quick local check
```bash
open index.html
```
You should see a **styled** page (larger name, blue links, pill-style skills).

---

## Phase B — Host on S3 (Static Website Hosting)

### B1) Create (or open) your bucket
- Console → **S3** → Create bucket (name: `resume-maruwathomas`, region: us-east-1)  
- Uncheck **Block all public access** (confirm)

### B2) Enable Static Website Hosting
- Bucket → **Properties** → **Static website hosting** → **Enable**
- **Index document:** `index.html`
- **Save changes**
- Note the **Bucket website endpoint** URL

### B3) Upload files with correct metadata
- **Objects** → **Upload** → add `index.html` and `styles.css`
- Expand **Properties → Metadata** for each file:
  - `index.html` → `Content-Type: text/html`
  - `styles.css` → `Content-Type: text/css`
- **Upload**

### B4) Public read access
- **Permissions** → **Bucket policy** → paste the contents of `s3_bucket_policy.json` (already filled with your bucket ARN)
- **Save**

### B5) Open your site
- **Properties** → **Static website hosting** → open the **Website endpoint**
- Hard refresh: **⌘ + Shift + R**

---

## Phase C — Verify + Troubleshoot

### C1) Verify CSS is loading
- Browser → Right‑click → **Inspect** → **Network** → refresh
- You should see **index.html** (`text/html`) and **styles.css** (`text/css`) with status **200**

### C2) Common gotchas (see `TROUBLESHOOTING.md` for full list)
- File casing: `index.html` (not `Index.html`)
- TextEdit saved as RTF or `.txt` → use **Plain Text** or **nano**
- Wrong URL: use **Website endpoint**, not Object URL
- Missing/incorrect **Content-Type**
- CSS in wrong folder or mis‑spelled in `<link rel="stylesheet" href="styles.css">`
- Public access blocked or missing bucket policy

---

## Optional — Commit and push to GitHub
```bash
git add .
git commit -m "Add resume site and docs"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/resume-site.git
git push -u origin main
```

---

## Files included in this docs pack
- `TROUBLESHOOTING.md` — Symptoms → Causes → Fixes
- `commands_cheatsheet.md` — All commands used with plain-English notes
- `nano_quick_guide.md` — Nano keys (save, exit, cut lines, paste)
- `s3_bucket_policy.json` — Public read policy for **resume-maruwathomas**
- `examples/index.full.html` — Clean resume HTML (same structure we deployed)
- `examples/styles.css` — Matching CSS
- `checklist.md` — One-page checklist to tick off each action

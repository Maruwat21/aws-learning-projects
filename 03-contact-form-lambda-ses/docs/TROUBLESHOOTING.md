# TROUBLESHOOTING — Resume Site on S3

Use this when something doesn’t look right. Find the symptom, apply the fix.

---

## 1) Page shows raw HTML tags
**Symptom:** You see `<!doctype html>`, `<html>`, etc. in the browser.  
**Cause:** `index.html` served as `text/plain` or wrong URL.  
**Fix:**
- Re-upload `index.html` with **Content-Type: text/html**
- Use the **Website endpoint** (S3 static website URL), not an Object URL

---

## 2) Page loads but looks unstyled (plain text)
**Symptom:** Content appears but no formatting/colors.  
**Causes & Fixes:**
- `styles.css` missing or misnamed → Ensure file is in bucket root as `styles.css`
- Wrong link → In `<head>`, confirm: `<link rel="stylesheet" href="styles.css">`
- Wrong Content-Type → Set `text/css` on `styles.css`
- Browser cached old copy → Hard refresh (**⌘ + Shift + R**)

---

## 3) 403 Forbidden / Access Denied
**Cause:** Public access blocked or missing bucket policy.  
**Fix:**
- Disable **Block all public access** at bucket level
- Add bucket policy (see `s3_bucket_policy.json`)

---

## 4) 404 Not Found
**Cause:** Wrong key name or index document not configured.  
**Fix:**
- Ensure **Index document** is `index.html` in Static website hosting
- Confirm object keys exactly: `index.html`, `styles.css`

---

## 5) CSS still won’t load
**Checklist:**
- File at bucket root? (not inside a subfolder)
- Exact filename? (`styles.css`)
- Content-Type = `text/css`
- DevTools Network tab → verify 200 status for `styles.css`

---

## 6) Local editing issues
**Symptoms:** Weird quotes, `.txt` extensions, RTF.  
**Fix:**
- Use **nano** in Terminal or **TextEdit → Format → Make Plain Text**
- Ensure filenames are lowercase and extensions correct (`.html`, `.css`)

---

## 7) Shell error: `bash: !doctype: event not found`
**Cause:** `!` triggers history expansion in bash/zsh.  
**Fix:** Use **single quotes** around HTML or use **nano** editor.

---

## 8) `Index.html` vs `index.html`
**Cause:** Case-sensitive on S3.  
**Fix:** Rename to lowercase:
```bash
mv Index.html index.html
```

---

## 9) Wrong URL used
**Fix:** Use the **S3 Website endpoint** from **Properties → Static website hosting**.

---

## 10) CloudFront caching (if you add CDN later)
**Fix:** Create invalidation: path `/*`.

# Commands Cheat Sheet

## Create project
```bash
cd ~/Documents
mkdir resume-site && cd resume-site
git init
```

## Minimal test file
```bash
printf '<!doctype html><html><body><h1 style="color:red">Hi Maruwa ✅</h1></body></html>' > index.html
open index.html
```

## Edit files with nano
```bash
nano index.html   # Ctrl+O (save), Enter, Ctrl+X (exit)
nano styles.css
```

## Rename (fix case)
```bash
mv Index.html index.html
```

## View files
```bash
ls -l
cat index.html
cat styles.css
```

## Git (optional)
```bash
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/resume-site.git
git push -u origin main
```

# Deploying the portfolio

Files: `index.html`, `favicon.svg`, `apple-touch-icon.png`, `og-image.png` (LinkedIn preview), `CNAME`, `.nojekyll`.

## 1. Set your domain
The site assumes `portfolio.yousecurity.com`. To use a different one:
```bash
cd ~/portfolio-site
sed -i 's#https://portfolio.yousecurity.com#https://YOUR.DOMAIN#g' index.html
echo "YOUR.DOMAIN" > CNAME
```
Using `<username>.github.io` with no custom domain? Delete `CNAME` and set the URL to `https://<username>.github.io`.

## 2. Push to GitHub Pages
```bash
# Create an empty public repo on github.com first, e.g. "portfolio"
cd ~/portfolio-site
git init -b main
git add .
git commit -m "AI portfolio site"
git remote add origin https://github.com/<username>/portfolio.git
git push -u origin main
```
Then in the repo: **Settings > Pages > Source: Deploy from a branch > main / (root)**.

## 3. DNS (custom domain)
At your DNS provider, add:

| Type  | Name      | Value                  |
|-------|-----------|------------------------|
| CNAME | portfolio | `<username>.github.io` |

Back in **Settings > Pages**, confirm the custom domain and tick **Enforce HTTPS** once the cert issues (usually minutes, up to an hour).

Verify:
```bash
dig +short portfolio.yousecurity.com CNAME
curl -sI https://portfolio.yousecurity.com | head -1
```

## 4. Check the LinkedIn preview
Paste the URL into https://www.linkedin.com/post-inspector/ to force LinkedIn to re-read `og-image.png`.

## Security notes
- Static HTML only: no forms, no JS dependencies, no third-party trackers. Fonts load from Google Fonts.
- Before pushing, confirm no lab details slipped in:
```bash
grep -nE '192\.168\.|100\.[0-9]+\.[0-9]+\.[0-9]+|:[0-9]{4}/|token|apikey' index.html || echo clean
```
- Optional hardening: enable GitHub account 2FA and turn on "Require signed commits" for the repo.

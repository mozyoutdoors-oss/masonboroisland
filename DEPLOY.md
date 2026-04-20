# Deploy MasonboroIsland.org

Same pattern as SharkToothIsland.org. Static site on GitHub Pages, custom domain via Porkbun DNS. ~15 minutes end to end.

---

## 1. Create the GitHub repo

1. Go to **https://github.com/mozyoutdoors-oss** (or your preferred org) → **New repository**
2. Repository name: `masonboroisland`
3. **Public** (required for free GitHub Pages)
4. Don't initialize with README — we have files ready to push
5. Click **Create repository**

GitHub will show you a "quick setup" page with the remote URL. Copy the HTTPS URL: `https://github.com/mozyoutdoors-oss/masonboroisland.git`

---

## 2. Push the local folder

Open a terminal in the project folder:

```bash
cd "C:\Users\Admin\OneDrive\Documents\Claude\Projects\Masonboro Island"

# Initialize git if not already
git init -b main

# Stage everything except dev-only files
git add index.html PROJECT.md DEPLOY.md

# Optional: include svg-test.html for reference, OR add to .gitignore
git add svg-test.html

# Create .gitignore to keep dev clutter out
cat > .gitignore << 'EOF'
.claude/
*.log
.DS_Store
Thumbs.db
EOF
git add .gitignore

git commit -m "Initial commit: Masonboro Island field guide landing"

# Link to GitHub remote
git remote add origin https://github.com/mozyoutdoors-oss/masonboroisland.git

# Push
git push -u origin main
```

---

## 3. Turn on GitHub Pages

1. In the GitHub repo page, click **Settings** (top tabs)
2. Left sidebar → **Pages**
3. Under "Build and deployment":
   - **Source:** Deploy from a branch
   - **Branch:** `main`
   - **Folder:** `/` (root)
4. Click **Save**

Wait ~30 seconds. You'll get a preview URL like `https://mozyoutdoors-oss.github.io/masonboroisland/` — confirm the site loads there before moving to DNS.

---

## 4. Configure the custom domain (Porkbun)

### a. Add CNAME file to the repo

From the project folder:

```bash
echo "masonboroisland.org" > CNAME
git add CNAME
git commit -m "Add CNAME for masonboroisland.org"
git push
```

### b. In GitHub Pages settings

1. Back in **Settings → Pages**
2. **Custom domain** field: enter `masonboroisland.org`
3. Click **Save**
4. GitHub will verify and eventually provision HTTPS (takes up to 24h; usually minutes)
5. Once verified, check **"Enforce HTTPS"**

### c. In Porkbun DNS

Log into Porkbun, go to the `masonboroisland.org` domain's DNS records. Add these records:

| Type | Host | Answer | TTL |
|---|---|---|---|
| A | (blank / @) | 185.199.108.153 | 600 |
| A | (blank / @) | 185.199.109.153 | 600 |
| A | (blank / @) | 185.199.110.153 | 600 |
| A | (blank / @) | 185.199.111.153 | 600 |
| CNAME | www | mozyoutdoors-oss.github.io | 600 |

**Important:** delete any existing Porkbun parking/placeholder A or ALIAS records first.

DNS propagates in 10–60 minutes. Site will be live at `https://masonboroisland.org`.

---

## 5. Verify it's live

Check:
- [ ] `https://masonboroisland.org` loads the field guide
- [ ] `https://www.masonboroisland.org` redirects to the root
- [ ] HTTPS works (green padlock, no mixed-content warnings)
- [ ] Live data loads: tide card, weather card, launch card all populate (they need HTTP not file:// to work)
- [ ] ClearDarkSky link opens correctly in new tab
- [ ] Email capture submits successfully (check Station@sharktoothisland.org inbox — MasonboroIsland sends there too via Web3Forms, same key)

---

## 6. Post-launch housekeeping

Once live, set these up in a new session:

### Google Search Console
1. Add property for `masonboroisland.org` (URL-prefix mode)
2. Verify via HTML meta tag (can be added to `index.html`)
3. Submit `sitemap.xml` once one exists (not created yet — low priority for single-page site)

### Analytics (optional)
- If you want analytics, add a tag. Mozy Outdoors probably has a preference — match that pattern.

### Email forwarding
- `hello@masonboroisland.org` currently points to the mailto: link in the footer. Configure an actual mailbox or forward in Porkbun email settings.

### Web3Forms
- Currently using STI's shared access key. Consider creating a dedicated MasonboroIsland form access key so you can filter signups separately.

---

## Iterating after launch

The site is a single `index.html` file. To update:

```bash
cd "C:\Users\Admin\OneDrive\Documents\Claude\Projects\Masonboro Island"
# edit files
git add -A
git commit -m "What changed"
git push
```

GitHub Pages re-deploys on every push to main. Takes ~30-60 seconds for changes to go live.

---

## Troubleshooting

**"Site not found" or 404 after first deploy**
- Wait 5 minutes. GitHub Pages provisioning takes a moment.
- Confirm `index.html` is at repo root (not inside a subfolder)

**Live data cards stuck on "Loading..."**
- Only happens on file:// origin. On live domain they work.
- If still broken on live: open dev tools → Console, check for CORS errors. All 3 APIs (NOAA, Open-Meteo, LL2) are CORS-friendly from HTTPS origins.

**Custom domain shows "Domain's DNS record could not be retrieved"**
- DNS hasn't propagated yet. Wait 1 hour.
- Verify A records in Porkbun point to GitHub Pages IPs exactly as listed above.

**HTTPS cert not issuing**
- Uncheck "Enforce HTTPS", wait 1 hour, check it again. GitHub provisions Let's Encrypt after DNS propagates.

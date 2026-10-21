# Chaoping Ma – personal website

Single static page (EN / DE / 中文), no build step.

## Files
- `index.html` – the whole site (text for all 3 languages is in the `T = {...}` block near the bottom; LinkedIn link in `LINKEDIN_URL`)
- `assets/icf-acc-certificate.png` – ICF ACC certificate

## Deploy to GitHub Pages
1. Create a new public repo on GitHub, e.g. `chaoping-ma.github.io` (then the site lives at https://chaoping-ma.github.io) – or any name (site at https://<username>.github.io/<repo>/).
2. Upload `index.html`, `README.md` and the `assets` folder ("Add file → Upload files" → drag & drop → Commit).
3. Repo → Settings → Pages → Source: "Deploy from a branch" → Branch `main`, folder `/ (root)` → Save.
4. After ~1 minute the site is live at the URL shown on that page.

To edit text later: open `index.html` on GitHub → pencil icon → change the line in `T = {...}` → Commit. The site updates automatically.

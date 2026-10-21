# Handover: Chaoping Ma – Personal Website

**For:** Claude Code
**Owner:** Chaoping Ma
**Goal:** Deploy this finished static site to GitHub Pages. Keep it as plain HTML. There is no build step and no framework.

---

## 1. TL;DR – what you need to do

1. Check the files (section 3). The site is already built and tested. **Do not rebuild it from scratch.**
2. Open `index.html` in a browser (or Playwright) and confirm the acceptance checklist (section 8) passes.
3. Create the GitHub repo, push, and turn on GitHub Pages (section 7).
4. Report the live URL back to the owner.

Ask the owner before changing any wording, design or structure. The content has been reviewed against her latest CV.

---

## 2. Context

- **Who:** Chaoping Ma. Strategy & Transformation leader, ex-McKinsey (2019–2024), now Senior Project Manager at BASF In-house Consulting (since 03/2025). ICF Associate Certified Coach (ACC). Based in Heidelberg.
- **Purpose of the site:** a personal page used for job applications (professional profile) and to present her coaching side business.
- **Languages:** EN / DE / 中文. The visitor switches in the header. The default follows the browser language, and the choice is remembered in `localStorage`.
- **Tone:** sharp, concise and humble (MBB consultant). Keep the copy conservative and professional.

---

## 3. Files

```
/
├── index.html                    # Entire site: HTML + CSS + JS, self-contained
├── assets/
│   └── icf-acc-certificate.png   # ICF ACC certificate (image only, 1210×935)
├── README.md                     # Short manual deploy guide for the owner
└── HANDOVER.md                   # This file
```

External dependencies: **Google Fonts only** (DM Serif Display, Inter, Noto Sans SC, Noto Serif SC). The site uses no JS libraries and no CDN scripts.

---

## 4. Page structure (`index.html`)

| Section id | Content |
|---|---|
| `#home` (hero) | Name, chips ("Strategy & Transformation", ex-McKinsey, ICF ACC), one-line pitch, LinkedIn button, "Explore coaching" button, illustrated portrait with 2 speech bubbles, 4 stat tiles (7+ years · 300+ initiatives · €500m+ EBITDA potential · 3 languages) |
| `#about` | 3 paragraphs plus a "What I stand for" card listing 4 values: Equality, Curiosity, Deep connection, Continuous growth |
| `#experience` | Timeline: BASF (4 bullets) → Career break (3) → McKinsey with 3 sub-roles. Then Education cards (Mannheim, Peking University, Internships) |
| `#coaching` | Intro + coach/coachee scene; "What coaching is / is not" (4 cards); "When to consider coaching" plus "Who I work with" (audiences + topics); "My coaching style" (6 cards); "How we work together" (4 steps); honest-fit note; ICF certificate card |
| `#voices` | 6 testimonial quotes with illustrated avatars, plus a "crowd" of 20 illustrated people |
| `#contact` | CTA panel with a LinkedIn button |
| footer | © year · Chaoping Ma · Heidelberg |

### Content rules
- **LinkedIn:** `https://www.linkedin.com/in/chaoping-ma-8b560373`. Set once in the `LINKEDIN_URL` constant at the top of the script and applied to every `.js-linkedin` link.
- **No phone number and no email on the site.** This is a privacy choice by the owner, even though both appear on her CV.
- **Certificate:** show it **as an image only**. Do not link to a PDF and do not add a download button. Clicking the image opens an in-page lightbox to enlarge it, and that is fine to keep.
  - Awarded: 23.10.2025. Valid through: 31.10.2028.
- **Career break bullet:** keep the wording "Completed ICF-accredited coach training". Do not change it to "Earned the ICF ACC certification", because the certificate's award date (10/2025) falls after the career break (07/2024–02/2025).
- **Testimonials:** anonymised, shortened and translated from the owner's coaching document. Do not add names.

---

## 5. How it works (technical)

### i18n
- All translatable text sits in one object `T` near the bottom of `index.html`:
  ```js
  "key": ["English", "Deutsch", "中文"]
  ```
- Elements carry `data-i18n="key"`. The function `setLang()` sets their `innerHTML`, so simple inline HTML such as `<b>` and `<em>` is allowed.
- `<html lang>` switches between `en`, `de` and `zh-CN`, and `document.title` changes per language.
- **To edit text, change the entry in `T`.** Every key must have all 3 languages.

### Illustrations ("ligne claire", Tintin-inspired, all original characters)
- Generated as inline SVG by `figure(person, flip)` in the script. No image files are used.
- Style: uniform black outlines (3px), flat colours, dot eyes (with a blink animation), small blush cheeks.
- **Do not draw recognisable Tintin characters.** The quiff hairstyle and the ginger-hair + blue-sweater combination are deliberately excluded.
- `CAST` holds 20 people in the **required ratio**:
  - Asian women 6 (30%): indices 0–5
  - European women 4 (20%): indices 6–9
  - Asian men 5 (25%): indices 10–14
  - Western men 5 (25%): indices 15–19
- `PEOPLE.me` is the owner (Asian woman, long black hair, navy blazer). `PEOPLE.coachee` is used in the coaching scene.
- Usage: `<div class="fig" data-person="me|coachee|0-19" [data-flip="1"] [data-crop="1"]>`. `data-crop` zooms in on the face for avatars.
- If you add figures, keep the overall cast ratio at 30 / 20 / 25 / 25.

### Design system
- Visual reference: [CommunityPro/portfolio-html](https://github.com/CommunityPro/portfolio-html) for the clean, illustration-led portfolio feel. Interaction reference: [AstroWind](https://github.com/arthelokyo/astrowind) for the sticky header that blurs on scroll, fade-up reveal on scroll, dark mode and card hover lift.
- Comic-panel accents: 2.5px ink borders, offset hard shadows (`5px 5px 0`), speech bubbles, halftone dot background in the hero.
- Colour tokens live on `:root`: cream `#FBF7EE`, ink `#17171C`, blue `#2B5CA8`, red `#D6402F`, yellow `#F2C14E`.
- Dark mode follows `prefers-color-scheme` by default and can be overridden with the moon button (`data-theme`, saved in `localStorage`).
- `prefers-reduced-motion` disables all animations.
- Responsive: the nav collapses to a hamburger menu at 860px or narrower, and the brand name is hidden at 480px or narrower. There is no horizontal scroll at 390px width.
- Every `localStorage` access is wrapped in try/catch.

---

## 6. Optional improvements (only if the owner asks)

- Add `<meta property="og:*">` tags and a social preview image for LinkedIn sharing.
- Add a favicon. An SVG of the owner's figure head would work.
- Use a custom domain via a `CNAME` file.
- Add an email contact. **Ask the owner first.**

---

## 7. Deployment to GitHub Pages

Prerequisites: `git` and the GitHub CLI `gh`, authenticated as the owner (`gh auth status`). If you are not authenticated, ask the owner to run `gh auth login`.

**Repo name: ask the owner which option she prefers.**
- `<github-username>.github.io` → the site is served at `https://<username>.github.io/`. This is recommended for a personal site.
- Any other name, e.g. `personal-website` → the site is served at `https://<username>.github.io/personal-website/`.

```bash
cd <project folder containing index.html>
git init
git add index.html assets README.md HANDOVER.md
git commit -m "Initial commit: personal website (EN/DE/ZH)"
git branch -M main

# Replace REPO with the chosen name
gh repo create REPO --public --source=. --remote=origin --push

# Enable GitHub Pages from main / root
gh api -X POST repos/{owner}/REPO/pages \
  -f "source[branch]=main" -f "source[path]=/"

# Check status / URL (first build takes ~1 min)
gh api repos/{owner}/REPO/pages --jq '.html_url,.status'
```

Notes:
- `{owner}` is filled in automatically by `gh api` when it is run inside the repo. Otherwise use the username from `gh api user --jq .login`.
- Add an empty `.nojekyll` file to the root to skip Jekyll processing: `touch .nojekyll`, then commit.
- The repo must be **public** for GitHub Pages on a free account.
- Asset paths are relative (`assets/...`), so the site works both at the domain root and under `/REPO/`.

After deploying, open the live URL and run the checklist below against it.

---

## 8. Acceptance checklist

- [ ] The page loads with no console errors, both locally and at the live URL.
- [ ] EN / DE / 中文 switching updates **every** text element (no empty or missing strings). The `<html lang>` and the tab title also change.
- [ ] The language choice persists after a reload.
- [ ] The dark-mode toggle works, and the choice persists.
- [ ] Both LinkedIn buttons open `https://www.linkedin.com/in/chaoping-ma-8b560373` in a new tab.
- [ ] The certificate image loads from `assets/icf-acc-certificate.png`, and clicking it opens the lightbox.
- [ ] There is no PDF link anywhere.
- [ ] There is no phone number or email anywhere.
- [ ] The crowd shows 20 figures (6 Asian women, 4 European women, 5 Asian men, 5 Western men).
- [ ] On mobile (390px): the hamburger menu works, there is no horizontal scroll, and the header fits on one line.
- [ ] Scroll-reveal animations play, and are disabled with `prefers-reduced-motion`.
- [ ] The footer shows "Heidelberg" and the current year.

---

## 9. Source material (for reference; already reflected in the site)

- Latest CV: `CV_Chaoping_Ma_SAP.pages` (Oct 2026 version: Heidelberg, updated BASF bullets).
- Coaching philosophy document: "Personal AI assistant for coaching" (coaching definition, style, audiences, process, testimonials).
- ICF ACC credential certificate (PDF, rendered to the PNG in `assets/`).

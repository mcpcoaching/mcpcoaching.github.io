# Handover: Chaoping Ma – Personal Website

**For:** the next Claude session (Claude Code or Cowork)
**Owner:** Chaoping Ma (GitHub: `mcpcoaching`)
**Last updated:** 2026-10-22

---

## 1. Current state – LIVE

| | |
|---|---|
| Live site | https://mcpcoaching.github.io/ |
| Repo | https://github.com/mcpcoaching/mcpcoaching.github.io (public) |
| Branch / Pages source | `main`, folder `/` (root). Pages turned on automatically because of the `<user>.github.io` repo name. |
| Build | None. Plain static HTML. Each push to `main` redeploys in about 1 minute. |
| Jekyll | Off (`.nojekyll` in root). |

History:
1. 2026-10-21: First deploy. The site came from Cowork's `chaoping-website_1.zip`, unchanged.
2. 2026-10-21: Added a favicon: the owner's cartoon head (`assets/favicon.svg` and `assets/apple-touch-icon.png`). The owner checked it and approved.
3. 2026-10-22: The theme button icon now matches the mode (moon in dark, sun in light). The default still follows the OS.

The acceptance checklist (section 8) passed on the live URL. That covers: no console errors; all 144 `data-i18n` elements filled in EN/DE/ZH; `<html lang>` and the title switch; both LinkedIn links are correct and open with `_blank`; the certificate loads; no PDF, email or phone. The page has 26 figures in total: 6 testimonial avatars and 20 in the crowd. The footer reads "© 2026 Chaoping Ma · Heidelberg". There is no horizontal scroll at 384px. Not tested by hand: lightbox click, and whether language and theme stay after a reload.

---

## 2. How to make a change (workflow)

1. Clone: `gh repo clone mcpcoaching/mcpcoaching.github.io`. Or use git with HTTPS.
2. Edit the files. Ask the owner first before you change wording, design or structure (see section 4).
3. Commit and push to `main`. Wait for the build:
   `gh api repos/mcpcoaching/mcpcoaching.github.io/pages/builds/latest --jq .status` → `built`
4. Check the live URL. Tell the owner they may need a hard refresh (Cmd+Shift+R) because of caching.

**Tooling on the owner's Mac (as of 2026-10-21):**
- No Homebrew. `gh` is not on PATH.
- GitHub CLI binary: `~/.local/gh/gh_2.103.0_macOS_arm64/bin/gh`. It is logged in as `mcpcoaching` over HTTPS and is set up as the git credential helper.
  Use it with: `export PATH="$HOME/.local/gh/gh_2.103.0_macOS_arm64/bin:$PATH"`
- If the auth expired, run this in a terminal tab that the owner can see. The owner pastes the code at github.com/login/device:
  `gh auth login --hostname github.com --git-protocol https --web --clipboard`
- No SSH keys. No global git user. Commit with
  `git -c user.name="Chaoping Ma" -c user.email="mcpcassie@gmail.com" commit ...`
- A GitHub connector in claude.ai does **not** give local git access. Use `gh`.

---

## 3. Files

```
/
├── index.html                    # Entire site: HTML + CSS + JS, self-contained
├── assets/
│   ├── icf-acc-certificate.png   # ICF ACC certificate (image only, 1210×935)
│   ├── favicon.svg               # Tab icon: owner's cartoon head, yellow circle, ink border
│   └── apple-touch-icon.png      # 180×180 PNG of the same (Safari / iOS home screen)
├── .nojekyll
├── README.md                     # Short manual guide for the owner
└── HANDOVER.md                   # This file
```

External dependencies: Google Fonts only (DM Serif Display, Inter, Noto Sans SC, Noto Serif SC). No JS libraries.

---

## 4. Content rules (do not break)

- **Tone:** sharp, concise and humble (MBB consultant). Keep the copy conservative.
- **Ask the owner first** before any change to wording, design or structure.
- **LinkedIn:** `https://www.linkedin.com/in/chaoping-ma-8b560373`. It is set once in the `LINKEDIN_URL` constant and applied to every `.js-linkedin` link.
- **No phone number and no email on the site.** This is the owner's privacy choice.
- **Certificate:** show it as an image only. No PDF link and no download button. The in-page lightbox is fine. Awarded 23.10.2025, valid through 31.10.2028.
- **Career break bullet:** keep "Completed ICF-accredited coach training". The ACC was awarded 10/2025, after the break (07/2024–02/2025).
- **Testimonials:** anonymised. Never add names.
- **Illustrations:** original characters only. Never draw recognisable Tintin characters: no quiff, and no ginger hair with a blue sweater. `CAST` has 20 people, ratio 30/20/25/25: Asian women, European women, Asian men, Western men. Keep that ratio.

---

## 5. Page structure (`index.html`)

| Section id | Content |
|---|---|
| `#home` | Name, chips, pitch, LinkedIn and "Explore coaching" buttons, portrait with speech bubbles, 4 stat tiles (7+ years · 300+ initiatives · €500m+ EBITDA potential · 3 languages) |
| `#about` | 3 paragraphs, plus a "What I stand for" card (Equality, Curiosity, Deep connection, Continuous growth) |
| `#experience` | Timeline: BASF → Career break → McKinsey (3 sub-roles), then Education cards |
| `#coaching` | Intro scene, what coaching is and is not, when to consider it, who I work with, style (6 cards), process (4 steps), fit note, ICF certificate card |
| `#voices` | 6 testimonials, plus a crowd of 20 figures |
| `#contact` | CTA panel with LinkedIn |
| footer | © year · Chaoping Ma · Heidelberg |

---

## 6. Technical notes

- **i18n:** all text lives in object `T` near the bottom: `"key": ["English", "Deutsch", "中文"]`. Elements carry `data-i18n="key"`, and `setLang()` sets their `innerHTML`. Every key needs all 3 languages. The default language follows the browser. The visitor's choice is saved in `localStorage`.
- **Figures:** inline SVG from `figure(person, flip)`. Use them as `<div class="fig" data-person="me|coachee|0-19" [data-flip="1"] [data-crop="1"]>`. `data-crop` gives the face-only avatar, as in the header `.brand-mark`.
- **Favicon:** a static copy of `figure('me')` cropped to viewBox `20 18 80 80`, clipped to a circle on yellow `#F2C14E`. If the portrait in `figure()` changes, regenerate `favicon.svg` the same way. Then re-render the PNG: `qlmanage -t -s 180 -o assets assets/favicon.svg` and rename the output to `apple-touch-icon.png`.
- **Design tokens** (`:root`): cream `#FBF7EE`, ink `#17171C`, blue `#2B5CA8`, red `#D6402F`, yellow `#F2C14E`. Comic accents: 2.5px ink borders, `5px 5px 0` hard shadows.
- **Dark mode:** follows `prefers-color-scheme` by default. The `#theme` button shows the current mode: moon = dark, sun = light (CSS only, `.i-moon` / `.i-sun`). A click that differs from the OS sets `data-theme` and saves it in `localStorage`. A click back to the OS mode clears the override, so the page follows the OS again. `prefers-reduced-motion` turns off all animation.
- **Responsive:** hamburger menu at ≤860px. The brand name is hidden at ≤480px.
- Every `localStorage` access is wrapped in try/catch.

---

## 7. Open ideas (only if the owner asks)

- `og:*` meta tags and a social preview image for LinkedIn sharing.
- A custom domain: add a `CNAME` file and set up DNS.
- Email contact. Ask the owner first, because it is currently excluded on purpose.

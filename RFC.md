# RFC: App Store Legal Pages Static Site

**Author:** Ulad
**Date:** 2026-06-20
**Status:** Accepted — scaffold implemented
**Stack:** Plain HTML/CSS → GitHub Pages

---

## 1. Problem Statement

Apple App Store requires every app to have publicly accessible URLs for:

- **Privacy Policy** — mandatory for all apps
- **Terms & Conditions (EULA)** — required for apps with subscriptions or user accounts

Four apps need legal pages:

1. **Stop the Stranger** (Parental control — category TBD)
2. **Currency Converter** (Offline currency converter)
3. **Document Scanner PDF** (Scanner utility)
4. **QR Code, Business Card** (QR/card scanner)

These pages must be reachable via direct URL but **not discoverable** from the site homepage. A personal Apple Developer account (individual, not organization) is used, so no company branding is needed — developer name attribution is sufficient.

Legal text content lives in Google Docs and will be provided directly (as text) for each page; this RFC's implementation step ends with placeholder content awaiting that text.

---

## 2. Goals

- [x] Host Privacy Policy and Terms & Conditions pages for all 4 apps
- [x] Pages accessible via direct URL (for App Store Connect submission)
- [x] Homepage does **not** link to or reveal any of these pages
- [ ] Convert provided legal text into clean HTML pages (pending — text to be provided per app)
- [x] Zero ongoing maintenance cost
- [x] Zero build toolchain — no Node.js, no bundlers, no frameworks

## 3. Non-Goals

- No public-facing marketing site
- No CMS, no database, no backend
- No user authentication or logins
- No analytics or tracking (would contradict the privacy policies)
- No CI/CD pipeline (manual `git push` is sufficient)

---

## 4. Solution

### 4.1 Technology

**Plain HTML + CSS only.** No dependencies, no build step. GitHub Pages serves the repo directly.

### 4.2 Hosting: GitHub Pages

1. Public GitHub repository (`legal-pages`)
2. Push HTML files to `main`
3. Repo Settings → Pages → Source: `main` branch, `/ (root)` folder
4. Site live at `https://<username>.github.io/legal-pages/` (or custom domain via `CNAME`)

### 4.3 Domain

`CNAME` file is in place with a placeholder domain (`yourname.dev`). To go live with a custom domain: register it, point a `CNAME` DNS record at `<username>.github.io`, and replace the placeholder in the `CNAME` file. Otherwise the free GitHub subdomain works as-is — no domain purchase required.

---

## 5. File Structure (as implemented)

```
legal-pages/
├── index.html                       blank homepage, no links
├── style.css                        shared stylesheet, mobile-responsive
├── CNAME                            placeholder custom domain
├── README.md                        GitHub Pages setup notes
│
├── stop-the-stranger/
│   ├── privacy.html
│   └── terms.html
├── currency-converter/
│   ├── privacy.html
│   └── terms.html
├── document-scanner/
│   ├── privacy.html
│   └── terms.html
└── qr-code-business-card/
    ├── privacy.html
    └── terms.html
```

11 files, committed as the initial scaffold. Each `privacy.html` / `terms.html` currently contains placeholder markers: `[APP NAME]`, `[CONTACT EMAIL]`, `[DATE]`, and a placeholder paragraph for body text.

---

## 6. Content Workflow (next step)

Legal text will be supplied directly per page rather than via PDF extraction. For each app/page:

1. User provides the final text for `privacy.html` and/or `terms.html`.
2. Replace the placeholder paragraph with the supplied text, mapped to semantic HTML (`<h2>`, `<p>`, `<ul>`).
3. Replace `[DATE]` with the effective date and `[CONTACT EMAIL]` with the real contact address.
4. Leave `[APP NAME]` resolved to the actual app name in the `<title>` and footer.

One page at a time, as content arrives — no need to batch all 8.

---

## 7. Page Design Spec (implemented)

- System font stack, no external font dependency
- Mobile-responsive single-column layout, max-width container
- No navigation header linking to other app pages
- `Last updated` date near the top (`.updated` class)
- Footer with developer name + contact email
- No cookies, no scripts, no analytics
- Dark mode support via `prefers-color-scheme`

---

## 8. App Store Connect — Where to Enter URLs

Per app, in App Store Connect → App Information → General Information:

- **Privacy Policy URL** → `https://<domain>/<app-folder>/privacy.html`
- **Terms of Use URL** (if subscriptions/IAP) → `https://<domain>/<app-folder>/terms.html`

URLs must return HTTP 200 with readable content — Apple may check during review.

---

## 9. Maintenance

| Scenario | Action |
|---|---|
| Update privacy policy text | Edit the relevant `.html` file, `git push` |
| Add a 5th app | Create new folder + 2 pages, `git push` |
| Change contact email | Find & replace across all pages, `git push` |
| Domain renewal | Renew at registrar annually |
| GitHub Pages goes down | Extremely rare; mirror to Netlify as backup if needed |

---

## 10. Open Questions

- [ ] Do all 4 apps share the same base privacy policy text, or is each unique?
- [ ] What is the developer contact email to display on pages?
- [ ] Custom domain or free GitHub subdomain?
- [ ] Do any apps collect personal data beyond what's standard (camera, contacts, location)?
- [ ] Are any apps paid / have subscriptions? (affects whether T&C is mandatory)

These remain open and will be resolved as content is provided per app.

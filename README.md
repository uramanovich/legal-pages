# apps-legal

Static legal pages (Privacy Policy / Terms & Conditions) for App Store Connect submissions. Plain HTML/CSS, no build step.

## Structure

```
index.html                  blank landing page, no links to legal pages
style.css                   shared stylesheet
CNAME                       custom domain for GitHub Pages
<app-folder>/privacy.html
<app-folder>/terms.html
```

## GitHub Pages setup

1. Create a public GitHub repo (e.g. `apps-legal`) and push this project to `main`.
2. Repo Settings → Pages → Source: `main` branch, `/ (root)` folder.
3. Site goes live at `https://<username>.github.io/apps-legal/` (or the custom domain in `CNAME`).
4. If using a custom domain, add a `CNAME` DNS record at your registrar pointing to `<username>.github.io`, and update the `CNAME` file with your domain.

## Updating a page

Edit the relevant `privacy.html` / `terms.html` and push to `main`. No build step required.

## App Store Connect

For each app: App Information → General Information →
- Privacy Policy URL: `https://<domain>/<app-folder>/privacy.html`
- Terms of Use URL (if subscriptions/IAP): `https://<domain>/<app-folder>/terms.html`

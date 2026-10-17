# F.R.I.D.A.Y V2 legal pages

This package contains a mini website for GitHub Pages:
- `index.html` — landing page
- `terms.html` — Terms of Service
- `privacy.html` — repository-audited Privacy Policy
- `AUDIT.md` — code audit notes and remaining risks

## Publish with GitHub Pages

1. Create a **public** repository for the legal website, for example `friday-v2-legal`.
2. Upload all three HTML files (`index.html`, `terms.html`, `privacy.html`) to the repository root.
3. Open **Settings → Pages**. Select deployment from the `main` branch and `/(root)`, then save.
4. Wait for the Pages deployment to complete. Open both page URLs in a private/incognito browser window to confirm they load without signing in.
5. In the Discord Developer Portal, put the full `terms.html` URL in **Terms of Service URL** and the full `privacy.html` URL in **Privacy Policy URL**.

Expected URL pattern:

- `https://YOUR-GITHUB-USERNAME.github.io/friday-v2-legal/terms.html`
- `https://YOUR-GITHUB-USERNAME.github.io/friday-v2-legal/privacy.html`

Replace `YOUR-GITHUB-USERNAME` and the repository name with your actual values. Do not use those example URLs verbatim.

## Important before submitting

The privacy page is based on the public `main` branch reviewed on 17 October 2026. A code audit cannot verify the currently deployed commit, Firestore contents/permissions/TTL, Render log settings, or who can access Discord ticket/log channels. Make sure the live deployment matches the reviewed code and that the practices described are accurate. The audit identified no automatic expiry for unfinished ModMail pending records; consider adding a cleanup/expiry mechanism instead of relying only on disclosure.

These files are practical templates, not legal advice or a guarantee of Discord verification approval. Add a dedicated private privacy contact route before a wide public rollout.

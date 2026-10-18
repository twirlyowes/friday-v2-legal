# F.R.I.D.A.Y legal pages

This package contains a mini website for F.R.I.D.A.Y:
- `index.html` — landing page
- `terms.html` — Terms of Service
- `privacy.html` — repository-audited Privacy Policy
- `AUDIT.md` — code audit notes and remaining risks

## Important before submitting

The Privacy Policy is based on the public `main` branch reviewed on 17 October 2026.

A code audit cannot verify the currently deployed commit, Firestore contents, permissions or TTL settings, Render log settings, or who can access Discord ticket and log channels. Ensure the live deployment matches the reviewed code and that the practices described in the policy are accurate.

The audit identified no automatic expiry for unfinished ModMail pending records. Consider adding a cleanup or expiry mechanism instead of relying only on disclosure.

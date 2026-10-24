# F.R.I.D.A.Y Legal Center

Static GitHub Pages site for the public-facing legal documents:

- `index.html` — concise legal landing page
- `privacy.html` — privacy policy for the Discord bot and dashboard
- `terms.html` — terms of service for the bot and dashboard
- `legal.css` — shared styling
- `AUDIT.md` — maintainer notes from a source-code privacy review of the bot repository

The policies were refreshed on 24 October 2026 to cover both the bot and the dashboard, including Discord OAuth, dashboard sessions, Firestore configuration and history, ModMail, appeals, moderation records and retention limits.

## Maintenance note

Keep the published policy aligned with the deployed code, configured providers and actual data practices. The bot-code audit in `AUDIT.md` is a technical review, not a legal review and not a verification of live database permissions, provider logs or channel access. The current policy directs privacy requests to the maintainer's GitHub profile to request a private contact route; publishing a dedicated private privacy contact remains a recommended follow-up before a wider public rollout.

# F.R.I.D.A.Y — Privacy code audit notes

**Reviewed:** 17 October 2026  
**Repository:** https://github.com/twirlyowes/F.R.I.D.A.Y-V-2  
**Scope:** publicly accessible `main` branch files. This is a source-code review, not an audit of the live Render deployment, actual Firestore data, access-control rules, logs, or Discord channel permissions.

## Confirmed processing and storage

| Data/function | Source code behavior | Relevant files |
|---|---|---|
| Guild configuration | Stores per-server prefix, channels, roles, ticket/verification/voice settings, bad-word/filter configuration and optional text/YouTube settings in `guild_configs/{guildId}` and feature collections. Configs are not automatically wiped when the Bot leaves a server. | `lib/guildConfig.js`, `settings.js`, `badwords.js` |
| ModMail pending messages | DMs received before ticket setup are written to `modmail_pending/{userId}` with message content, attachment URL/name/content type and timestamp. That pending document is deleted once ticket history has been forwarded, or when there are no eligible ticket servers. | `help.js` around the DM `messageCreate` handler and ticket creation flow |
| Abandoned ModMail | No automatic expiry was found for pending ModMail documents when a member simply leaves the category/server selection unanswered. Those messages can therefore remain in Firestore. **Priority: high.** | `help.js` |
| Open/closed tickets | `modmail_tickets/{channelId}` stores ticket metadata including server/user/channel IDs, category/status, created/closed timestamps and claim/close staff IDs. Closing updates the record to `closed`; it does not delete the record. | `help.js` |
| Ticket transcripts | When `ticketLogs` is configured, ticket closure attempts to upload a transcript containing the ticket's messages, timestamps, authors/IDs and attachment names/URLs to the configured Discord log channel. The temporary ticket channel is scheduled for deletion after around 5 seconds, but the transcript attachment is not deleted by that flow. | `help.js`, `lib/guildConfig.js` |
| Warnings | Firestore warning records include a warning ID, moderator tag, reason and timestamp under a server/user-derived document ID. Commands exist to remove warnings/reset records; no common TTL is applied. | `warn.js` |
| Strikes | Firestore stores strike records with guild/user IDs, reason, moderator ID, strike number and timestamp plus a strike counter. Records are removed through the relevant command flow rather than a universal expiry. | `strike.js` |
| Bad-word and edited-link filtering | The Bot examines guild message content for configured words/links. When an offending message is deleted, up to the first 1,000 characters, user ID and channel details can be posted to the configured logs channel. These copied Discord log messages are not stored in Firestore by this code path. | `badwords.js` |
| Staff activity | The Bot accumulates online/active time, voice time, staff message count and selected moderation command counts by server/user. Firestore stores totals and update timestamps. Daily reset zeroes figures; it does not delete the documents. A `removeuser` command deletes a member's activity document. | `activetime.js` |
| AFK | Stores a reason, time/setup timestamps and nickname restoration state by server/user. It is normally deleted when the member speaks again in that server. | `afk.js` |
| Message sniping | Retains up to 15 deleted messages per channel in process memory with content, author reference, timestamp, and an image URL when present. No Firestore write or time-based expiry was found; cache clears on process restart or when older entries are evicted. | `verify.js` |
| Captcha verification | Pending captcha answer/attempt state and short-lived cooldowns are held in memory. The pending captcha has a 60-second timeout. Verification changes Discord roles based on server config. | `verify.js` |
| Temporary voice rooms | Firestore records channel ID, owner ID, server ID and creation timestamp. The code deletes the Firestore record when it successfully deletes a temporary voice channel; stale entries may remain if cleanup is skipped/fails. | `voicesystem.js` |
| Quote images | State includes up to 400 characters of quoted text, author display details/avatar, and requester ID, held in memory for about one hour. The generated quote image is sent as a Discord message and isn't removed by the state TTL. | `quote.js` |
| Server allowlist/no-prefix | Owner-managed server allowlist stores server IDs and approval metadata; global no-prefix list stores user IDs and who/when granted access. These aren't automatically deleted on server departure. | `access.js`, `lib/noprefix.js` |
| Console mirroring | When `CONSOLE_LOG_CHANNEL_ID` is set, runtime console logs are mirrored into that Discord channel. Diagnostic lines may include names, tags, IDs, server/channel names, and error context. | `console.js`, `.env.example` |
| External calls | Optional YouTube notification reads public channel/feeds and scans recent notification-channel messages; Wikipedia utility sends the search term to Wikipedia. | `YouTube.js`, `misc.js` |

## Main recommendations before a public rollout

1. **Add automatic expiry for abandoned pending ModMail.** Add a document-level `expiresAt` and configure Firestore TTL or schedule a cleanup job. Consider limiting pending message count/age and deleting older content. Merely disclosing indefinite retention is not ideal.
2. **Decide ticket transcript retention.** Provide a clear retention period, restrict `ticketLogs` permissions, and if appropriate add a command/job to delete old transcript messages/attachments. Closed Firestore ticket metadata currently remains.
3. **Set a private privacy contact route.** Do not ask users to publish personal information in a public issue. Publish a dedicated contact method and document the process for verifying/fulfilling requests.
4. **Check deployed and provider settings.** Confirm the deployed commit matches the reviewed branch; inspect Firestore IAM/rules and backups, hosting log retention, optional console mirror channel permissions, and Discord ticket/log channel permissions.
5. **Define retention for other records.** Decide how long warnings, strikes, activity totals, AFK/voice records and moderation log copies should exist; document and implement cleanup where appropriate.
6. **Disclose content processing clearly.** Members should know that bad-word/link-filtered message content can be copied into moderation logs, deleted messages can be temporarily retrieved by `snipe`, and quote images can reproduce message text.

## Limitations

This review describes visible code, not whether a running deployment uses this exact branch, whether external services retain their own logs/backups, or whether Discord servers have separate moderation/retention systems. This is not legal advice and does not guarantee that Discord will approve verification.

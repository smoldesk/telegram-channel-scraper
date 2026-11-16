# Telegram Channel Scraper

![Telegram Channel Scraper](https://raw.githubusercontent.com/smoldesk/apify-scrapers/main/assets/telegram/banner.webp)

Scrape public Telegram channels: messages, views, reactions, media, links, public comments and subscriber counts. No Telegram account, phone number, `api_id` or `api_hash`. Runs on [Apify](https://apify.com/smoldesk/telegram-channel-scraper).

## What you get

- **Messages**: text, date, views, forwards, hashtags, links and link previews, with a clean `markdown` version for LLMs.
- **Reactions**: each emoji with its count, plus the total.
- **Media**: photo and video URLs, and a `mediaType` per message.
- **Public comments** under each post, one row each, with the author and the comment it replies to. Every message gets a `commentCount`.
- **Emails and @mentions** pulled out of each message.
- **Channel info**: title, description, verified flag, subscribers, and counts of photos, videos and links.
- **Filters**: keyword search inside each channel, date range or last N days, message type, minimum views and excluded words. Skipped messages are free.
- **Single posts** by link with `postUrls`.
- **Only new messages** mode for monitoring: each run returns what was posted since the previous one.
- **Many channels per run**, scraped in parallel.

## Run it

Open the [Telegram Channel Scraper on Apify](https://apify.com/smoldesk/telegram-channel-scraper), fill in the form and click Start. Or call it from code with your [Apify token](https://console.apify.com/settings/integrations):

```bash
curl -X POST "https://api.apify.com/v2/acts/smoldesk~telegram-channel-scraper/run-sync-get-dataset-items?token=YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "channels": ["durov", "https://t.me/telegram"],
    "maxMessagesPerChannel": 200,
    "searchQuery": "update",
    "lastNDays": 30,
    "includeComments": true,
    "includeChannelInfo": true
  }'
```

To monitor a channel, add `"onlyNewMessages": true` and schedule the run hourly or daily.

## Sample output

```json
{
  "type": "message",
  "channel": "durov",
  "messageId": 531,
  "url": "https://t.me/durov/531",
  "date": "2026-07-16T20:45:25+00:00",
  "text": "Telegram now has a Rich Text Editor...",
  "markdown": "Telegram now has a **Rich Text Editor**...",
  "mediaType": "video",
  "emails": [],
  "mentions": [],
  "views": 5800000,
  "totalReactions": 95000,
  "videos": ["https://cdn4.telesco.pe/file/....mp4"],
  "forwardedFrom": null,
  "channelTitle": "Pavel Durov",
  "channelSubscribers": 10400000,
  "commentCount": null
}
```

```json
{
  "type": "comment",
  "commentId": 74555,
  "channel": "cointelegraph",
  "messageId": 73214,
  "messageUrl": "https://t.me/cointelegraph/73214",
  "date": "2026-11-12T00:34:28+00:00",
  "text": "It's all a setup by the u.s. govt. They want full control of nvidia...",
  "authorName": "Horus the Elder",
  "replyToCommentId": null
}
```

```json
{
  "type": "channel",
  "channel": "durov",
  "title": "Pavel Durov",
  "verified": true,
  "subscribers": 10400000,
  "photos": 102,
  "videos": 47,
  "links": 200
}
```

Export as JSON, CSV or Excel, or send it to Google Sheets, Slack, Make, Zapier or n8n.

## Pricing

Pay per result. No monthly fee. Prices drop on paid Apify plans.

| Result (per 1,000) | Free | Starter | Scale | Business |
|---|---|---|---|---|
| Message | $1.00 | $1.00 | $0.90 | $0.75 |
| Channel info (only when `includeChannelInfo` is on) | $2.00 | $2.00 | $1.75 | $1.50 |
| Comment (only when `includeComments` is on) | $0.50 | $0.50 | $0.50 | $0.50 |

Messages skipped by your filters or already seen in only-new mode are not charged.

## Limits

- Public channels with web preview only. Private channels, groups and user chats are not reachable.
- Comments come from the channel's public discussion, so only channels that have comments switched on have them. Sticker comments are skipped.
- Large counts are rounded the way Telegram shows them, so `10.4M` becomes `10400000`.
- Video URLs on Telegram's CDN expire. Download them soon after the run.

## FAQ

**Do I need a Telegram API key or phone number?**
No. There is no `api_id`, `api_hash`, phone login or session file. No account is used, so none can be banned.

**How do I scrape a Telegram channel without Telethon?**
Pass the channel name or `t.me` link in `channels`. This reads the public web preview, so there is nothing to install or authorise.

**Can it scrape private channels or groups?**
No. Only public channels that have a `t.me/s/<channel>` preview.

**Can I scrape Telegram comments?**
Yes, for channels that have comments switched on. Set `includeComments` to `true`. Each comment is its own row and every message gets a `commentCount`.

**How do I monitor a channel for new posts?**
Turn on `onlyNewMessages` and schedule the run. Each run returns, and charges, only what was posted since the last one.

**Can I search for a keyword inside a channel?**
Yes. Set `searchQuery` and only matching messages are returned. `excludeKeywords` drops messages with certain words.

**Can I get a single post?**
Put the link in `postUrls`, for example `https://t.me/durov/123`.

## More scrapers

Telegram is one of 23 no-login scrapers by Smoldesk Studios. See the full list: [smoldesk/apify-scrapers](https://github.com/smoldesk/apify-scrapers).

# Telegram Channel Scraper

Scrape public Telegram channels: messages, views, reactions, media, links and subscriber counts. No Telegram account, phone number, `api_id` or `api_hash`. Runs on [Apify](https://apify.com/smoldesk/telegram-channel-scraper).

## What you get

- **Messages**: text, date, views, forwards, hashtags, links and link previews.
- **Reactions**: each emoji with its count, plus the total.
- **Media**: photo and video URLs.
- **Channel info**: title, description, verified flag, subscribers, and counts of photos, videos and links.
- **Filters**: keyword search inside each channel, and a date range.
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
    "dateFrom": "2026-10-01",
    "includeChannelInfo": true
  }'
```

## Sample output

```json
{
  "type": "message",
  "channel": "durov",
  "messageId": 531,
  "url": "https://t.me/durov/531",
  "date": "2026-07-16T20:45:25+00:00",
  "text": "Telegram now has a Rich Text Editor...",
  "views": 5800000,
  "totalReactions": 95000,
  "videos": ["https://cdn4.telesco.pe/file/....mp4"],
  "forwardedFrom": null,
  "channelTitle": "Pavel Durov",
  "channelSubscribers": 10400000
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

Pay per result. No monthly fee.

| Result | Per 1,000 |
|---|---|
| Message | $1 |
| Channel info | $2 |

## Limits

- Public channels with web preview only. Private channels, groups and user chats are not reachable.
- Large counts are rounded the way Telegram shows them, so `10.4M` becomes `10400000`.
- Video URLs on Telegram's CDN expire. Download them soon after the run.

## FAQ

**Do I need a Telegram API key or phone number?**
No. There is no `api_id`, `api_hash`, phone login or session file. No account is used, so none can be banned.

**How do I scrape a Telegram channel without Telethon?**
Pass the channel name or `t.me` link in `channels`. This reads the public web preview, so there is nothing to install or authorise.

**Can it scrape private channels or groups?**
No. Only public channels that have a `t.me/s/<channel>` preview.

**How do I monitor a channel for new posts?**
Schedule a run every hour with `dateFrom` set to a recent date. Send the results to Slack, Google Sheets or a webhook.

**Can I search for a keyword inside a channel?**
Yes. Set `searchQuery` and only matching messages are returned.

## More scrapers

Telegram is one of 17 no-login scrapers by Smoldesk Studios. See the full list: [smoldesk/apify-scrapers](https://github.com/smoldesk/apify-scrapers).

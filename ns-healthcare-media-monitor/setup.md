# Setup: Nova Scotia Healthcare Media Monitor

Six steps in Zapier: a trigger plus five actions. No code.

## 1. RSS by Zapier, New Items in Multiple Feeds

Feed URLs (comma-separated). Defaults:

```
https://www.cbc.ca/webfeed/rss/rss-canada-novascotia
https://globalnews.ca/halifax/feed/
https://novascotia.ca/news/rss/rss.asp?dept=134
```

The third is the Government of Nova Scotia, Health and Wellness news-release feed. Leave dedupe on Different Guid / URL.

## 2. Filter by Zapier, keyword pre-filter

Continue if any of these is true (join every condition with OR). This drops non-health items before the paid AI step runs:

- Link contains `novascotia.ca` (passes all gov health releases, which have no item description)
- Title contains `health`, `medical`, or `hospital`
- Description contains `health`, `hospital`, `Nova Scotia Health`, `IWK`, `doctor`, `nurse`, or `care`

Filters are free, so this is the cost control that keeps the AI step from running on every article.

## 3. AI by Zapier, Gate & Score NS Healthcare

Use the prompt and the five structured output fields from [ai-prompt.md](ai-prompt.md). Keep the prompt short; the field descriptions carry the classification rules.

## 4. Filter by Zapier, category and relevance

Continue only if both are true (AND):

- `category` exactly matches `NS Healthcare`
- `relevance` is greater than `30`

Anything tagged `Skip` or scoring 30 or below stops here.

## 5. Digest by Zapier, Append Entry and Schedule Digest

- Title (digest key, keep constant): `NS Healthcare Digest`
- Entry:

```
• {{title}}  (Relevance {{relevance}}/100, {{sentiment}})
{{summary}}
{{link}}
```

- Frequency: Daily, 7:00am, Atlantic time
- Trigger on weekends: Yes
- Delete Digest on Release: Yes (so each morning starts fresh)

## 6. Slack, Send Channel Message

- Channel: your channel
- Message:

```
*Nova Scotia Healthcare: Daily Digest*

{{Current Digest}}
```

- Send as a bot: Yes
- Bot Name: NS Health Monitor
- Bot Icon: :hospital:
- Auto-Expand Links: No (keeps the digest compact)

## How it runs

Articles append to the digest all day. The digest releases once at 7am Atlantic with the last 24 hours of matches, then clears. Slack posts only once a day.

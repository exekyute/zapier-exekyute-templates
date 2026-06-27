# Setup: Blog Post to Social Media Drafts

Four steps in Zapier: a trigger plus three actions. No code.

## 1. RSS by Zapier, New Item in Feed

- Feed URL: your blog's RSS feed URL, e.g. `https://yourblog.com/feed`
- Leave Customize Feed Item at the defaults. Zapier will expose Title, URL, and Summary (or Content) as mappable fields.

If your feed does not populate Summary, check whether it uses a Content field instead. The RSS spec varies by platform. Map whichever field holds the post text to the `{{summary}}` placeholder in step 2.

## 2. AI by Zapier, Understand Text, Summarize, or Analyze

Paste the full prompt from [ai-prompt.md](ai-prompt.md) into the Instructions field.

Map the three trigger fields:

| Placeholder in prompt | Field to map |
|---|---|
| `{{title}}` | RSS: Title |
| `{{url}}` | RSS: URL |
| `{{summary}}` | RSS: Summary (or RSS: Content if Summary is blank) |

The step returns one field: `output`. That field holds the full text block with the `LINKEDIN:` and `TWITTER:` sections.

## 3. Notion, Create Database Item

Before configuring this step, create a Notion database called Social Drafts with these properties:

| Property | Type |
|---|---|
| Title | Title |
| Status | Select (add a Draft option) |
| Published URL | URL |

In Zapier, select Create Database Item and connect your Notion account.

- Database: select Social Drafts
- Title: map RSS: Title
- Status: type `Draft` (literal text)
- Published URL: map RSS: URL
- Page content / body: map AI by Zapier: Output

The AI output lands in the Notion page body as a text block. Both the LinkedIn and Twitter drafts are visible on the page.

## 4. Slack, Send Channel Message

- Channel: your content review channel, e.g. `#content`
- Message:

```
:pencil: New drafts ready for *{{title}}* - check Notion to review and schedule.
```

Map `{{title}}` to RSS: Title.

- Send as a bot: Yes
- Expand links: No

The Slack message fires every time a new post comes in. It does not branch on the AI output and does not link directly to the Notion page (Notion's shareable page URL is not available as a Zapier output at the time the Zap runs).

## How it runs

Each time a new item appears in the RSS feed, the Zap fires once. The AI step runs immediately. Notion creates the page and Slack posts the nudge in sequence. There is no batching, no filter, and no delay. If the feed publishes three posts in quick succession, three separate Notion pages and three Slack messages are created.

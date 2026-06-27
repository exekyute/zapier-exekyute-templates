# Blog Post to Social Media Drafts

A no-code automation that watches an RSS feed for new blog posts, generates a LinkedIn draft and an X/Twitter draft in one AI step, saves both to a Notion page for review, and posts a Slack notification.

Built with Zapier, RSS, an AI step, Notion, and Slack. No code, no scripting.

## What it does

When a new post appears in the feed it:

1. Picks up the post title, URL, and summary from the RSS trigger.
2. Sends the three fields to one AI step that returns a LinkedIn draft and an X/Twitter draft in a single output.
3. Creates a page in a Notion database with the post title, a Draft status, the published URL, and the full AI output as the page body.
4. Posts a Slack message in a content channel to signal the drafts are ready.

Example Notion page body:

```
LINKEDIN:
New research out on how smaller editorial teams are covering more ground with structured content processes. Three things stood out to me:

First, the shift from reactive to scheduled publishing changed how the team measured quality, not just volume.

Second, lightweight review gates, like a shared Notion inbox, kept drafts from going stale without adding a full approval layer.

Third, the biggest gains came from standardizing the brief, not the writing.

What does your team use as the handoff point between writing and scheduling?

TWITTER:
Most editorial gains come from fixing the brief, not the writing. New post on how smaller teams are doing more with structured content processes. https://example.com/post-slug
```

## How it works

| Stage | What happens |
|---|---|
| RSS trigger | Fires on each new item in the blog feed |
| AI step | Generates a LinkedIn draft and an X/Twitter draft from the post title, URL, and summary |
| Notion | Creates a page in the Social Drafts database with both drafts as the body |
| Slack | Posts a nudge to the content channel that drafts are ready |

One AI call returns both drafts in a single output block. Nothing posts to social automatically. The Notion page is the review gate: a person picks up from there before anything goes live.

## The prompt

The AI step prompt is in [ai-prompt.md](ai-prompt.md), ready to paste into Zapier's AI by Zapier step. The prompt instructs the model to return a `LINKEDIN:` section and a `TWITTER:` section as labeled plain text, which land together in the Notion page body.

## Setup

Full step-by-step is in [setup.md](setup.md). In short:

1. RSS by Zapier, New Item in Feed, paste your blog feed URL.
2. AI by Zapier, paste the prompt from [ai-prompt.md](ai-prompt.md) and map title, URL, and summary from the trigger.
3. Notion, Create Database Item, map title and URL to properties, paste the full AI output into the page body.
4. Slack, Send Channel Message, paste the notification text and set your channel.

## Configuring the Notion database

Create a Notion database called Social Drafts with these properties:

| Property | Type | Notes |
|---|---|---|
| Title | Title | Blog post title |
| Status | Select | Add a Draft option |
| Published URL | URL | Source post link |

The page body holds the AI output as a text block. You can add a `Platform` or `Scheduled` property later without touching the Zap.

## Stack

- Zapier for orchestration and delivery
- RSS by Zapier for the feed trigger
- AI by Zapier for draft generation
- Notion for review and scheduling
- Slack for notification

## What is in this folder

| File | What it is |
|---|---|
| `README.md` | This overview |
| `ai-prompt.md` | The AI prompt to paste into the Zapier AI step |
| `setup.md` | Step-by-step Zap configuration |

---

All sample data is fictional. No real credentials, IDs, or channels are included.

Part of the [zapier-exekyute-templates](../) collection. MIT licensed.

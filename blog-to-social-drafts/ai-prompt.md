# AI step: Generate LinkedIn and X/Twitter Drafts

The step takes the blog post title, URL, and summary and returns two platform drafts as labeled sections in a single text output. One call handles both platforms, so no loop or iteration is needed.

## Prompt

Paste this into the Zapier AI by Zapier step. Map the three trigger fields to the placeholders before saving.

```
You are a social media copywriter. Read the blog post details below and write two social media drafts. Return them as plain text with exactly the labels shown. No preamble, no explanation, no extra headings.

Title: {{title}}
URL: {{url}}
Summary: {{summary}}

LINKEDIN:
Write a 150 to 200 word professional post. Start with a hook on the first line that can stand alone. Follow with 2 to 3 short paragraphs covering the key takeaways from the post. End with one open question that invites a reply. No hashtags.

TWITTER:
Write a single post of 240 characters or fewer. Open with a punchy, specific statement. End with the URL. No hashtags.
```

Map these trigger fields to the placeholders:

| Placeholder | RSS trigger field |
|---|---|
| `{{title}}` | Title |
| `{{url}}` | URL |
| `{{summary}}` | Summary (use Content if Summary is empty) |

## Output field

The AI by Zapier step returns one field by default: `output`. That field holds the full text block, with both the `LINKEDIN:` and `TWITTER:` sections. Map `output` to the Notion page body in the next step.

No structured output fields are needed here because the two drafts are read by a person in Notion, not branched on by a downstream filter. Keeping it as one text block means the Notion page displays both drafts together for review.

## Why one output instead of structured fields

The downstream step is a human reviewer in Notion, not a filter or a branch. A single labeled text block is easier to read and edit than two separate fields on a Notion property. If you later want to split the drafts to separate Notion properties, add two more Notion properties (LinkedIn Draft, Twitter Draft) and use Zapier's text formatter to extract the content after each label.

# AI step: Write Celebration Message

The AI step takes the team member's name, event type, and year count and writes one short, human-sounding Slack message. Free text output works here because the message goes straight to Slack with no downstream branching on the content.

## Prompt

```
Write a warm, one-paragraph Slack message celebrating a team member. Keep it under 60 words. Do not use the word "heartfelt". Do not use an em dash.

If it is a birthday: wish them a happy birthday and mention something generic but human (like enjoying the day, or having cake).
If it is a work anniversary: acknowledge the milestone year count and say something genuine about longevity on a team.

Person's name: {{name}}
Event type: {{event_type}} (birthday or work anniversary)
Year count (for anniversaries): {{years}}
```

Map the three placeholders to the Google Sheets lookup fields:

| Placeholder | Zapier field to map |
|---|---|
| `{{name}}` | Name (from Lookup Row) |
| `{{event_type}}` | Event Type (from Lookup Row) |
| `{{years}}` | Years (from Lookup Row) |

For birthdays, `{{years}}` will be blank. The prompt handles that gracefully because it tells the model to use the year count only for anniversaries.

## Output

AI by Zapier returns one text response. Map that directly as the Slack message body. No structured fields needed.

# Setup: Anniversary and Birthday Nudge

Five steps in Zapier: a trigger plus four actions. No code.

## Before you start: prepare the Google Sheet

Create a new Sheet (or add a tab to an existing one) with these five columns in order:

| Column | Header text | Notes |
|---|---|---|
| A | Name | Full name of the team member |
| B | Event Type | Type exactly `birthday` or `work anniversary` |
| C | Month-Day | The date in `MM-DD` format, e.g. `03-15` for March 15 |
| D | Years | Number of years, for anniversaries. Leave blank for birthdays. |
| E | TODAY_MATCH | Formula. Do not type values here. |

Paste this formula into E2 and fill it down to every data row:

```
=IF(TEXT(TODAY(),"MM-DD")=C2,"yes","")
```

Adjust the `C2` reference to match the row you are in (C3, C4, and so on). When a row's Month-Day matches today's date, the cell returns `yes`. All other cells stay blank. The Zapier lookup step searches this column for `yes` to find the day's match.

To test a row before the Zap runs, temporarily change a Month-Day value to today's date and confirm the TODAY_MATCH cell shows `yes`. Change it back when you are done.

## 1. Schedule by Zapier, Every Day

- Trigger: Every Day
- Time of day: 8:00 AM
- Time zone: your local time zone

## 2. Google Sheets, Lookup Spreadsheet Row

- Spreadsheet: your team events Sheet
- Worksheet: the tab with the data
- Lookup Column: `TODAY_MATCH` (column E)
- Lookup Value: `yes`

This returns the first row where TODAY_MATCH is `yes`. If no row matches, the step returns an empty result and the next step stops the Zap.

## 3. Filter by Zapier

Continue only if:

- Name (from step 2) does not equal (Text) *(leave the value field blank)*

This exits the Zap cleanly on days with no events. No error is thrown and no blank message reaches Slack.

## 4. AI by Zapier, Understand Text, Summarize, or Analyze

Paste the prompt from [ai-prompt.md](ai-prompt.md) into the prompt field, then map the three placeholders:

| Placeholder in prompt | Map to |
|---|---|
| `{{name}}` | Name from step 2 |
| `{{event_type}}` | Event Type from step 2 |
| `{{years}}` | Years from step 2 |

No structured output fields. The AI returns one text response.

## 5. Slack, Send Channel Message

- Channel: `#general` (or whichever channel you use for team announcements)
- Message: map the AI output from step 4
- Send as a bot: Yes

## How it runs

The Zap fires once each morning at 8 AM. It looks up the Sheet, checks for a match, and either posts a Slack message or exits with no action. One Zap run per day, one message per day at most.

If two people share a birthday or anniversary date, the Lookup Row step returns whichever row the Sheet finds first. Duplicate the Zap or use a Looping by Zapier approach if you need to handle multiple matches on the same day.

> **Draft** — Screenshots and final walkthrough pending.

# Anniversary and Birthday Nudge

Each morning at 8 AM, this Zap checks a Google Sheet for anyone on the team with a birthday or work anniversary today and sends a personalized Slack message for the match.

Built with Zapier, Google Sheets, AI by Zapier, and Slack. No code, no scripting.

## Why I built it

I kept forgetting to acknowledge team birthdays and work anniversaries until after the fact. A reminder in my calendar was fine for one or two people, but it does not scale when the team grows and it does not send the actual message. I wanted the Zap to handle the lookup, write something that does not sound canned, and post it automatically.

## What it does

Every morning at 8 AM it:

1. Triggers on a daily schedule.
2. Looks up the Sheet for a row where `TODAY_MATCH` equals `yes` (a formula in the sheet handles the date comparison).
3. Exits cleanly if no match is found, with no error and no blank Slack message.
4. Sends the person's name, event type, and year count to an AI step that writes a short, human-sounding message.
5. Posts the message to a Slack channel.

Example output:

```
Happy birthday, Priya! Hope you take some time today to do something you actually enjoy, cake very much included.
```

```
Three years, Marcus. That is a real stretch of time on a team, and it means something. Glad you are still here.
```

## How it works

| Stage | What happens |
|---|---|
| Schedule trigger | Fires at 8 AM in the user's local time zone |
| Google Sheets lookup | Searches the `TODAY_MATCH` column for `yes` |
| Filter | Stops the Zap if the Name field is empty (no match today) |
| AI by Zapier | Writes a warm, short Slack message from the person's details |
| Slack | Posts the message to a channel |

The Sheet formula does the date matching, not Zapier logic. That keeps the Zap simple and makes it easy to test by temporarily editing a row.

## The data model

The Sheet has five columns:

| Column | What goes here |
|---|---|
| Name | Team member's full name |
| Event Type | `birthday` or `work anniversary` |
| Month-Day | The date in `MM-DD` format (e.g. `03-15`) |
| Years | Number of years, for anniversaries. Leave blank for birthdays. |
| TODAY_MATCH | Formula (see below). Do not edit this column manually. |

Formula for the `TODAY_MATCH` column (paste into every row, adjusting the row number):

```
=IF(TEXT(TODAY(),"MM-DD")=C2,"yes","")
```

Where `C2` is the Month-Day column. When the date in that row matches today, the cell returns `yes`. All other cells stay blank. The Zapier lookup step searches this column for `yes` to find the day's match.

## Setup

Full step-by-step is in [setup.md](setup.md). In short:

1. Create the Sheet with the five columns above and add your team's data.
2. Schedule by Zapier, trigger at 8 AM.
3. Google Sheets, Lookup Spreadsheet Row, search `TODAY_MATCH` for `yes`.
4. Filter by Zapier, continue only if Name is not empty.
5. AI by Zapier, paste the prompt from [ai-prompt.md](ai-prompt.md).
6. Slack, Send Channel Message, map the AI output as the message body.

## One event per day

Zapier's Lookup Row returns one match. This template handles one birthday or anniversary per day. If two people share a date, the Zap picks up whichever row the Sheet returns first.

If your team is large enough that collisions are common, duplicate the Zap (one per row) or switch to a multi-row solution using Google Sheets "Find Many" rows with a Looping by Zapier step.

## Stack

- Schedule by Zapier for the daily trigger
- Google Sheets for the event data and date matching
- Filter by Zapier for the no-match exit
- AI by Zapier for the message text
- Slack for delivery

## What is in this folder

| File | What it is |
|---|---|
| `README.md` | This overview |
| `ai-prompt.md` | The AI prompt ready to paste |
| `setup.md` | Step-by-step Zap configuration |

---

All sample data is fictional. No real credentials, IDs, or channels are included.

Part of the [zapier-exekyute-templates](../) collection. MIT licensed.

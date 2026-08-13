# Setup: Project Health Roll-up

Six steps in Zapier: a trigger plus five actions. No code.

The two Storage steps bracket the Zap. Step 2 reads what yesterday produced, step 6 writes what today produced, and both use the same constant key. Build them together or change detection has nothing to compare against.

## 1. Schedule by Zapier, Every Day

- Trigger: Every Day
- Time of day: 7:30 AM
- Trigger on weekends: No

The step returns a date field. Step 4 maps it in as today's date, so the standup can stamp itself and the scoring rules have an anchor for their day counts.

## 2. Storage by Zapier, Get Value

- Key: a constant you choose and never change, for example `standup-yesterday`
- Default Value: `FIRST RUN - no prior standup`

The key has to stay identical here and in step 6. The default is what makes the first run behave: with nothing stored yet, the AI step receives the words FIRST RUN and the prompt tells it to report no changes rather than invent them.

Storage by Zapier uses a secret that acts as the account for your stored values. Keep it out of anything committed.

## 3. Notion, API Request (Beta)

The built-in Notion actions read pages one at a time. Querying a whole database in one call needs the raw API, which is what this action is for. The Notion connection supplies the auth, so you only set the request.

- HTTP Method: `POST`
- URL: `https://api.notion.com/v1/databases/<NOTION_DB_ID>/query`
- Body:

```json
{
  "filter": {
    "property": "Status",
    "select": { "equals": "Active" }
  }
}
```

Replace `<NOTION_DB_ID>` with your own database ID and keep the real value out of the repo. If your Status column is a Status property rather than a Select property, swap the `select` key for `status`; Notion rejects the filter otherwise. If the call comes back complaining about the API version, add a `Notion-Version` header of `2022-06-28`.

Share the database with your Notion connection before you test this step. An unshared database does not error, it returns an empty result list, so the Zap looks like it works and the standup comes back with nothing in it.

Filtering here rather than later is the cost control. On hold and Done rows never leave Notion, so the AI step is never paid to read them.

## 4. AI by Zapier, Analyze and Return Data

Use the prompt from [ai-prompt.md](ai-prompt.md) and map its three placeholders:

| Placeholder | Maps to |
|---|---|
| `{{TODAY}}` | The date from the Schedule step (step 1) |
| `{{YESTERDAY_DIGEST}}` | The value from Storage Get (step 2) |
| `{{PROJECTS_JSON}}` | The response body from the Notion request (step 3) |

This step returns one block of plain text rather than structured fields, because the text it returns is both the Slack message in step 5 and the value saved in step 6. The output format is what change detection runs on, so edit the shape in the prompt file and nowhere else.

## 5. Slack, Send Direct Message

- To: yourself, or whoever should get the standup
- Message Text: the output from step 4
- Send as a bot: Yes
- Bot Name: Project Standup
- Auto-Expand Links: No

Send Channel Message works just as well if the standup should be public. Nothing downstream depends on which one you pick.

## 6. Storage by Zapier, Set Value

- Key: the same constant as step 2
- Value: the output from step 4

Last step on purpose. Today's standup is only worth saving once it has actually been delivered.

## Testing it

Import `projects-day1.csv` into Notion, then follow [synthetic-data.md](synthetic-data.md). It gives eight fictional projects, the exact standup the Day 1 rows should produce, and two edits that should flip one project from red to green and another from green to red on the second run. Two of the eight rows are not Active and should never appear in the standup at all.

Run it twice. The first run has no stored value and should report no changes; the second is the one that proves the Storage pair works.

## How it runs

The Zap fires at 7:30 AM on weekdays, reads yesterday's standup out of Storage, pulls the Active projects from Notion, scores and diffs them in one AI step, posts the result to Slack, and writes that same text back to Storage for tomorrow. One task-consuming run per weekday morning, and one Slack message.

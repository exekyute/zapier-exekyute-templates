# Setup: CSAT / NPS Router

Six steps in Zapier: a trigger plus five actions across three Paths branches. No code.

Google Forms also works as the trigger via the Google Forms "New Response in Spreadsheet" trigger. Typeform is the recommended default because it returns the numeric score as a clean, directly mappable field without an intermediate lookup.

## 1. Typeform, New Entry

Connect your Typeform account and select the survey form.

Map these fields from the submission:

- `score`: the NPS or CSAT numeric question (0 to 10)
- `feedback`: the open-text question
- `respondent_email`: the email question
- `submitted_at`: the submission timestamp

If your form uses different question labels, map by question ID in the field picker.

## 2. Paths by Zapier

Add a Paths step with three branches. Each branch has its own filter condition.

**Path A: Detractor**

Filter condition:
```
score (Number) Less than or equal to 6
```

**Path B: Passive**

Filter conditions (both must be true):
```
score (Number) Greater than 6
score (Number) Less than 9
```

**Path C: Promoter**

Filter condition:
```
score (Number) Greater than or equal to 9
```

## 3a. Path A step 1: Slack, Send Channel Message

- Channel: `#cx-alerts` (or your CX alert channel)
- Message:

```
:red_circle: Detractor score *{{score}}/10* from {{respondent_email}} — "{{feedback}}"
```

- Send as a bot: Yes

## 3b. Path A step 2: Notion, Create Database Item

- Database: your shared survey response database
- Properties to set:

| Property | Value |
|---|---|
| Respondent Email | `{{respondent_email}}` |
| Score | `{{score}}` |
| Feedback | `{{feedback}}` |
| Status | Needs Follow-Up |
| Date | `{{submitted_at}}` |

## 4a. Path B step 1: Notion, Create Database Item

Same database as Path A. Properties:

| Property | Value |
|---|---|
| Respondent Email | `{{respondent_email}}` |
| Score | `{{score}}` |
| Feedback | `{{feedback}}` |
| Status | Logged |
| Date | `{{submitted_at}}` |

Passives are logged for trend analysis but do not require immediate action, so Path B stops here.

## 5a. Path C step 1: Gmail, Create Draft

- To: `{{respondent_email}}`
- Subject: `Thank you for your feedback`
- Body (paste this and edit before each send):

```
Hi there,

Thank you for taking the time to complete our survey and for your kind score. Feedback like yours helps us understand what we are doing well.

If you have a moment to share more, we would love to hear it.

Best,
[Your name]
```

The draft is never auto-sent. A rep opens it in Gmail, personalizes the greeting and sign-off, and sends it manually. This keeps the thank-you genuine.

## Notion database setup

Create a Notion database with these properties before connecting the Notion steps:

| Property | Type |
|---|---|
| Respondent Email | Email |
| Score | Number |
| Feedback | Rich text |
| Status | Select (options: Needs Follow-Up, Logged, Thanked) |
| Date | Date |

All three paths write to the same database so trend views and filters work across all segments.

## How it runs

The Zap fires once per Typeform submission. Zapier evaluates the Paths branches immediately and runs only the branch whose filter condition matches the score. A response with a score of 5 posts a Slack alert and creates a Notion row. A response with a score of 8 creates a Notion row only. A response with a score of 10 creates a Gmail draft.

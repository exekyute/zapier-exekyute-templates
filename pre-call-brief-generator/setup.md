# Setup: Pre-Call Brief Generator

Four steps in Zapier: a trigger plus three actions. No code.

## 1. Calendly, Invitee Created

Connect your Calendly account. No additional configuration is required at the trigger level. The trigger fires for every new invitee across all your event types.

If you want to limit the Zap to one event type (for example, product demos only), add a Filter by Zapier step after this trigger:

- Continue only if `Event Name` contains `Demo` (or whatever text matches your event type).

Fields used downstream:

| Field | Used in |
|---|---|
| Invitee Name | AI prompt, Slack message, Doc title |
| Invitee Email | AI prompt |
| Event Name | AI prompt, Slack message |
| Questions and Answers | AI prompt |
| Event Start Time | Google Docs title |

## 2. AI by Zapier, Understand Text, Summarize, or Analyze

Paste the full prompt from [ai-prompt.md](ai-prompt.md) into the Instructions field.

Map these Zapier fields to the placeholders in the prompt:

| Prompt placeholder | Map to |
|---|---|
| `{{invitee_name}}` | Step 1: Invitee Name |
| `{{invitee_email}}` | Step 1: Invitee Email |
| `{{event_name}}` | Step 1: Event Name |
| `{{questions_and_answers}}` | Step 1: Questions and Answers |

Leave Output Format as plain text. The four-section structure (WHO / CONTEXT / SUGGESTED QUESTIONS / WATCH FOR) is enforced by the prompt itself, so no structured output fields are needed here.

## 3. Google Docs, Create Document

- **Title:**

```
Call Brief -- {{invitee_name}} -- {{event_start_time}}
```

Map `invitee_name` from Step 1 and `event_start_time` from Step 1. Format the date as `YYYY-MM-DD` using Zapier's date formatter if needed (add a Formatter by Zapier step between Step 1 and this step, using Format Date, output format `YYYY-MM-DD`).

- **Content:** Map the AI output text from Step 2. This lands as the body of the Google Doc.

- **Folder (optional):** Set a destination folder in Google Drive (for example, `Sales / Call Briefs`). If left blank, the doc lands in the root of My Drive.

## 4. Slack, Send Channel Message

- **Channel:** `#sales` (or any channel where the rep should see the notification).
- **Message:**

```
:briefcase: Prep brief ready for *{{invitee_name}}* ({{event_name}}) -- {{google_doc_url}}
```

Map `invitee_name` and `event_name` from Step 1, and `google_doc_url` from Step 3 (the "Document URL" or "Sharing Link" field that Google Docs returns).

- **Send as bot:** Yes.
- **Bot name (optional):** `Call Brief Bot` or leave as the Zapier default.

## How it runs

The Zap fires within seconds of a Calendly booking being confirmed. The AI step runs once per booking. The Google Doc is created immediately, and the Slack message follows. There is no batching or scheduling: one booking produces one doc and one Slack message.

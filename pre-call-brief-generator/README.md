> **Draft** — Screenshots and final walkthrough pending.

# Pre-Call Brief Generator

A no-code automation that fires when a new Calendly booking comes in, writes a one-page prep brief with an AI step, saves it to Google Docs, and sends the link to a Slack channel.

Built with Zapier, Calendly, an AI step, Google Docs, and Slack. No code, no scripting.

## Why I built it

A Calendly invite arrives with a name, an email, and whatever the prospect typed in the custom questions. That information is enough to write a useful prep brief, but pulling it together manually before every call adds up. This Zap does it automatically the moment the booking is confirmed.

## What it does

When a new Calendly invitee is created it:

1. Picks up the invitee name, email, event name, and their answers to the custom booking questions.
2. Sends those fields to an AI step that writes a four-section brief: WHO, CONTEXT, SUGGESTED QUESTIONS, and WATCH FOR.
3. Creates a new Google Doc titled with the invitee name and the call date.
4. Posts a Slack message in the sales channel with the invitee name, event name, and a direct link to the doc.

Example Slack message:

```
:briefcase: Prep brief ready for *Jordan Mercier* (Product Demo - 30 min) - https://docs.google.com/document/d/example-doc-id
```

Example doc body:

```
WHO: Jordan Mercier (jordan@example.com), Head of Operations at Fieldstone Logistics.

CONTEXT: Jordan wants to understand how the product handles multi-site inventory syncing. They mentioned a specific pain point around end-of-month reconciliation.

SUGGESTED QUESTIONS:
- How many locations are you syncing today, and what does that process look like manually?
- What triggers the end-of-month reconciliation currently, and who owns it?
- If you could remove one step from that process, what would it be?

WATCH FOR: Jordan may be evaluating multiple tools at once. If they mention a competitor or a vendor shortlist, ask what criteria matter most before moving into a demo.
```

## How it works

| Stage | What happens |
|---|---|
| Calendly trigger | Fires when a new invitee completes a booking |
| AI step | Writes the four-section brief from the booking fields |
| Google Docs | Creates a doc with the brief as the body |
| Slack | Posts a message to the sales channel with the doc link |

The brief is created as a Google Doc so the rep can annotate it before the call. Nothing sends to the prospect automatically.

## The prompt

The AI step prompt is in [ai-prompt.md](ai-prompt.md), ready to paste into Zapier's AI by Zapier step. The four-section format (WHO / CONTEXT / SUGGESTED QUESTIONS / WATCH FOR) is fixed in the prompt so the output is scannable in under 60 seconds. The CONTEXT section is written in the prospect's own words where possible, pulled directly from the `questions_and_answers` trigger field.

## Calendly custom questions

The trigger field `questions_and_answers` captures whatever questions you have added to the Calendly event form. The recommended fields to add are:

| Question | Why it matters |
|---|---|
| Company name | Feeds the WHO section |
| Your role | Feeds the WHO section |
| What are you hoping to get from this call? | Feeds the CONTEXT and WATCH FOR sections |

If no custom questions are configured in Calendly, `questions_and_answers` will be empty and the AI will generate SUGGESTED QUESTIONS and WATCH FOR from the event name and invitee email domain only. The brief will be shorter but still useful.

## Setup

Full step-by-step is in [setup.md](setup.md). In short:

1. Calendly, Invitee Created, connect your Calendly account.
2. AI by Zapier, paste the prompt from [ai-prompt.md](ai-prompt.md) and map the four trigger fields.
3. Google Docs, Create Document, set the title and map the AI output as the content.
4. Slack, Send Channel Message, set the channel and paste the message template.

## Stack

- Zapier for orchestration
- Calendly for the booking trigger
- AI by Zapier for brief generation
- Google Docs for the output document
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

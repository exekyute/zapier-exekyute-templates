# CSAT / NPS Router

A no-code automation that reads a survey score when it comes in and routes the respondent to one of three outcomes: a Slack alert and a Notion follow-up item for detractors (0 to 6), a logged Notion entry for passives (7 to 8), and a Gmail draft for promoters (9 to 10).

Built with Zapier, Typeform, Paths, Notion, Slack, and Gmail. No code, no scripting.

![Zap flow](images/zap-overview.png)

## Why I built it

After every survey run, someone on the team has to open the results, find the low scores, and manually create follow-up tasks. Promoters rarely get a thank-you at all. I wanted one automation that handles the triage the moment a response comes in, with no manual review in the middle.

## What it does

When a survey response arrives it:

1. Picks up the submission from Typeform with the numeric score, feedback text, respondent email, and timestamp.
2. Sends the response through Paths by Zapier, which splits into three branches based on the score.
3. Detractors (0 to 6): posts a Slack alert to `#cx-alerts` and creates a Notion item with status "Needs Follow-Up".
4. Passives (7 to 8): creates a Notion item with status "Logged". No immediate action.
5. Promoters (9 to 10): creates a Gmail draft addressed to the respondent. A rep reviews and sends it.

Example output for a detractor:

```
Slack message in #cx-alerts:
:red_circle: Detractor score *4/10* from respondent@example.com — "The onboarding process was confusing and took too long."
```

Example Notion row:

```
Respondent Email: respondent@example.com
Score:            4
Feedback:         The onboarding process was confusing and took too long.
Status:           Needs Follow-Up
Date:             2026-06-27
```

## How it works

| Stage | What happens |
|---|---|
| Typeform trigger | Fires on each new survey submission and returns score, feedback, email, and timestamp |
| Paths by Zapier | Splits the response into three branches by score range |
| Path A (Detractor, 0 to 6) | Slack alert to `#cx-alerts` plus a Notion row with status "Needs Follow-Up" |
| Path B (Passive, 7 to 8) | Notion row with status "Logged", no other action |
| Path C (Promoter, 9 to 10) | Gmail draft to the respondent for a rep to review and send |

No AI step. The routing is purely numeric, so a deterministic Filter is the right tool: no cost per run, no classification errors, and the logic is easy to audit.

## Notion database

All three paths write to one shared Notion database, so the team has one place for trend data.

Recommended properties:

| Property | Type | Notes |
|---|---|---|
| Respondent Email | Email | Pulled from the Typeform submission |
| Score | Number | The raw 0 to 10 value |
| Feedback | Rich text | Open-text response |
| Status | Select | Needs Follow-Up, Logged, or Thanked |
| Date | Date | Submission timestamp from Typeform |

"Thanked" is available on the Status field so a rep can mark a promoter row once the Gmail draft has been reviewed and sent.

## Setup

Full step-by-step is in [setup.md](setup.md). In short:

1. Typeform, New Entry, connect your form and map score, feedback, respondent email, submitted at.
2. Paths by Zapier, three branches: score <= 6, score 7 to 8, score >= 9.
3. Path A: Slack Send Channel Message to `#cx-alerts`, then Notion Create Database Item with status "Needs Follow-Up".
4. Path B: Notion Create Database Item with status "Logged".
5. Path C: Gmail Create Draft addressed to the respondent.

## Stack

- Typeform for the survey trigger
- Paths by Zapier for score-based branching
- Notion for the shared response log
- Slack for detractor alerts
- Gmail for promoter thank-you drafts

## What is in this folder

| File | What it is |
|---|---|
| `README.md` | This overview |
| `setup.md` | Step-by-step Zap configuration |

---

All sample data is fictional. No real credentials, IDs, or channels are included.

Part of the [zapier-exekyute-templates](../) collection. MIT licensed.

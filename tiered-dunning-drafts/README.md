# Tiered Dunning Drafts

When an invoice is marked overdue in Xero, this Zap calculates how many days it has been past due, routes it into one of three aging buckets (30, 60, or 90+ days), and creates a tone-matched Gmail draft for each tier. No email is ever sent automatically.

Built with Zapier, Xero, and Gmail. No code, no scripting.

![Zap flow](images/zap-overview.png)

## Why I built it

Most overdue-invoice reminders either send the same generic email regardless of how late the invoice is, or require a paid AR tool to handle tiered escalation. This Zap does the routing with no extra tools. Three static templates give full control over the wording, and the draft-only approach means nothing goes out until someone reviews and approves it.

## What it does

When an invoice is updated to OVERDUE in Xero, it:

1. Confirms the invoice status is OVERDUE (catches updates that changed something other than status).
2. Calculates how many days the invoice is past its due date using Zapier Formatter.
3. Routes the invoice through a Paths step based on the aging bucket.
4. Creates a tone-matched Gmail draft for the matching tier: firm but polite at 30 days, direct at 60, escalated at 90+.

Example output (Path A, 30 days):

```
To: accounts@acmesupplies.ca
Subject: Friendly reminder -- Invoice INV-1042 due 2026-05-28

Hi Acme Supplies,

Just a quick note that invoice INV-1042 for 4,750.00 CAD was due on 2026-05-28.

If you have already sent payment, please disregard this message. Otherwise, you
can reply to this email or reach out if you have any questions.

Thank you.
```

Example output (Path C, 90+ days):

```
To: accounts@acmesupplies.ca
Subject: Urgent -- Invoice INV-1042 requires immediate attention

Hi Acme Supplies,

Invoice INV-1042 for 4,750.00 CAD is now significantly overdue
(original due date: 2026-05-28).

Please respond to this email within 48 hours with a payment confirmation or
a payment arrangement. If we do not hear from you, we may need to escalate
this matter.

Thank you.
```

## How it works

| Stage | What happens |
|---|---|
| Xero trigger | Fires when an invoice is updated; the Filter step narrows it to OVERDUE only |
| Filter by Zapier | Stops the Zap if the status is not OVERDUE |
| Formatter by Zapier | Calculates days overdue: diff between due date and today, in days |
| Paths by Zapier | Routes to Path A (1-30 days), Path B (31-60 days), or Path C (60+ days) |
| Gmail (x3) | Each path creates a draft with a different subject and body |

Every path ends in a draft, never a sent email. That is a deliberate choice: collection calls and relationship decisions belong to a person, not an automation.

## Aging tiers

| Path | Days overdue | Tone |
|---|---|---|
| A | 1 to 30 | Friendly reminder, assumes oversight |
| B | 31 to 60 | Direct follow-up, requests prompt payment |
| C | 61+ | Escalated notice, 48-hour response window |

## Setup

Full step-by-step is in [setup.md](setup.md). In short:

1. Xero, Updated Invoice trigger.
2. Filter by Zapier, status equals OVERDUE.
3. Formatter by Zapier, Date / Time, Diff Between Dates: due date vs. today, output in days.
4. Paths by Zapier, three paths on days-overdue ranges.
5. Gmail, Create Draft (one per path) with the subject and body for that tier.

## Stack

- Xero for the overdue invoice trigger
- Filter by Zapier to gate on invoice status
- Formatter by Zapier to calculate days overdue
- Paths by Zapier for tier routing
- Gmail for draft creation

## What is in this folder

| File | What it is |
|---|---|
| `README.md` | This overview |
| `setup.md` | Step-by-step Zap configuration |

---

All sample data is fictional. No real credentials, IDs, or email addresses are included.

Part of the [zapier-exekyute-templates](../) collection. MIT licensed.

# Setup: Tiered Dunning Drafts

Five steps in Zapier: a trigger plus four actions. No code.

## 1. Xero, Updated Invoice

Trigger: "Updated Invoice" in Xero.

Connect your Xero account. The trigger fires whenever an invoice is updated, including status changes. The Filter step in step 2 narrows it to OVERDUE only.

Fields used downstream:

| Xero field | Used as |
|---|---|
| `Contact Name` | Greeting and draft context |
| `Contact Email` | Gmail draft "To" address |
| `Invoice Number` | Subject line and body |
| `Amount Due` | Body copy |
| `Currency Code` | Body copy (e.g. CAD, USD) |
| `Due Date` | Date diff calculation and body copy |

**QuickBooks Online alternative:** Use the "Updated Invoice" or "New Invoice" trigger, filtered by status "Overdue" or the equivalent status field in your QBO account. Map the same fields from the QBO output.

**Canadian users on Xero:** Xero returns currency as a separate `Currency Code` field, so CAD invoices will display correctly in the draft body when the field is mapped.

## 2. Filter by Zapier

Continue only if:

- Invoice Status (from Xero) **exactly matches** `OVERDUE`

This stops the Zap when the trigger fires for other status changes (e.g. invoice marked paid, edited amount).

## 3. Formatter by Zapier, Date / Time

Event: **Diff Between Dates**

- Start Date: `Due Date` (from Xero)
- End Date: Today (use Zapier's built-in `Today` value or a second Formatter step for `{{zap_meta_human_now}}` formatted as a date)
- Output in: **Days**

The result is the number of days the invoice is past due. This value feeds the Paths logic in step 4.

## 4. Paths by Zapier

Three paths on the days-overdue value from step 3.

**Path A: 30 days**

Continue if: Days Overdue (from Formatter) **is greater than or equal to** `1` AND **is less than or equal to** `30`

Action inside Path A: Gmail, Create Draft

- To: `Contact Email`
- Subject:
```
Friendly reminder -- Invoice {{Invoice Number}} due {{Due Date}}
```
- Body:
```
Hi {{Contact Name}},

Just a quick note that invoice {{Invoice Number}} for {{Amount Due}} {{Currency Code}} was due on {{Due Date}}.

If you have already sent payment, please disregard this message. Otherwise, you can reply to this email or reach out if you have any questions.

Thank you.
```

**Path B: 60 days**

Continue if: Days Overdue **is greater than** `30` AND **is less than or equal to** `60`

Action inside Path B: Gmail, Create Draft

- To: `Contact Email`
- Subject:
```
Second notice -- Invoice {{Invoice Number}} now 30+ days overdue
```
- Body:
```
Hi {{Contact Name}},

Invoice {{Invoice Number}} for {{Amount Due}} {{Currency Code}}, due on {{Due Date}}, remains outstanding.

Please arrange payment at your earliest convenience or contact us to discuss.

Thank you.
```

**Path C: 90+ days**

Continue if: Days Overdue **is greater than** `60`

Action inside Path C: Gmail, Create Draft

- To: `Contact Email`
- Subject:
```
Urgent -- Invoice {{Invoice Number}} requires immediate attention
```
- Body:
```
Hi {{Contact Name}},

Invoice {{Invoice Number}} for {{Amount Due}} {{Currency Code}} is now significantly overdue (original due date: {{Due Date}}).

Please respond to this email within 48 hours with a payment confirmation or a payment arrangement. If we do not hear from you, we may need to escalate this matter.

Thank you.
```

## How it runs

The Zap fires each time Xero updates an invoice. If the status is OVERDUE, Formatter calculates how many days past due, Paths selects the right tier, and Gmail creates one draft. The draft sits in your Sent / Drafts folder until you review and send it manually. No email is sent automatically.

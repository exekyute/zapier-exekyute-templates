# AI step: Write Prep Brief

The AI by Zapier step takes the four Calendly booking fields and returns a four-section prep brief as a single text block. The fixed section headings (WHO, CONTEXT, SUGGESTED QUESTIONS, WATCH FOR) make the output scannable without any downstream parsing.

## Prompt

```
You are a sales rep preparing for a discovery call. Write a concise one-page prep brief using the following booking details.

Format the output with these sections:
WHO: Name, email, company, and role in one sentence.
CONTEXT: What they said they want from the call (1-2 sentences, use their words where possible).
SUGGESTED QUESTIONS: Three open-ended discovery questions tailored to their context.
WATCH FOR: One potential objection or concern based on what they shared, and a suggested way to address it.

Booking details:
Name: {{invitee_name}}
Email: {{invitee_email}}
Event: {{event_name}}
Their answers: {{questions_and_answers}}
```

Map the four placeholders to the Calendly trigger fields:

| Placeholder | Calendly field |
|---|---|
| `{{invitee_name}}` | Invitee Name |
| `{{invitee_email}}` | Invitee Email |
| `{{event_name}}` | Event Name |
| `{{questions_and_answers}}` | Questions and Answers |

## Note on empty answers

If the Calendly event has no custom questions configured, `questions_and_answers` will be empty. The AI will still produce SUGGESTED QUESTIONS and WATCH FOR based on the event name and email domain, but the WHO and CONTEXT sections will be thinner. Adding at least one custom question ("What are you hoping to get from this call?") gives the AI enough to write a useful CONTEXT and a grounded WATCH FOR.

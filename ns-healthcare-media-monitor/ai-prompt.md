# AI step: Gate & Score NS Healthcare

The monitor uses one AI by Zapier step with structured output fields. The prompt supplies the article; the output fields define what comes back, so the downstream filter can branch on clean values instead of parsing text.

## Prompt

```
You are a media monitor for healthcare news in Nova Scotia, Canada. Analyze the article below and fill in the output fields. If it is not about healthcare in Nova Scotia, set category to "Skip".

Title: {{article title}}
Source: {{article link}}
Content: {{article description}}
```

Map the three placeholders to the RSS trigger fields (Title, Link, Description).

## Output fields

| Field | Type | Description |
|---|---|---|
| `category` | Text, required | Exactly `NS Healthcare` if the article is about healthcare in Nova Scotia (hospitals, ERs, Nova Scotia Health, IWK, doctors, nurses, wait times, long-term care, mental health, public health, or NS health policy / funding). Otherwise exactly `Skip`. |
| `relevance` | Number | Integer 0 to 100, how central Nova Scotia healthcare is to the article. |
| `sentiment` | Text | Exactly one of: Positive, Neutral, Negative. |
| `entities` | Text | Comma-separated organizations or people mentioned (e.g. Nova Scotia Health, IWK), or `None`. |
| `summary` | Text | One plain sentence summarizing the article. No line breaks. |

## Why structured fields instead of one text response

Returning labeled fields lets the next filter read `category` and `relevance` directly, with no fragile delimiter parsing. `category` drives the keep-or-skip decision; `relevance` sets the noise threshold. Keep the prompt short and let the field descriptions carry the rules, otherwise the model tends to wrap everything back into a single blob.

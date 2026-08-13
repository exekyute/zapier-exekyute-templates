# How these templates get built

Every Zap in this repo comes out of the same working method. Pick one operational pain, build the smallest Zap that closes it, and keep the running cost of it visible in the design rather than discovered at the end of the month. Writing the method down once, from the first design question to the shared template link, keeps the individual READMEs about their own subject. The habits below are defaults, not a checklist; where a template breaks one on purpose, its README says so.

## Free steps before paid steps

Zapier meters tasks, so the first design question is not what the Zap should do. It is which steps are allowed to run. A Filter step costs nothing. An AI step is an app action and costs a task every time it fires.

That ordering is the whole shape of the [NS Healthcare Media Monitor](ns-healthcare-media-monitor/). A keyword filter sits between the RSS trigger and the AI step and drops the obvious non-health items, so the paid classification only sees the handful of articles that survive it. Its setup file states the reason in one line: filters are free, so this is the cost control that keeps the AI step from running on every article. On a slow news day that filter changes nothing. On a busy one it is the difference between a working Zap and a spent task budget.

Batching is the same decision wearing a different hat. A Digest step collects matches all day and releases one message, which cuts the Slack action count from one per article to one per day and cuts the channel noise by the same ratio.

## Rules before models

The next question is whether the job needs judgment or just rules. Most of the time it is rules, and a Filter or a Formatter step settles it deterministically, for free, with the condition sitting right there on the canvas where anyone can read it.

A model earns its slot when the task really is judgment: deciding whether a story is about healthcare in this province, or reading a progress note and saying whether a project is in trouble. When one is in the Zap, two habits attach.

The prompt stays short and the rules live in the output field descriptions. The media monitor's prompt is one instruction and three mapped fields; the classification rules sit in the description of the `category` field, which names every qualifying subject and the exact string to return otherwise. Structured fields are what let the next filter read `category` and `relevance` directly, with no delimiter parsing, and a field description is the closest thing a no-code Zap has to a rules table you can read. Keep the prompt long and the rules loose and the model folds everything back into one blob.

Second, the prompt carries the invariants that block a failure you have already seen. The [Project Health Roll-up](project-health-rollup/) tells the model that every project appears in exactly one color section and that the count in the green header must equal the number of names listed, because a model handed three buckets will eventually file one project in two of them.

The roll-up takes the opposite side of the structured-fields default on purpose. Its AI step returns one block of plain text, because that block is both the Slack message and the value written to Storage for tomorrow's comparison. Split it into fields and you would have to reassemble the same shape before saving it, so the format is load-bearing and the prompt says so.

## Scope restraint

A Zap that reads a system of record does not write back to it unless writing back is the point. Notion is read-only in the roll-up: the Zap reads project fields, scores them, and posts to Slack, and the Health column in Notion stays whatever a person set it to. The score is an opinion delivered to a channel, not a change applied to the source.

I draw the line the same way each time. The Zap does the reading, the sorting, and the comparison, and the change with a blast radius stays with a person.

## Design defaults

Six habits recur. Not every Zap needs all six, but each build starts from them and drops one only with a reason.

| Default | In practice |
|---|---|
| A free step in front of a paid one | A Filter runs before the AI step or the API request, so the metered step only sees records that earned it |
| Rules in one editable place | Thresholds and day counts sit in a filter condition or in the prompt file, so tuning is one field and not a rebuild |
| Structured fields over a text blob | An AI step returns named fields the next filter reads directly, with no delimiter parsing |
| State that survives the run | Storage by Zapier holds what the last run produced, so the next message can open with what changed |
| A deliberate noise level | Batched to one message a day, or posted per event, chosen per template and stated in its README |
| Read, decide, hand off | The Zap reads the source and posts the result; changing the source of record stays a human job |

Each default names the failure it exists to block. The state default is the one that separates these from a plain scheduled reminder: a Storage Get at the top and a Storage Set at the bottom, both on a constant key, are what let the roll-up lead with the two projects that moved overnight instead of restating all six every morning. The first run returns FIRST RUN and the prompt is told to treat that as no prior data, because a missing key should read as a known state rather than an error.

## Testing before turning it on

A Zap is off until you turn it on, and nothing here goes on without a watched run first. Every step gets tested in the editor against real trigger data, and the AI step gets read, not skimmed, on output that has not been cherry-picked.

Where the output is a judgment rather than a value, the test is a seeded dataset with the expected result written down before the run. The roll-up ships `projects-day1.csv` and a `synthetic-data.md` that anchors eight fictional projects to a fixed date, two of them deliberately not Active so you can confirm they never reach the standup, then gives the exact standup those rows should produce. It follows with two edits, one clearing a blocker and one letting a milestone slip, that should flip one project from red to green and another from green to red. It also names the tolerance: the wording of the reasons varies run to run, and what has to hold is the colors and the two transitions.

That is a weaker guarantee than an assertion count, and it is the honest ceiling of a no-code build. There is no test runner in this repo and nothing here is asserted automatically. The check is a person reading a seeded run against a written expectation, which catches a broken rule and will not catch a slow drift.

## The Zap as documentation

Zapier has no sticky notes, so the documentation load falls on two things: step names and the canvas screenshot. Steps get renamed off their defaults to say what they do, `Gate & Score NS Healthcare` rather than `AI by Zapier`, and every template folder carries a `zap-overview.png` of the canvas. The README's stage table then uses the same order and the same language as the steps in that screenshot, so the doc and the Zap never describe two different automations.

Nothing enforces that match. Both templates in this repo have taken a commit correcting their docs against the live Zap config after the fact, which is the argument for keeping the stage table thin enough to check by eye in under a minute.

## What the README owes the reader

Structure is fixed across templates: Use it when, How it works, Requirements, Setup, Customize, and the folder inventory. Inside that structure, every README owes the reader four things.

- A stage table matching the Zap step for step.
- One rationale sentence naming the choice and what it cost to make it.
- Trade-offs stated in both directions. Raising the media monitor's relevance gate above 30 cuts the noise and drops the marginal stories with it, and the README says which knob does that and where it lives.
- Stated limits. The roll-up admits its scoring is only as good as the notes people write, and that it costs about two minutes a week of jotting to keep those notes worth reading. A README with no caveat in it did not look hard enough.

The files split along the same line. The prompt and its output fields live in `ai-prompt.md` rather than buried in the setup, because the prompt is the thing most likely to be edited and it should be the easiest file to find. The step-by-step lives in `setup.md`, and the README's Setup section stays short and points at it.

## Shipping and lifecycle

Nothing reaches the repo unscrubbed. Storage secrets, Notion database IDs, real channel names, and recipient addresses are stripped or swapped for placeholders, and the `.gitignore` blocks the local files that hold the real values so they cannot be staged by accident. All sample data is fictional.

A finished Zap can ship twice. It ships as a folder here that a reader rebuilds step by step and can edit anywhere, and it ships as a shared Zapier template link that imports the whole thing and prompts for the parts that should change. The media monitor's link opens with the feed URLs, relevance threshold, digest time, and channel editable, and the classification logic locked. A shared link carries no credentials: whoever imports it connects their own accounts. The two routes are not equivalent, and the README says which one a given change needs. Retargeting that monitor at a different beat means editing the prompt and its five output fields, so it is a rebuild from the folder, not a guided import.

Every template ends at the same place. A stranger can import it or rebuild it, read every rule it applies, see which steps cost a task, and know what it will not touch on their behalf.

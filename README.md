# Zapier Templates

A collection of no-code Zapier automations, each a self-contained, importable template. Every folder has its own README, the AI prompts where used, and fictional sample data.

Built with Zapier plus Notion, Slack, RSS, and AI steps. No code, no scripting.

Both share the same working habits: a free Filter step in front of anything that costs a task, thresholds kept in one editable place instead of scattered through the steps, and Storage holding what the last run produced so the next message can open with what changed. Each README states its own limits too. The roll-up says how much weekly upkeep its scoring leans on, and the monitor names the knob that decides which marginal stories it drops. The full method is written down in [HOW-I-BUILD.md](HOW-I-BUILD.md).

## Templates

**2 templates**, one folder each. Both are also published as shared Zaps; hit **Import** on a row to open it on Zapier.

| Template | What it does | Stack |
|---|---|---|
| [Project Health Roll-up](project-health-rollup/) · [Import](https://zapier.com/templates/details/daily-project-health-standup-notion-to-slack-with-ai-scoring-75c23a?secret=MTp0ZW1wbGF0ZTpKRkhVVDVkR1hXMDhkWk9Bb0xVTTdfY182ZURaTlVISTBWQW55X1NmMno4OmVlcnhiMw) | Reads active projects from Notion each morning, scores each Green / Yellow / Red with AI, highlights what changed overnight, and posts one standup to Slack. | Schedule, Notion, AI, Storage, Slack |
| [Nova Scotia Healthcare Media Monitor](ns-healthcare-media-monitor/) · [Import](https://zapier.com/templates/details/monitor-nova-scotia-healthcare-news-and-post-a-daily-slack-digest-302b01?secret=MTp0ZW1wbGF0ZTpxSEZ2TVBtSTQ1VFhESEVtUUdWekE1Y0pBSmF3cG9uNlRpdllxb0t0clZNOncxZ2hyZw) | Watches Nova Scotia news feeds, uses AI to flag healthcare stories, batches them, and posts one digest to Slack every morning. | RSS, Filter, AI, Digest, Slack |

## How these are organized

Each template folder is independent:

- `README.md`: what it does, how it works, and setup.
- `ai-prompt.md`: the AI step prompt and output fields, where the template uses AI.
- `setup.md`: step-by-step Zap configuration.
- `images/`: canvas and output screenshots.
- sample data files where relevant.

All sample data is fictional. No real credentials, IDs, or private channels are included anywhere in this repo.

## License

MIT. See [LICENSE](LICENSE).

Built by Kevin Yu ([exekyute](https://github.com/exekyute)).

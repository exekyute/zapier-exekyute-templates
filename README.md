# Zapier Templates

A collection of no-code Zapier automations, each a self-contained, importable template. Every folder has its own README, the AI prompts where used, and fictional sample data.

Built with Zapier plus Notion, Slack, RSS, and AI steps. No code, no scripting.

## Templates

| Template | What it does | Stack |
|---|---|---|
| [Project Health Roll-up](project-health-rollup/) | Reads active projects from Notion each morning, scores each Green / Yellow / Red with AI, highlights what changed overnight, and posts one standup to Slack. | Schedule, Notion, AI, Storage, Slack |
| [Nova Scotia Healthcare Media Monitor](ns-healthcare-media-monitor/) | Watches Nova Scotia news feeds, uses AI to flag healthcare stories, batches them, and posts one digest to Slack every morning. | RSS, Filter, AI, Digest, Slack |

## How these are organized

Each template folder is independent:

- `README.md`: what it does, how it works, and setup.
- `ai-prompt.md`: the AI step prompt and output fields, where the template uses AI.
- `setup.md`: step-by-step Zap configuration, where included.
- `images/`: canvas and output screenshots.
- sample data files where relevant.

All sample data is fictional. No real credentials, IDs, or private channels are included anywhere in this repo.

## License

MIT. See [LICENSE](LICENSE).

Built by Kevin Yu ([exekyute](https://github.com/exekyute)).

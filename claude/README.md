# Meadow for Claude

[Meadow](https://findmeadow.com) is a social publishing workspace. Connect your channels, create once, and schedule the right version of every post from one Meadow workspace. Meadow publishes to Instagram, TikTok, YouTube, Facebook, X, LinkedIn, Pinterest, Threads, Bluesky, Telegram, and Google Business.

This plugin connects Claude to Meadow's MCP server so Claude can review your workspace and save new post drafts. It works in Claude Code, Cowork, and claude.ai.

## What it can do

- Identify the Meadow workspace your API key belongs to
- List your projects and the social accounts connected to each one
- Browse drafts, scheduled posts, and publishing history, and read a single post with its per-account delivery status
- Read cached analytics totals for a project and each connected account
- Save a new post as a draft for you to review in Meadow

The plugin never publishes, schedules, or deletes posts, and it doesn't connect accounts or upload media. You do those in the Meadow app at [app.findmeadow.com](https://app.findmeadow.com).

## What's included

| Component | Purpose |
| --- | --- |
| `meadow` MCP server | Remote Streamable HTTP server at `https://findmeadow.com/mcp` with seven tools: `get_profile`, `list_projects`, `list_accounts`, `list_posts`, `get_post`, `create_draft`, and `get_analytics` |
| `meadow` skill | Tells Claude how to pick a project, save drafts safely with an idempotency key, and read post statuses and analytics accurately |

Every tool is annotated. Six are read-only; `create_draft` is additive and idempotent.

## Setup

1. Sign in to [Meadow](https://app.findmeadow.com) and open **Configuration > API Keys**.
2. Select **Create API key** and copy the key. It starts with `br_live_` and is shown only once.
3. Enable the plugin. Claude asks for the key, masks it as you type, and stores it in your system's secure credential store.

To stop access, revoke the key in Meadow. The next request with that key is rejected.

## Network and data use

The plugin makes requests to one endpoint, `https://findmeadow.com/mcp`, operated by Meadow. Each request sends the API key you entered in the `Authorization` header and the tool arguments Claude chooses, such as a project ID or draft caption. The plugin contains no scripts or hooks, reads nothing from your machine, and sends data nowhere else.

## Privacy policy

Meadow handles the data this plugin sends under its [privacy policy](https://findmeadow.com/privacy/) and [terms of service](https://findmeadow.com/terms/).

## Support

Email [hello@findmeadow.com](mailto:hello@findmeadow.com) or visit [findmeadow.com](https://findmeadow.com).

## License

[MIT](LICENSE) © 2026 WoodBark Software LLC

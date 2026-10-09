# Meadow plugin

![Meadow](assets/logo.png)

[Meadow](https://findmeadow.com) is a social publishing workspace. Connect your channels, create once, and schedule the right version of every post from one Meadow workspace. Meadow publishes to Instagram, TikTok, YouTube, Facebook, X, LinkedIn, Pinterest, Threads, Bluesky, Telegram, and Google Business.

This plugin connects your AI agent to Meadow's MCP server so it can review your workspace, upload media, save drafts, and publish or schedule posts you confirm. It works with Cursor and Grok Build from the repository root. The Claude plugin lives in the [`claude/`](claude/) folder, and the ChatGPT and Codex plugin, which signs in to Meadow with OAuth instead of an API key, lives in [`openai/`](openai/).

## What it can do

- Identify your Meadow workspace, list its projects, and list the social accounts connected to each one
- Browse drafts, scheduled posts, and publishing history, and read a single post with its per-account delivery status
- Read cached analytics totals for a project and each connected account
- Upload images, videos, PDFs, Word and PowerPoint files to a project
- Save a post as a draft for you to review in Meadow
- Check a post against each selected platform's rules without publishing it
- Publish a post now or schedule it, after you confirm the exact content, accounts and time

Publishing posts publicly on your connected accounts. The bundled skill tells the agent to ask you for each platform's privacy, audience and consent choices instead of choosing them, to check the post with `preview_post` first, and to wait for your confirmation before it calls `publish_post` or `publish_draft`.

The plugin doesn't connect or remove social accounts, edit or cancel queued posts, delete posts, or refresh analytics. You do those in the Meadow app at [app.findmeadow.com](https://app.findmeadow.com).

## What's included

| Component | Purpose |
| --- | --- |
| `meadow` MCP server | Remote Streamable HTTP server at `https://findmeadow.com/mcp` with 13 tools. Read-only: `get_profile`, `list_projects`, `list_accounts`, `list_posts`, `get_post`, `get_analytics`, `get_account_options`, `preview_post`. Additive: `upload_media`, `create_upload_url`, `create_draft`. Publishing: `publish_post`, `publish_draft` |
| `meadow` skill | Tells the agent how to pick a project, save drafts with an idempotency key, check and confirm a post before publishing, and read post statuses and analytics accurately |

Every tool is annotated. `publish_post` and `publish_draft` are marked as destructive (they post publicly) and idempotent: retrying with the same `requestId` returns the original post instead of posting twice.

## Setup

1. Sign in to [Meadow](https://app.findmeadow.com) and open **Configuration > API Keys**.
2. Select **Create API key** and copy the key. It starts with `br_live_` and is shown only once.
3. Install the plugin and give it the key:
   - **Claude**: install the plugin in [`claude/`](claude/). It asks for the key when you enable it and stores it in your system's secure credential store.
   - **Claude Code**: run `claude plugin marketplace add MalikElate/meadow-plugin`, then `claude plugin install meadow@meadow`.
   - **Cursor**: set the `MEADOW_API_KEY` variable under **Plugins > Configure**.
   - **Grok Build**: set the `MEADOW_API_KEY` environment variable before starting Grok Build.

To stop access, revoke the key in Meadow. The next request with that key is rejected.

## Network and data use

The plugin makes requests to one endpoint, `https://findmeadow.com/mcp`, operated by Meadow. Each request sends your API key in the `Authorization` header and the tool arguments the agent chooses, such as a project ID, a caption, or a media file to upload. When you ask the agent to upload media from a link, Meadow's server downloads that link. Meadow then sends the posts you publish to the social platforms you selected. The plugin contains no scripts or hooks, reads nothing else from your machine, and sends data nowhere else.

Meadow handles that data under its [privacy policy](https://findmeadow.com/privacy/) and [terms of service](https://findmeadow.com/terms/).

## Support

Email [hello@findmeadow.com](mailto:hello@findmeadow.com) or visit [findmeadow.com](https://findmeadow.com).

## License

[MIT](LICENSE) © 2026 WoodBark Software LLC

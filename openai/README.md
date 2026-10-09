# Meadow for ChatGPT and Codex

[Meadow](https://findmeadow.com) is a social publishing workspace. Connect your channels, create once, and schedule the right version of every post from one Meadow workspace. Meadow publishes to Instagram, TikTok, YouTube, Facebook, X, LinkedIn, Pinterest, Threads, Bluesky, Telegram, and Google Business.

This package is the Meadow plugin for ChatGPT and Codex, in the [Agent Plugins](https://agent-plugins.org) format. It connects to Meadow's MCP server at `https://findmeadow.com/mcp` and signs in with your Meadow account through OAuth, so there's no API key to copy. The bundled `meadow` skill tells the assistant how to use the 13 Meadow tools safely.

## What it can do

- Review your projects, connected social accounts, drafts, scheduled posts, publishing history, and cached analytics
- Upload images, videos, PDFs, Word and PowerPoint files, including files you attach in ChatGPT
- Save drafts, check a post against each platform's rules, and publish now or schedule posts after you confirm the exact content, accounts and time

It doesn't connect or remove social accounts, edit or cancel queued posts, delete posts, or refresh analytics. Do those in the Meadow app at [app.findmeadow.com](https://app.findmeadow.com).

## Permissions

Sign-in asks for four Meadow permissions: `meadow:read` (read your workspace), `meadow:draft` (save drafts), `meadow:media` (upload media), and `meadow:publish` (publish and schedule posts). They only reach your own Meadow workspace.

## Data use

Requests go only to `https://findmeadow.com/mcp`, operated by Meadow, and carry the tool arguments the assistant chooses, such as a caption or a media file. Meadow sends the posts you publish to the platforms you selected, under its [privacy policy](https://findmeadow.com/privacy) and [terms of service](https://findmeadow.com/terms).

## Support

Email [hello@findmeadow.com](mailto:hello@findmeadow.com) or visit [findmeadow.com](https://findmeadow.com).

## License

[MIT](LICENSE) © 2026 WoodBark Software LLC

---
name: meadow
description: Work with a Meadow social publishing workspace through the Meadow MCP server. Use when the user asks about their Meadow projects, connected social accounts, drafts, scheduled or published posts, delivery status, or analytics, or wants to save new post content as a Meadow draft.
---

# Meadow

Meadow (https://findmeadow.com) is a social publishing workspace: people connect their channels, create a post once, and keep it as a draft, publish it, or schedule it to the destinations that support its format. This skill uses the `meadow` MCP server that ships with this plugin.

## Tools

| Tool | Use it to |
| --- | --- |
| `get_profile` | Confirm which Meadow workspace the API key belongs to |
| `list_projects` | Find the project the user means and its `id` |
| `list_accounts` | See the social accounts connected to a project |
| `list_posts` | Browse drafts, scheduled posts, and publishing history (newest first, `offset`/`limit` pagination, optional `status` filter) |
| `get_post` | Read one post and its per-account deliveries |
| `create_draft` | Save new post content as a draft |
| `get_analytics` | Read cached analytics totals for a project and each account |

Every tool except `get_profile` and `list_projects` needs a `projectId`. Call `list_projects` first; when the workspace has more than one project and the user hasn't said which one, ask instead of guessing.

## Saving a draft

1. Resolve the `projectId`. If the user named destinations, call `list_accounts` and pass the matching `accountIds`.
2. Call `create_draft` with the caption and, where it applies, a `title` and `format` (`auto`, `text`, `image`, `video`, `carousel`, `document`, `reel`, or `story`).
3. Generate one `requestId` (16–100 letters, digits, `_` or `-`) for the draft and reuse it if you retry. A response with `duplicate: true` means the draft already exists; don't create another.
4. Tell the user the draft was saved and that they can review, publish, or schedule it in Meadow at https://app.findmeadow.com.

A draft must contain a caption, a title, or media. `create_draft` saves content only. It does not publish, queue, or send anything to a social platform, so never say a post was published or scheduled because a draft was created.

## Reading status and analytics

- Post statuses are `draft`, `scheduled`, `publishing`, `published`, `awaiting_publish`, `cancelled`, `needs_attention`, and `partially_published`. Report per-account outcomes from the post's `deliveries`.
- `awaiting_publish` on a TikTok delivery means the video was sent to the user's TikTok inbox and they still need to finish posting it in TikTok. It is not a published post. Don't invent a public URL for it or suggest retrying the transfer.
- `get_analytics` returns cached figures and doesn't refresh them from the social platforms. Say so when the user asks for current numbers, and use `coverage` to explain when a metric is only available for some posts.

## What this plugin can't do

The Meadow MCP tools don't connect social accounts, upload media, publish, schedule, or delete posts. Send the user to https://app.findmeadow.com for those actions.

## Errors

- "A Meadow API key is required" or "This API key is invalid or has been revoked": ask the user to create a new key in Meadow under Configuration > API Keys and update the plugin's configuration.
- "Project not found": call `list_projects` again and use an exact `id` from the result.
- "Add text, a title, or media before saving this draft": ask the user for the post content.

---
name: meadow
description: Work with a Meadow social publishing workspace through the Meadow MCP server. Use when the user asks about their Meadow projects, connected social accounts, drafts, scheduled or published posts, delivery status, or analytics, or wants to upload media, save a draft, check a post against each platform, or publish or schedule a post with Meadow.
---

# Meadow

Meadow (https://findmeadow.com) is a social publishing workspace: people connect their channels, create a post once, and keep it as a draft, publish it, or schedule it to the destinations that support its format. This skill uses the `meadow` MCP server that ships with this plugin.

## Tools

| Tool | Use it to |
| --- | --- |
| `get_profile` | Confirm which Meadow workspace you're connected to |
| `list_projects` | Find the project the user means and its `id` |
| `list_accounts` | See the social accounts connected to a project |
| `list_posts` | Browse drafts, scheduled posts, and publishing history (newest first, `offset`/`limit` pagination, optional `status` filter) |
| `get_post` | Read one post and its per-account deliveries |
| `get_analytics` | Read cached analytics totals for a project and each account |
| `get_account_options` | Fetch an account's current publishing options from its platform, such as TikTok privacy choices or Pinterest boards |
| `upload_media` | Add an image, video, PDF, Word or PowerPoint file to a project and get its media ID |
| `create_upload_url` | Get a one-time link for uploading one large local file |
| `create_draft` | Save new post content as a draft |
| `preview_post` | Check a post against every selected account without publishing it |
| `publish_post` | Publish a post now or schedule it |
| `publish_draft` | Publish or schedule a saved draft |

Every tool except `get_profile` and `list_projects` needs a `projectId`. Call `list_projects` first; when the workspace has more than one project and the user hasn't said which one, ask instead of guessing.

## Saving a draft

1. Resolve the `projectId`. If the user named destinations, call `list_accounts` and pass the matching `accountIds`.
2. Call `create_draft` with the caption and, where it applies, a `title` and `format` (`auto`, `text`, `image`, `video`, `carousel`, `document`, `reel`, or `story`).
3. Generate one `requestId` (16–100 letters, digits, `_` or `-`) for the draft and reuse it if you retry. A response with `duplicate: true` means the draft already exists; don't create another.
4. Tell the user the draft was saved and that they can review it in Meadow at https://app.findmeadow.com.

A draft must contain a caption, a title, or media. `create_draft` saves content only: it doesn't publish, queue, or send anything to a social platform, so never say a post was published or scheduled because a draft was created.

## Publishing or scheduling

Publishing posts publicly on the user's real accounts. Only publish when the user asks for it.

1. Upload any media with `upload_media` (a public `url`, base64 `data` up to 10 MB, or an attached `file`) and keep the returned media IDs. For a large local file, use `create_upload_url`.
2. For TikTok and Pinterest accounts, call `get_account_options`.
3. Ask the user for every platform choice the post needs rather than choosing it yourself: TikTok privacy and TikTok's music usage consent, YouTube privacy and whether it's made for kids, and the Pinterest board.
4. Call `preview_post` with exactly the input you plan to publish, and fix every error it reports.
5. Show the user the exact content, the accounts, and the time (now, or the scheduled local time and time zone), and wait for them to confirm.
6. Call `publish_post` with a new `requestId`, or `publish_draft` for a saved draft. Reuse the same `requestId` only to retry the same request.

Without `schedule`, a post goes out now. A scheduled post takes `schedule: { mode: "scheduled", localDateTime: "2026-10-08T09:30" }` in the project's time zone unless `timeZone` is given. Use `overrides`, keyed by account ID, for a different caption, title, format, time, or platform settings per account.

`publish_post` queues one delivery per account. Report the post as queued, and call it published only when `get_post` shows a delivery as `published`.

## Reading status and analytics

- Post statuses are `draft`, `scheduled`, `publishing`, `published`, `awaiting_publish`, `cancelled`, `needs_attention`, and `partially_published`. Report per-account outcomes from the post's `deliveries`.
- `awaiting_publish` on a TikTok delivery means the video was sent to the user's TikTok inbox and they still need to finish posting it in TikTok. It isn't a published post: don't invent a public URL for it or retry the transfer.
- `get_analytics` returns cached figures and doesn't refresh them from the social platforms. Say so when the user asks for current numbers, and use `coverage` to explain when a metric is only available for some posts.

## What the tools can't do

The Meadow MCP tools don't connect or remove social accounts, edit or cancel queued posts, delete posts, or refresh analytics. Send the user to https://app.findmeadow.com for those actions. The tools only reach the connected user's own Meadow workspace.

## Errors

- "A Meadow API key is required" or "This API key is invalid or has been revoked": ask the user to create a new key in Meadow under Configuration > API Keys and update the plugin's configuration.
- "Sign in to Meadow" or "Authorize the required Meadow permissions": ask the user to reconnect Meadow and approve the requested permissions.
- "Project not found": call `list_projects` again and use an exact `id` from the result.
- "Add text, a title, or media before saving this draft": ask the user for the post content.
- A `preview_post` or `publish_post` validation error: explain each destination's error and ask the user how to fix it.

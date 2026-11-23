---
name: paxmod-integration
description: Add Paxmod content moderation to an app, or review an existing Paxmod integration for common mistakes. Use when the user wants to moderate chat, comments, usernames or images with Paxmod, or asks whether their Paxmod setup is right.
---

# Paxmod integration

Paxmod is a content moderation API. You send a message, it returns `flagged` or `not_flagged` with a score per category, or a verdict against the project's plain-English policy (Advanced mode).

If the Paxmod MCP server is connected, call `get_integration_guide` for the full API reference and `create_test_api_key` for a working key with no signup. Without it, the guide is at https://www.paxmod.com/llms-full.txt and a key comes from `POST https://www.paxmod.com/api/v1/agent-key`.

## Adding Paxmod

1. Find where user content is accepted: the chat send handler, comment create route, username form, upload handler. Moderate there, on the server, before anyone else sees it.
2. In Node or TypeScript use the `paxmod` package. Elsewhere call `POST /api/v1/text` (or `/api/v1/image`) with `Authorization: Bearer <key>`.
3. Send `user_id` (the author) and `context_id` (the room, channel or DM, the same id for everyone in it) with every message. Player history and conversation context depend on them.
4. Decide what a flag does with the user: block, hide, hold for review or mask. Make it a clear branch in the code.
5. Test with a few clean messages and a few that should be flagged, then give the user the `claim_url` so they keep the key.

## Review checklist

Check an existing integration against each of these and report what's wrong, with the file and line.

- **Key on the server.** The API key must never be in a game client, mobile app or browser bundle. Calls go through the app's backend.
- **Reads the right field.** Use `result === "flagged"` from the REST response, or `.flagged` from the SDK. Don't infer it from `reason` text or a single category.
- **Moderates before display.** Content is checked before it's shown to other users or stored as visible, not after.
- **Has a timeout and a chosen failure mode.** A 2 to 5 second timeout, and an explicit choice when Paxmod can't be reached: let it through (chat keeps working; SDK `onUnavailable: "allow"`) or hold it (safer for kids' platforms). Never an unhandled promise that leaves the message stuck.
- **Handles rate limits.** `429 RATE_LIMITED` means over 600 requests a minute for the project. Wait the `Retry-After` seconds, retry with jitter and a cap. `402` or a quota `429` means the plan limit, which retrying won't fix.
- **Sends user_id and context_id.** Missing ids turn off per-player history and conversation context.
- **AI apps check both sides.** For a chatbot or AI companion, moderate what the user sends and what the AI replies.
- **Images.** `422 CHECKS_BLOCKED` should be treated as flagged or sent for review. `503 CHECKS_UNAVAILABLE` should be retried shortly.
- **No double counting.** Each message is checked once, not on every render, edit preview or retry of an unrelated step.

## Tuning the policy

For Advanced mode, collect messages that should be flagged and ones that should pass, then test drafts with `test_policy` (or `test_policy_on_project` when signed in) until every example comes back as expected. Show the user the before and after. Save with `save_policy` only after they agree, or they can paste it into the Paxmod console.

# Paxmod MCP Server

Connect your AI agent to [Paxmod](https://www.paxmod.com), content moderation for any app where people post or chat: in-app chat, comments, forums, communities, marketplaces and games. It catches profanity, harassment, grooming risk, personal information and spam, plus anything else you describe in a plain-English policy.

This is a hosted (remote) [Model Context Protocol](https://modelcontextprotocol.io) server. There is nothing to install:

```
https://www.paxmod.com/mcp
```

## What your agent can do

- **Add moderation to your app.** "Add Paxmod moderation to our app's chat." The agent reads the API guide, creates a test key with no signup, builds the integration and checks it with real messages, then gives you a link to keep the key.
- **Tune your policy from examples.** "These 6 messages were flagged but are fine, and these 2 got through. Fix the policy." Advanced mode moderates against a policy you write in plain English. The agent rewrites it and tests each draft against your examples until every one comes back right. Tests store nothing and use no credits on your project (on a free test key, each message checked counts toward its 100).
- **Check messages.** "Would this get flagged?" with a score per category.
- **Work on your own projects.** "What got flagged in our chat this week?" or "Show me this player's history." Sign in to Paxmod once and the agent can read your projects, settings, usage and flagged messages, and test policies on them, with no API key in the chat.

## Tools

| Tool | What it does | Needs |
| - | - | - |
| `get_integration_guide` | The full API guide: endpoints, responses, errors, rate limits, code for Node, Python, Unity, Roblox and more | Nothing |
| `create_test_api_key` | A working key with no signup: 100 text messages over 72 hours, plus a claim link to keep it | Nothing |
| `moderate_text` | Checks one message with the project's settings | API key |
| `test_policy` | Runs up to 50 examples against a draft or saved policy and reports which come back as expected | API key |
| `test_word_rules` | Shows what word rules (allow, rewrite or block words) do to each message, with no model call | API key |
| `list_projects` | Your projects and the ones in your organizations, with their ids | Sign-in |
| `get_project_settings` | A project's mode, categories, thresholds, policy and word rules | Sign-in |
| `get_usage` | Messages per day and this month's total for a project | Sign-in |
| `test_policy_on_project` | The policy test on one of your projects, without pasting an API key | Sign-in |
| `list_flagged_messages` | Recent flagged messages, by date, category or player | Sign-in |
| `get_player_history` | One player's flagged messages and what they were flagged for | Sign-in |

`get_integration_guide`, `test_policy`, `test_word_rules` and every Sign-in tool are read-only.

## Setup

### Claude Code

```bash
claude mcp add --transport http paxmod https://www.paxmod.com/mcp
```

### Claude and Claude Desktop

Open **Customize → Connectors → Add custom connector** and enter `https://www.paxmod.com/mcp`.

### Cursor

`.cursor/mcp.json` in your project, or `~/.cursor/mcp.json` for every project:

```json
{
  "mcpServers": {
    "paxmod": { "url": "https://www.paxmod.com/mcp" }
  }
}
```

### VS Code

`.vscode/mcp.json`:

```json
{
  "servers": {
    "paxmod": { "type": "http", "url": "https://www.paxmod.com/mcp" }
  }
}
```

### Other clients

Add a remote MCP server with the URL `https://www.paxmod.com/mcp` and the Streamable HTTP transport.

## Sign-in and keys

The public tools need no sign-in. The Sign-in tools use Sign in with Paxmod (OAuth 2.1): the first time your agent calls one, your MCP client opens a Paxmod page where you sign in and choose Allow. Connected apps can't change settings or see API keys, and organizations that keep data in a separate region don't share player messages with them unless Paxmod turns it on.

Tools marked API key take one from your project's Settings in the [Paxmod console](https://www.paxmod.com/console), or a test key from `create_test_api_key`.

## Links

- [MCP server docs](https://www.paxmod.com/docs/mcp)
- [API reference](https://www.paxmod.com/docs)
- [SKILL.md](https://www.paxmod.com/SKILL.md) for agents without MCP
- [Pricing](https://www.paxmod.com/pricing)
- [Privacy policy](https://www.paxmod.com/privacy-policy)

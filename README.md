# Paxmod MCP Server

Connect your AI agent to [Paxmod](https://www.paxmod.com), content moderation for games and online communities: player chat, profanity, harassment, grooming risk, PII and spam.

This is a hosted (remote) [Model Context Protocol](https://modelcontextprotocol.io) server. There is nothing to install:

```
https://www.paxmod.com/mcp
```

## What your agent can do

- **Add moderation to your app.** "Add Paxmod moderation to our game chat." The agent reads the API guide, creates a test key with no signup, builds the integration and checks it with real messages, then gives you a link to keep the key.
- **Tune your policy from examples.** "These 6 messages were flagged but are fine, and these 2 got through. Fix the policy." Advanced mode moderates against a policy you write in plain English. The agent rewrites it and tests each draft against your examples until every one comes back right. Tests store nothing and use no credits on your project (on a free test key, each message checked counts toward its 100).
- **Check messages.** "Would this get flagged?" with a score per category.

## Tools

| Tool | What it does | Needs |
| - | - | - |
| `get_integration_guide` | The full API guide: endpoints, responses, errors, rate limits, code for Node, Python, Unity, Roblox and more | Nothing |
| `create_test_api_key` | A working key with no signup: 100 text messages over 72 hours, plus a claim link to keep it | Nothing |
| `moderate_text` | Checks one message with the project's settings | API key |
| `test_policy` | Runs up to 50 examples against a draft or saved policy and reports which come back as expected | API key |
| `test_word_rules` | Shows what word rules (allow, rewrite or block words) do to each message, with no model call | API key |

`get_integration_guide`, `test_policy` and `test_word_rules` are read-only.

## Setup

### Claude Code

```bash
claude mcp add --transport http paxmod https://www.paxmod.com/mcp
```

### Claude and Claude Desktop

Open **Settings → Connectors → Add custom connector** and enter `https://www.paxmod.com/mcp`.

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

## Keys

There is no sign-in. Tools that work on a project take its API key, from your project's Settings in the [Paxmod console](https://www.paxmod.com/console), or a test key from `create_test_api_key`.

## Links

- [MCP server docs](https://www.paxmod.com/docs/mcp)
- [API reference](https://www.paxmod.com/docs)
- [SKILL.md](https://www.paxmod.com/SKILL.md) for agents without MCP
- [Pricing](https://www.paxmod.com/pricing)
- [Privacy policy](https://www.paxmod.com/privacy-policy)

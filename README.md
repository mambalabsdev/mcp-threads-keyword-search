# Threads Keyword Search MCP Server

[![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-threads-keyword-search)](https://www.npmjs.com/package/@mambalabsdev/mcp-threads-keyword-search) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-threads-keyword-search)](https://github.com/mambalabsdev/mcp-threads-keyword-search/blob/main/LICENSE)

An MCP server for the Mamba Labs Threads Keyword Search actor on Apify. Search Threads for keywords and get one flat row per post that contains them.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Each keyword is searched the way Threads search works for a visitor who is not logged in, which shows about 20 to 25 posts per search. By default a post is kept only when the keyword's words are in its text, next to each other and in order, so loosely related results are set aside and not charged. A post found by several keywords is returned once with every keyword it matched. Filters for age, likes, and replies, and an only new posts memory for schedules. Each keyword also returns one free summary row. No Threads or Instagram account, login, or key is used.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-threads-keyword-search": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-threads-keyword-search"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `search_threads_posts` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "Find Threads posts about cold email from the last 30 days."
- "Which Threads posts mention our competitor by name this week?"
- "Collect Threads posts about hubspot alternatives with their like counts."

## Tool

`search_threads_posts`. One tool. Example arguments:

```json
{
  "keywords": [
    "cold email",
    "hubspot alternative"
  ],
  "match_mode": "phrase",
  "since_days": 30
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `keywords` | array | no | Words or phrases to search Threads for, up to 50. |
| `max_posts_per_keyword` | integer | no | Posts per keyword, up to 50 (Threads shows about 20 to 25). Default 25. |
| `match_mode` | string | no | phrase, all_words, or off. Default phrase. |
| `since_days` | integer | no | Set aside posts older than this many days. |
| `min_likes` | integer | no | Set aside posts with fewer likes. |
| `include_replies` | boolean | no | Return matching replies too. Default true. |
| `only_new` | boolean | no | Skip posts an earlier run returned. Default false. |
| `state_key` | string | no | Name for the only new posts memory. |
| `max_charge_usd` | string | no | Optional cost cap for the run. |
| `proxy_mode` | string | no | auto or residential. Default auto. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `post-row`, per Threads post that contains your keyword | $0.0025 | $0.002375 | $0.00225 | $0.002125 |
| `apify-actor-start`, per run start | $0.0005 | $0.0005 | $0.0005 | $0.0005 |

Summary rows, refused input, posts set aside by your filters, and already delivered posts are free. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/threads-keyword-search](https://apify.com/mambalabs/threads-keyword-search)

## License

MIT. See [LICENSE](LICENSE).

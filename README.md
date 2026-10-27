# Winify X API for Claude

Ask Claude for X/Twitter data, provably fair giveaway draws, and Kick or YouTube lookups. Claude calls the [Winify X API](https://winify.gg/twitter-api) for you and pays from **your own Winify API balance**.

> "Who are the 20 biggest accounts that retweeted this post?"
> "Roll 3 winners from the retweets of x.com/…/status/…, they must follow @winify and be over 30 days old."
> "How many followers did these five accounts gain? Compare them."

Every answer ends with what the call cost and the balance left. Every Winify account starts with **$0.50 free**, and you top up at [winify.gg/twitter-api](https://winify.gg/twitter-api). Calls that fail cost nothing.

## Claude Code

```
/plugin marketplace add MinimaCreative/winify-claude-plugin
/plugin install winify-x-api@winify
```

When the plugin is enabled, Claude Code asks for your **Winify API key**. It is optional and stored in your system keychain.

- **Paste the key** (`wnfy_live_…`, from [winify.gg/twitter-api](https://winify.gg/twitter-api)) and the tools are ready.
- **Or leave it empty** and sign in with Winify: run `/mcp`, pick `winify-x-api`, then authenticate in the browser and click **Allow**.

Without the plugin, add the server straight from a terminal:

```
claude mcp add --transport http winify-x-api https://winify.gg/api/mcp --header "X-API-Key: wnfy_live_…"
```

## Claude on the web, desktop and mobile

1. Open **Settings → Connectors → Add custom connector**.
2. Name it **Winify X API** and set the URL to `https://winify.gg/api/mcp`.
3. Click **Connect**, sign in to Winify and choose **Allow**.

Claude never sees your API key, only a sign-in token. Disconnect it any time on [winify.gg/twitter-api](https://winify.gg/twitter-api?tab=claude) → *Use with Claude* → *Connected apps*.

## Other MCP clients

The server is a standard [MCP](https://modelcontextprotocol.io) Streamable HTTP endpoint at `https://winify.gg/api/mcp`:

- **API key:** send `X-API-Key: wnfy_live_…` (or `Authorization: Bearer wnfy_live_…`).
- **OAuth 2.1:** discovery is at `https://winify.gg/.well-known/oauth-protected-resource/api/mcp`. It supports PKCE, dynamic client registration and client ID metadata documents.

## Tools

| Group | Tools |
|---|---|
| Account (free) | `get_balance`, `get_usage`, `get_pricing` |
| Profiles | `get_user`, `get_user_about`, `check_follow`, `search_users` |
| Follow graph | `get_followers`, `get_following`, `get_affiliates` |
| Posts | `get_post`, `get_posts`, `get_user_posts` |
| Engagement | `get_retweeters`, `get_replies`, `get_quotes` |
| Search | `search_posts`, `get_trends` |
| Lists & communities | `get_list`, `get_community` |
| Giveaways | `roll_giveaway`, `giveaway_entrants`, `get_giveaway_roll` |
| Kick & YouTube | `kick`, `youtube` |
| Anything else | `call_endpoint` |

Prices are on the [live rate card](https://winify.gg/twitter-api). The plugin's skill teaches Claude to start with one page, check the balance before big pulls, and roll giveaways in a single call.

## Notes

Winify is not affiliated with X Corp. The Winify X API is not an official X product. You are responsible for how you use the data it returns: see the [Terms of Service](https://winify.gg/terms).

## License

MIT

---
name: winify-x-api
description: Use when the user wants X/Twitter data (a profile, followers, following, retweeters, replies, quotes, post stats, search, trends, lists, communities), wants giveaway winners drawn from an X post, or wants Kick or YouTube channel data, through the winify-x-api tools. Those tools spend the user's own prepaid Winify balance, so this covers choosing the cheap call.
---

# Winify X API

The `winify-x-api` tools call the Winify X API as the user and are **paid from their prepaid Winify balance**. Every result ends with a line like `Cost: 1,200 credits ($0.0120) · Balance: 48,800 credits ($0.4880)`. When a call cost more than a few cents, say what it cost.

## Spend carefully

- **Free:** `get_balance`, `get_usage`, `get_pricing`, `get_giveaway_roll`. Failed calls are free too.
- Check `get_balance` before anything that pulls many items.
- Start with one page, which is the default. Use `mode: "all"` only when the user needs the whole list. "All" walks 10 pages unless you set `max_pages`.
- `mode: "all"`, `"deep"` and `"shadow"` need $0.20 of balance held before they start, and they bill per item returned.
- Ask before a pull that could cost more than about $1. Followers of a big account, every reply to a viral post, or a long deep search can all get there.
- Results are compact rows, and very long lists are trimmed with a "Showing N of M" note. Everything that came back was billed, so narrow the request instead of repeating it.

## Rates (20× twitterapi.io; `get_pricing` has the live table)

| What | Cost |
|---|---|
| Post returned (post, search, replies, quotes, timelines) | $0.003 each |
| Single profile (`get_user`, `get_user_about`) | $0.0036 |
| Profile in a list (followers, following, retweeters, user search) | $0.0002–$0.0006 each, cheapest on full pages |
| Follow check, trends, community info | $0.003 per call |
| List timeline / members | +$0.03 per call |
| `roll_giveaway` | $0.03 + $0.0002 per entrant + $0.003 per follow check |
| `giveaway_entrants` | $0.015 + $0.0002 per entrant + $0.003 per follow check (every entrant) |
| Kick / YouTube | per call; batch actions per item (see `get_pricing`) |

1 USD = 100,000 credits.

## Giveaways

- To pick winners, call `roll_giveaway` **once** with the post and the rules. Rules include `must_follow`, `min_followers`, `min_account_age_days`, `exclude_handles` and the like. It collects entrants and checks the rules on drawn candidates only, which is far cheaper than pulling every retweeter yourself and filtering.
- `sources` defaults to retweets. Add `replies` / `quotes` for those giveaways; `entry_contains` checks reply text.
- To re-roll, pass the earlier winners in `requirements.exclude_previous_winners`.
- Give the user the `roll_id` and the seeds. Anyone can verify the draw with `get_giveaway_roll`.
- Use `giveaway_entrants` only when the user wants to see everyone eligible before drawing.

## Picking the tool

- A post by URL: `get_post`. Up to 50 posts: `get_posts`.
- Who retweeted: `get_retweeters`. Use mode `shadow` when the user needs entrants X hides as "unavailable".
- Replies: `get_replies`. Use `deep` / `shadow` for posts with folded or hidden replies.
- Search: `search_posts` uses X search syntax (`from:`, `min_faves:`, `filter:media`…). Use mode `deep` with `since`/`until` for an exhaustive window.
- Anything not covered: `call_endpoint` with a path from https://winify.gg/twitter-api?tab=docs.

## Errors

- **Not enough balance (402):** tell the user to top up at https://winify.gg/twitter-api.
- **Key refused (401):** the key was rotated or revoked. The user can paste the new key with `/plugin` → configure winify-x-api, or switch to signing in through `/mcp`.

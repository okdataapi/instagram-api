# Instagram API: profiles, posts and Reels, comments, hashtags and Reel transcripts

**Instagram data API · Instagram profile and follower count · Instagram posts and Reels · Instagram comments · Instagram hashtag posts · Reel transcripts.**

Examples for the [okdata](https://okdata.online/?src=gh-instagram-api) Instagram API: 8 live endpoints behind one API key, pay per call from $0.002. No Instagram login, no cookies and no browser, so there is no account to get blocked.

[Sign up free ($0.10 of trial credit)](https://okdata.online/signup/?src=gh-instagram-api) · [All Instagram endpoints](https://okdata.online/apis/instagram/?src=gh-instagram-api) · [Use it from Claude, Cursor or any MCP client](https://okdata.online/mcp/?src=gh-instagram-api)

## Endpoints

| Endpoint | What it returns | Price | Path |
|---|---|---|---|
| [Instagram account profile](https://okdata.online/apis/instagram/users/get/) | An Instagram account by handle: name, bio, followers, following, post count, verification, whether it is private, and the website and links the profile gives. | $0.004 a call | `GET /v1/instagram/users/get` |
| [Instagram account posts](https://okdata.online/apis/instagram/users/posts/list/) | An account's posts, newest first, 12 a page, or its reels only: caption, likes, comments, plays, time posted, images and the other authors of a collab post. | $0.004 a call | `GET /v1/instagram/users/posts/list` |
| [Instagram post detail](https://okdata.online/apis/instagram/posts/get/) | One post or reel by shortcode or link: caption, author, likes, comments and plays, images, and the other authors of a collab post. | $0.004 a call | `GET /v1/instagram/posts/get` |
| [Instagram post comments](https://okdata.online/apis/instagram/posts/comments/list/) | Comments on a post or reel, newest first, about 15 a page: text, commenter, likes and time posted. | $0.004 a call | `GET /v1/instagram/posts/comments/list` |
| [Instagram comment replies](https://okdata.online/apis/instagram/comments/replies/list/) | The replies to one comment, by the comment's ID and its post's shortcode: text, replier, likes and time posted. | $0.004 a call | `GET /v1/instagram/comments/replies/list` |
| [Instagram hashtag posts](https://okdata.online/apis/instagram/hashtags/posts/list/) | Public posts under a hashtag, about 10 a page, as search engines find them: caption, author, likes, comments and time posted. Not every post under the tag, and not in time order. | $0.004 a call | `GET /v1/instagram/hashtags/posts/list` |
| [Instagram reel transcript](https://okdata.online/apis/instagram/posts/transcript/get/) | What is said in a reel or video post, by shortcode or link, with timestamps. Videos up to two minutes long. | $0.004 a call | `GET /v1/instagram/posts/transcript/get` |
| [Instagram account search](https://okdata.online/apis/instagram/users/search/) | Find Instagram accounts by keyword, about 20 in one answer and no further pages: handle, name, avatar, verification and whether it is private. | $0.002 a call | `GET /v1/instagram/users/search` |

## Quick start

1. Sign up at the [okdata console](https://okdata.online/signup/?src=gh-instagram-api) with your email and copy the key that starts with `okd_`.
2. Make a call:

```bash
export OKDATA_API_KEY=okd_...
curl -G "https://api.okdata.online/v1/instagram/users/search" \
  -H "Authorization: Bearer $OKDATA_API_KEY" \
  --data-urlencode 'query=nike'
```

The Python version is [examples/quickstart.py](examples/quickstart.py) (standard library only, and it fetches a second page too): `python3 examples/quickstart.py`.

Every endpoint answers in the same shape: the list is in `data.items`, `page.next_cursor` goes back as `cursor` for the next page, what the call cost is in `meta.billing.amount`, and a field the platform does not provide is null with the reason in `meta.unavailable`.

## Use it from AI (MCP)

Claude Code:

```bash
claude mcp add --transport http --scope user okdata https://api.okdata.online/mcp --header "Authorization: Bearer $OKDATA_API_KEY"
```

Cursor, Windsurf, VS Code and other clients with remote MCP:

```json
{
  "mcpServers": {
    "okdata": {
      "url": "https://api.okdata.online/mcp",
      "headers": {
        "Authorization": "Bearer okd_..."
      }
    }
  }
}
```

Then ask in plain words, for example "Instagram account search: nike". The agent uses the free `find` and `describe` tools to pick the endpoint and see its price, then `call` to fetch the data.

## FAQ

**Do I need a Instagram account?** No. The data is public content, fetched on the server; your own account is never involved.

**How is it billed?** Each endpoint has its own price, listed above for one call; endpoints billed per item count what comes back. A call that reached the data source is charged even when it comes back empty, not found or failed (one item for per-item endpoints); calls stopped before reaching a source (bad parameters, balance) are free. New accounts get $0.10 to try; after that you top up as you go, with no subscription. Pricing: https://okdata.online/pricing/

**Why not the Instagram Graph API?** It mostly reads the business and creator accounts that connect to your app. This returns public profiles, posts, Reels and comments of any public account, with no app review.

**Can I page through results?** Each page of a list is one call: pass `page.next_cursor` back as `cursor` for the next one. A cursor stays valid for 24 hours.

**What happens when a call fails?** Every error has a fixed `code`, a `message`, and says whether you were charged (`charged`) and whether to retry (`retry`). All error codes: https://okdata.online/docs/errors/

## 中文说明

**Instagram 数据 API · Instagram 账号信息 · Instagram 帖子和 Reels · Instagram 评论采集 · Instagram 话题 · Reels 字幕。**

okdata 的Instagram接口示例：8 个接口在线可用，一个 API Key 调用，按次计费（$0.002 起），不用登录平台账号、不用 Cookie。中文文档：https://okdata.online/zh/apis/instagram/?src=gh-instagram-api · 注册：https://okdata.online/zh/signup/?src=gh-instagram-api · 接入 AI（MCP）：https://okdata.online/zh/mcp/?src=gh-instagram-api

## License

MIT. The data comes from the okdata API; its terms apply to API use.

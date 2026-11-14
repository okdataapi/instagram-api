# Instagram 数据接口（Instagram API）：账号、帖子和 Reels、评论、话题、字幕

**Instagram 数据 API · Instagram 账号信息 · Instagram 帖子和 Reels · Instagram 评论采集 · Instagram 话题 · Reels 字幕。**

这个仓库是 [okdata](https://okdata.online/zh/?src=gh-instagram-api) 的Instagram接口示例：8 个接口在线可用，一个 API Key 调用，按次计费（$0.002 起），失败不扣费。不用登录平台账号，不用 Cookie，不开浏览器，所以也不存在账号被风控的问题。

[免费注册拿 Key（送 $0.1 试用额度）](https://okdata.online/zh/signup/?src=gh-instagram-api) · [全部接口文档](https://okdata.online/zh/apis/?src=gh-instagram-api) · [接入 AI（MCP）](https://okdata.online/zh/mcp/?src=gh-instagram-api)

## 接口清单

| 接口 | 说明 | 价格 | 路径 |
|---|---|---|---|
| [Instagram 账号信息](https://okdata.online/zh/apis/instagram/users/get/) | 按用户名取 Instagram 账号：名称、简介、粉丝数、关注数、帖子数、认证、是否私密，以及主页上的网站和链接。 | $0.004 / 次 | `GET /v1/instagram/users/get` |
| [Instagram 账号帖子](https://okdata.online/zh/apis/instagram/users/posts/list/) | 账号发布的帖子，从新到旧，每页 12 条，也可以只要 Reels：文案、点赞、评论、播放、发布时间、图片，以及合拍帖的其他作者。 | $0.004 / 次 | `GET /v1/instagram/users/posts/list` |
| [Instagram 帖子详情](https://okdata.online/zh/apis/instagram/posts/get/) | 按短码或链接取一条帖子或 Reel：文案、作者、点赞、评论和播放、图片，以及合拍帖的其他作者。 | $0.004 / 次 | `GET /v1/instagram/posts/get` |
| [Instagram 帖子评论](https://okdata.online/zh/apis/instagram/posts/comments/list/) | 帖子或 Reel 下的评论，从新到旧，每页约 15 条：内容、评论者、点赞和发布时间。 | $0.004 / 次 | `GET /v1/instagram/posts/comments/list` |
| [Instagram 评论回复](https://okdata.online/zh/apis/instagram/comments/replies/list/) | 按评论 ID 和所在帖子的短码取一条评论下的回复：内容、回复者、点赞和发布时间。 | $0.004 / 次 | `GET /v1/instagram/comments/replies/list` |
| [Instagram 话题帖子](https://okdata.online/zh/apis/instagram/hashtags/posts/list/) | 某个话题下的公开帖子，每页约 10 条，按搜索引擎收录的顺序：文案、作者、点赞、评论和发布时间。不是话题下的全部帖子，也不按时间排。 | $0.004 / 次 | `GET /v1/instagram/hashtags/posts/list` |
| [Instagram 视频字幕](https://okdata.online/zh/apis/instagram/posts/transcript/get/) | Reel 或视频帖子里说的话，带时间戳，按短码或链接取。视频最长两分钟。 | $0.004 / 次 | `GET /v1/instagram/posts/transcript/get` |
| [Instagram 账号搜索](https://okdata.online/zh/apis/instagram/users/search/) | 按关键词找 Instagram 账号，一次约 20 个，没有下一页：用户名、名称、头像、认证和是否私密。 | $0.002 / 次 | `GET /v1/instagram/users/search` |

## 快速开始

1. 在 [okdata 控制台](https://okdata.online/zh/signup/?src=gh-instagram-api) 用邮箱注册，拿到 `okd_` 开头的 Key。
2. 调一次接口：

```bash
export OKDATA_API_KEY=okd_...
curl -G "https://api.okdata.online/v1/instagram/users/search" \
  -H "Authorization: Bearer $OKDATA_API_KEY" \
  --data-urlencode 'query=nike'
```

Python 版在 [examples/quickstart.py](examples/quickstart.py)（只用标准库，会顺带演示翻页）：`python3 examples/quickstart.py`。

返回统一的结构：列表在 `data.items`，下一页用 `page.next_cursor` 传回 `cursor`，这次扣了多少钱在 `meta.billing.amount`，平台不提供的字段为 null 并在 `meta.unavailable` 里写明原因。

## 让 AI 直接用（MCP）

Claude Code：

```bash
claude mcp add --transport http --scope user okdata https://api.okdata.online/mcp --header "Authorization: Bearer $OKDATA_API_KEY"
```

Cursor、Cherry Studio、WorkBuddy 等支持远程 MCP 的客户端：

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

接好以后直接用中文说需求，比如「Instagram 账号搜索：nike」。AI 会先用免费的 `find`、`describe` 找接口和看价格，再用 `call` 取数据。

## 常见问题

**要登录平台账号吗？** 不用。数据是公开内容，okdata 在服务端获取，你的账号完全不参与。

**怎么收费？** 每个接口单独定价，表里写的就是一次调用的价格；按条计费的接口按实际返回条数算。调用失败（包括超时）不扣费。新账号送 $0.1 试用额度，之后按需充值，没有订阅。价格页：https://okdata.online/zh/pricing/

**和官方开放平台有什么区别？** 官方开放平台只能读授权给你的账号自己的数据，而且要企业资质和审核；这里是平台上任何人都能看到的公开数据，比如搜索结果、别人的笔记或视频、评论、热榜。

**能翻页吗？** 列表接口每页一次调用，把 `page.next_cursor` 原样传回 `cursor` 就是下一页，24 小时内有效。

**出错了怎么办？** 每个错误都有固定的 `code`、中文说明 `message_zh`，以及会不会扣费（`charged`）和要不要重试（`retry`）。全部错误码：https://okdata.online/zh/docs/errors/

## English

Examples for the okdata Instagram API: 8 live endpoints behind one API key, pay per call from $0.002, failed calls free, no platform login or cookies. Sign up at https://okdata.online/signup/?src=gh-instagram-api, docs at https://okdata.online/apis/, MCP at https://okdata.online/mcp/.

## License

MIT. The data comes from the okdata API; its terms apply to API use.

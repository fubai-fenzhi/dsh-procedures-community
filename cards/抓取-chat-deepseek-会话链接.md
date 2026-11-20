---
id: fetch-chat-deepseek-session
title: chat.deepseek.com 的私有会话链接需要登录，抓取只得到 SPA 外壳
type: trap
triggers:
  - 会话链接打不开
  - 只拿到 DeepSeek
  - 分享链接只有壳
anchors:
  - chat.deepseek.com
  - web_fetch
  - /a/chat/s/
valid_until: 2027-06-30
environment: 任意
created: 2026-11-06
last_hit: null
hits: 0
---

# 抓取-chat.deepseek.com 会话链接

## 触发
要读 `https://chat.deepseek.com/a/chat/s/<id>` 或类似私有会话链接，抓回来只有标题。

## 首选
请用户把对话正文贴过来，或另存为 HTML / txt / md 后给路径；不要继续猜端点。

## 被挡时
- 用户能提供公开 `/share/<id>` 链接时，再抓公开页。
- 用户已登录浏览器时，请用户复制正文最快。

## 别再来
| 弯路 | 为什么不通 |
| --- | --- |
| 反复 web_fetch 原链接 | 只返回 SPA 外壳，正文只有 “DeepSeek” |
| 把 `/a/chat/s/<id>` 改成 `/share/<id>` | 返回 `share/invalid`，仍是空壳 |
| 用 r.jina.ai 代理 | 登录墙挡在前面，fetch failed |
| 猜 `/api/...` 端点 | 需要登录态，端点未公开 |

## 注意
- HTTP 200 不等于拿到内容；私有会话必须登录。
- 判据：正文只有标题、没有消息节点。
- 正确动作是向用户要内容，不是继续重试。

证据：2026-11-06 本会话实测。

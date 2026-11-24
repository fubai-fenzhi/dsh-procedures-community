---
id: tool-github-git-connection-reset
title: git clone/push github.com:443 连接被重置，改用 GitHub Data API
type: trap
rarity: common
triggers:
  - git clone 连接被重置
  - git push 失败
  - Could not connect to server
anchors:
  - github.com:443
  - Schannel
  - SEC_E_NO_CREDENTIALS
  - Could not connect to server
requires: []
excludes: []
valid_until: null
environment: Windows / 网络受限
created: 2026-11-21
last_hit: null
hits: 0
---

# 工具-GitHub-git-网络重置-走API

## 触发
`git clone` / `git push` 连 `github.com:443` 报连接被重置（Schannel / SEC_E_NO_CREDENTIALS / Could not connect to server），重试也不行。

## 首选
不依赖 git 网络：用 **GitHub Data API** 同步仓库（Node fetch + `gh auth token`）：
blobs → tree → commit → ref，一次 PUT 即可更新仓库。

## 被挡时
1. 先试 HTTPS 代理 / ghproxy 镜像。
2. 仍失败就纯 API：`GET /repos/{owner}/{repo}/git/refs/heads/main` → 建 blob/tree/commit → PATCH ref。

## 别再来
| 弯路 | 为什么不通 |
| --- | --- |
| 反复重试 git clone | 网络层被重置，重试同样失败 |
| 换 `git://` 协议 | 现在 GitHub 基本不支持，且更不安全 |

## 注意
- 判据：API 返回新 commit SHA，raw.githubusercontent 能读到新内容。

证据：_inbox.md 2026-11-21 14:00

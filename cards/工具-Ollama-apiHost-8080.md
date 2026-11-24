---
id: tool-ollama-apihost-8080
title: Ollama 提供方 apiHost 误填 8080，模型列表为空
type: trap
rarity: common
triggers:
  - Ollama 模型列表为空
  - Ollama 提供方选不了模型
  - apiHost 8080
anchors:
  - apiHost
  - 11434
  - ollama
requires: []
excludes: []
valid_until: null
environment: DSH Desktop / Windows
created: 2026-11-19
last_hit: null
hits: 0
---

# 工具-Ollama-apiHost-8080

## 触发
配置 Ollama 提供方后模型列表为空/选不了；检查 `apiHost` 填的是 8080（那是 llama.cpp 的端口）。

## 首选
`apiHost: http://127.0.0.1:11434`（Ollama 默认端口）。

## 被挡时
1. `curl http://127.0.0.1:11434/api/tags` 确认 Ollama 在跑。
2. 8080 是本地模型/llama.cpp 端口，别和 Ollama 混用。

## 别再来
| 弯路 | 为什么不通 |
| --- | --- |
| 把 8080 当 Ollama | 8080 是 llama.cpp 的端口，Ollama 在 11434 |

## 注意
- 判据：`/api/tags` 返回模型列表，UI 里能选到模型。

证据：_inbox.md 2026-11-19 10:00

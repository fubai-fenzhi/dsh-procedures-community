---
id: engine-llamacpp-context-thinking
title: llama.cpp 上下文太小 + 思考进 reasoning_content 导致"不回答"
type: trap
rarity: common
triggers:
  - exceed_context_size_error
  - 只思考不回答
  - Chatbox 没有回答
anchors:
  - exceed_context_size_error
  - reasoning_content
  - --reasoning-format
  - enable_thinking
valid_until: 2027-06-30
environment: llama.cpp server / Chatbox / Qwen3.5
created: 2026-11-19
last_hit: null
hits: 0
---

# 引擎-llama.cpp 上下文与思考

## 触发
Chatbox 连本地 llama.cpp：报 `request (4319 tokens) exceeds the available context size (2048 tokens)`，或回复空白（只思考不回答）。

## 首选
- 上下文：`-c 8192`（Chatbox 请求 4k+ tokens）。
- 想看见思考：`--reasoning-format none`（思考并入 content 显示）。
- 想直接回答：env `LLAMA_ARG_CHAT_TEMPLATE_KWARGS={"enable_thinking":false}`（PowerShell 传 JSON 会被吞引号，用环境变量）。
- 别名：`--alias <name>` 让 /v1/models 显示干净的名字。

## 被挡时
- 模型太大装不进 5GB 显存 → 掉到 4-5 tok/s；换裁剪版或 4B。
- 真跑不动：Ollama 3b/8b 兜底（deepseek-r1 中文思考完整）。

## 别再来
| 弯路 | 为什么不通 |
| --- | --- |
| `-c 2048` 直接给 Chatbox | 超上下文 400 |
| Start-Process 传 `--chat-template-kwargs '{"enable_thinking": false}'` | PS 拼命令行吃引号，LLAMA 解析失败 |
| 用 9B 全量 Q4 | 5.63GB > 5GB VRAM |

## 注意
- 裁剪版思考会英文/死循环，属模型退化。
- 判据：/v1/chat/completions timings.predicted_per_second 正常且 content 非空。
证据：2026-11-18/19 本地 9B 部署。

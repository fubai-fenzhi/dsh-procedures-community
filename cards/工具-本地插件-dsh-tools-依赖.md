---
id: tool-local-plugin-dsh-tools-import
title: 本地目录安装的插件 import '@deepseek-ai/dsh-tools' 报 ERR_MODULE_NOT_FOUND
type: trap
rarity: common
triggers:
  - 插件加载失败 ERR_MODULE_NOT_FOUND
  - 本地插件起不来
  - install_bundle 后插件不生效
anchors:
  - ERR_MODULE_NOT_FOUND
  - "@deepseek-ai/dsh-tools"
  - install_bundle
requires: []
excludes: []
valid_until: null
environment: DSH Desktop / 本地插件
created: 2026-11-22
last_hit: null
hits: 0
---

# 工具-本地插件-dsh-tools-依赖

## 触发
从本地目录 `install_bundle` 的插件启动报 `Cannot find package '@deepseek-ai/dsh-tools'`（或类似 `ERR_MODULE_NOT_FOUND`）。

## 首选
插件**零依赖**：工具定义用纯 JSON Schema 直接 `ctx.tools.register({...})`，不要 `import '@deepseek-ai/dsh-tools'`。

## 被挡时
1. 必须用 `defineTool` 时，把 `@deepseek-ai/dsh-tools` 装进**插件自己的** `node_modules`（解析锚点是插件目录，不是 DSH 安装目录）。
2. 改完禁用/重新启用插件（`set_plugin`）让它重新加载。

## 别再来
| 弯路 | 为什么不通 |
| --- | --- |
| 以为 DSH 安装里的包会被插件目录解析到 | Node 从插件目录向上找 `node_modules`，找不到 DSH 的那个 |

## 注意
- 判据：`plugin_manager` 显示 `application: applied`，且工具出现在会话里。

证据：_inbox.md 2026-11-22 09:00

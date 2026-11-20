---
id: tool-app-asar-files
title: DSH Desktop 的包与 skill 文件在 app.asar 内，shell / ripgrep 读不到
type: trap
triggers:
  - skill 文件读不到
  - app.asar 路径不存在
  - ripgrep 读不到
anchors:
  - app.asar
  - dsh-agent-preset
  - ENOENT
valid_until: 2027-12-31
environment: DSH Desktop / Windows
created: 2026-11-06
last_hit: null
hits: 0
---

# 工具-DSH Desktop 的 app.asar 内文件

## 触发
要读 DSH 自带的 skill / 包内文件，shell、glob、grep、node、pnpm 都报路径不存在。

## 首选
去加载 `read-asar-file` skill（`skill read-asar-file`）——完整步骤、验证、注意事项都在里面。
一句话兜底：用 read 工具按精确路径读 `E:\DeepSeek\resources\app.asar\dsh\node_modules\@deepseek-ai\<包>\...`。
已知 skill 根：`...\dsh-agent-preset\skills\<skill>\`。

## 被挡时
- 先读同包 `package.json` 确认路径，再读目标文件。
- 目录无法列时，用 `cordis_inspect_query` 的 `packageDir`，或让 Host 侧读取。

## 别再来
| 弯路 | 为什么不通 |
| --- | --- |
| `Get-ChildItem app.asar\...` | 报“路径不存在”，不是权限问题 |
| `rg` / `grep` / `glob` 扫 app.asar | 原生进程读不了虚拟归档，返回 ENOENT |
| 去修 ACL / 提权 | 与权限无关，改了也没用 |

## 注意
- app.asar 是 Electron 虚拟归档，只有 Host 进程的文件读取能打开。
- shell、node、pnpm 都读不到；别把它当普通目录。
- 报错形如 `IO error ... os error 3` 或 `cannot read ... not found`。

证据：2026-11-06 本会话读取 `dsh-agent-preset/skills` 时实测。

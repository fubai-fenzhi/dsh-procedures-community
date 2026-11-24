---
id: tool-batch-chinese-path-ascii
title: .cmd/.bat 里写中文路径 + ASCII 编码会变成 E:\,0AI
type: trap
rarity: common
triggers:
  - 批处理中文路径乱码
  - cmd 路径变成 0AI
  - .bat 调用目标路径不对
anchors:
  - .cmd
  - .bat
  - 0AI
  - E:\AILocal
requires: []
excludes: []
valid_until: null
environment: Windows / 批处理
created: 2026-11-21
last_hit: null
hits: 0
---

# 工具-批处理-中文路径-ascii

## 触发
`.cmd` / `.bat` 里直接写中文路径，按 ASCII 保存后路径变成 `E:\,0AI` 之类乱码，调用目标失败。

## 首选
批处理里只用 **ASCII 路径**（例如 `E:\AILocal\...`）调用目标；中文路径留在 PowerShell 脚本内部使用。

## 被挡时
1. 确认 bat 文件编码是 ANSI/GBK 还是 UTF-8，统一后再试。
2. 用 `%~dp0` 相对路径绕开中文绝对路径。

## 别再来
| 弯路 | 为什么不通 |
| --- | --- |
| 在 bat 里直接写 `E:\本地AI\...` | ASCII 编码保存后中文被替换成乱码字节 |

## 注意
- 判据：bat 执行时打印的路径正确，目标程序能启动。

证据：_inbox.md 2026-11-21 09:05

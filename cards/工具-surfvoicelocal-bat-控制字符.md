---
id: tool-surfvoicelocal-bat-control-chars
title: surfvoicelocal 的 run.bat/gsv.bat 含控制字符，启动按钮起不来
type: trap
rarity: common
triggers:
  - surfvoicelocal 启动按钮没反应
  - run.bat 控制字符
  - gsv.bat 乱码
anchors:
  - run.bat
  - gsv.bat
  - 0x0B
  - 0x09
  - surfvoicelocal
requires: []
excludes: []
valid_until: null
environment: Windows / surfvoicelocal
created: 2026-11-19
last_hit: null
hits: 0
---

# 工具-surfvoicelocal-bat-控制字符

## 触发
SurfCore 设置页「启动」按钮点了没反应；包内 `run.bat` / `gsv.bat` 含 `0x0B`（\v）/`0x09`（\t）控制字符（模板转义留下的）。

## 首选
用 Node fs 重写这两个 bat：纯 ASCII 文本 + CRLF，去掉 `\v`/`\t` 等控制字符，再点启动。

## 被挡时
1. 用十六进制看文件，确认没有 `0x0B`/`0x09`。
2. 重写后单独双击 bat 验证能起服务。

## 别再来
| 弯路 | 为什么不通 |
| --- | --- |
| 直接编辑保存 | 编辑器可能又把制表符转成 `\t` 控制字符 |
| 不检查就点启动 | 控制字符会让 cmd 解析失败，按钮无反应 |

## 注意
- 判据：bat 里没有任何控制字符，启动按钮能拉起服务。

证据：_inbox.md 2026-11-19 10:15

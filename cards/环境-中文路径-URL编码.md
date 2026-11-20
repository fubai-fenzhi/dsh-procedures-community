---
id: env-chinese-path-url-encoded
title: 脚本路径含中文会被 URL 编码（E:\%E6%9C%AC...）
type: trap
rarity: common
triggers:
  - 中文路径报错
  - venv 创建失败
  - %E6 路径
anchors:
  - '%E6%9C%AC'
  - fileURLToPath
  - import.meta.url
  - mklink
valid_until: 2027-06-30
environment: Windows / Node ESM / 中文目录
created: 2026-11-19
last_hit: null
hits: 0
---

# 环境-中文路径 URL 编码

## 触发
Node 脚本在 `E:\本地AI\...` 下运行，报路径 `E:\%E6%9C%AC%E5%9C%B0AI\...` 不存在（venv/目录创建失败）。

## 首选
- 改脚本：`path.dirname(fileURLToPath(import.meta.url))` 替代 `new URL(import.meta.url).pathname`（手工解码只处理了盘符）。
- 或给依赖包建 ASCII 目录联接：`mklink /J E:\AILocal E:\本地AI`，从 `E:\AILocal\...` 运行。

## 被挡时
- 联接失效/变成真实目录：`dir /AL E:\` 看是否还标记为 <JUNCTION>；重建前先把旧目录改名（ENOTEMPTY 删不掉）。
- 修改前备份原脚本。

## 别再来
| 弯路 | 为什么不通 |
| --- | --- |
| 只改盘符前缀的正则 | 中文部分仍 %编码 |
| 直接把 E:\AILocal 当联接用 | 它可能是残留真实目录（曾 15.88GB），rmdir ENOTEMPTY |
| 用 python 打印路径再拼 | 控制台编码把中文弄坏 |

## 注意
- 判据：`fs.existsSync('E:/AILocal/voice-local') === true` 且脚本日志路径无 %。
证据：2026-11-19 surfvoicelocal bootstrap。

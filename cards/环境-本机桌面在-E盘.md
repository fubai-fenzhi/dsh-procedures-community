---
id: env-desktop-e-drive
title: 本机桌面被重定向到 E:\Desktop，不在用户目录
type: trap
triggers:
  - Desktop 不存在
  - 找不到桌面
  - 用户目录下没有 Desktop
anchors:
  - C:\Users\Administrator\Desktop
  - E:\Desktop
  - GetFolderPath
excludes:
  - out of range
  - Access is denied
  - 拒绝访问
valid_until: null
environment: 这台 Windows 机器
created: 2026-11-06
last_hit: null
hits: 0
---

# 环境-本机桌面在 E 盘

## 触发
按 `$env:USERPROFILE\Desktop` 找桌面，路径不存在。

## 首选
先看 `E:\Desktop`；这是本机真实桌面。

## 被挡时
- 用 `[Environment]::GetFolderPath('Desktop')` 读系统认定的桌面。
- 公共桌面：`C:\Users\Public\Desktop`。

## 别再来
| 弯路 | 为什么不通 |
| --- | --- |
| 只查 `C:\Users\Administrator\Desktop` | 本机不存在，直接报路径错误 |
| 在用户目录下新建 Desktop | 会造出第二个桌面，用户看不到 |

## 注意
- 这台机器的桌面被重定向到 `E:\Desktop`（PROJECT.md 也这么写）。
- 判据：`Test-Path` 为 False，但 `E:\Desktop` 有文件。

证据：2026-11-06 本会话实测。

---
id: tool-imageio-ffmpeg-path
title: python -c 打印 imageio_ffmpeg 路径时中文被编码弄坏
type: trap
rarity: common
triggers:
  - imageio_ffmpeg 路径
  - ffmpeg 找不到
  - python -c 输出中文乱码
anchors:
  - imageio_ffmpeg
  - ffmpeg
  - site-packages
requires: []
excludes: []
valid_until: null
environment: Windows / Python
created: 2026-11-19
last_hit: null
hits: 0
---

# 工具-imageio-ffmpeg-中文路径

## 触发
用 `python -c` 打印 `imageio_ffmpeg` 的 ffmpeg 路径时，输出的中文路径被编码弄坏（乱码/问号），拿不到真实路径。

## 首选
直接用 fs 拼路径，不经过控制台编码：
`<site-packages>/imageio_ffmpeg/binaries/ffmpeg-*`（用 Node/Python 的路径 API 拼，再 `fs.existsSync` 校验）。

## 被挡时
1. `python -c "import imageio_ffmpeg; print(imageio_ffmpeg.get_ffmpeg_exe())"` 只取返回值，不打印中文。
2. 仍乱码就 `chcp 65001` 后再试。

## 别再来
| 弯路 | 为什么不通 |
| --- | --- |
| 依赖 `python -c` 打印中文路径 | Windows 控制台编码会把中文路径转坏 |

## 注意
- 判据：`existsSync` 返回 true，且 ffmpeg 能执行 `-version`。

证据：_inbox.md 2026-11-19 10:05

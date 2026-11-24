---
id: engine-surfcore-voice-503-character
title: SurfCore 桌宠语音 503：角色没有配 [voice.characters] 映射
type: trap
rarity: common
triggers:
  - 桌宠语音 503
  - 角色没有配 voice.characters
  - SurfCore 语音起不来
anchors:
  - 503
  - voice.characters
  - socialV2.voice.tts.character
  - voxd
requires: []
excludes: []
valid_until: null
environment: DSH Desktop / SurfCore
created: 2026-11-19
last_hit: null
hits: 0
---

# 引擎-SurfCore-语音-503-角色映射

## 触发
SurfCore 桌宠语音报 `503 角色「X」没有配 [voice.characters] 映射`，语音服务起不来。

## 首选
`socialV2.voice.tts.character` **留空**（voxd 用默认音色）；或填一个已在 `[voice.characters]` 里映射过的 character。

## 被挡时
1. 检查配置里 `[voice.characters]` 是否真有该角色。
2. 别填音色 id（voice id）——那不是 character。

## 别再来
| 弯路 | 为什么不通 |
| --- | --- |
| 在 tts.character 填音色 id | 它要的是 character 名，映射不到就 503 |

## 注意
- 判据：语音服务不再 503，能正常合成。

证据：_inbox.md 2026-11-19 10:10

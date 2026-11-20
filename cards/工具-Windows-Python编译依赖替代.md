---
id: tool-win-python-native-deps
title: Windows + Python 3.12 装不了 C 扩展（pyopenjtalk/fasttext/jieba_fast）
type: trap
rarity: common
triggers:
  - pyopenjtalk 安装失败
  - fasttext 安装失败
  - distutils 报错
anchors:
  - pyopenjtalk
  - fasttext
  - distutils
  - ModuleNotFoundError
valid_until: 2027-06-30
environment: Windows / Python 3.12 / pip
created: 2026-11-19
last_hit: null
hits: 0
---

# 工具-Windows Python 编译依赖替代

## 触发
pip 装 pyopenjtalk-prebuilt、fasttext、jieba_fast 报错：`No module named 'distutils'`、`Cython is required`、`WinError 2`（要 cmake/MSVC）。

## 首选
- 先 `pip install "setuptools<70" wheel`（distutils shim）。
- fasttext → `pip install fasttext-wheel --no-deps`（有 cp312 预编译轮子）。
- opencc → `pip install opencc-python-reimplemented --no-deps`（GSV 合成报 400 No module named 'opencc'）。
- 装不上的纯功能依赖 → 造替身模块：
```
<venv>\Lib\site-packages\<name>\__init__.py
```
函数体抛 RuntimeError（只支持中文/用不到时）。

## 被挡时
- 确认 venv 的 `Lib\site-packages` 真实路径（别用 python -c 打印路径，控制台编码会乱）。
- 装完 `python -c "import <name>; print(1)"` 验证。

## 别再来
| 弯路 | 为什么不通 |
| --- | --- |
| `pip install fasttext` | 需 C++ 编译，失败 |
| `--no-build-isolation` 配 cython 重试 | 下一步要 cmake，仍然 FileNotFoundError |
| 用 python 打印 site-packages 取路径 | 中文路径被 GBK 控制台弄乱，写错目录 |

## 注意
- GPT-SoVITS 用 jieba_fast 但项目自带替身逻辑（bootstrap-env.mjs 的 ensureJiebaFastShim），先跑它。
- 判据：import 成功即过，不必追求真编译。
证据：2026-11-19 surfvoicelocal 安装。

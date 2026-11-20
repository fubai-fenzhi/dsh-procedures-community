---
id: tool-pip-torch-cu121-pin
title: pip 装可选包会顺手升级 torch，打坏 cu121 版
type: trap
rarity: common
triggers:
  - torch 版本不对
  - libtorchaudio 加载失败
  - x-transformers 装完 torch 变了
anchors:
  - cu121
  - torchaudio
  - x-transformers
  - dependency resolver
valid_until: 2027-06-30
environment: Windows / CUDA 12 / Python venv
created: 2026-11-19
last_hit: null
hits: 0
---

# 工具-pip 可选包升级 torch

## 触发
GPU 环境装 x-transformers / peft 等可选包后，`import torchaudio` 报 `OSError: Could not load this library: ...libtorchaudio.pyd`，或 torch.__version__ 变成非 cu121 版本。

## 首选
```
pip install torch==2.5.1+cu121 torchaudio==2.5.1+cu121 --index-url https://download.pytorch.org/whl/cu121
```
之后补装可选包时一律加 `--no-deps`。

## 被挡时
- 阿里云/清华的 pytorch-wheels 目录不是 PEP503 索引（直链 404）；用官方 `download.pytorch.org/whl/cu121`。
- 还要验证：`python -c "import torch;print(torch.__version__, torch.cuda.is_available())"` 应为 `2.5.1+cu121 True`。

## 别再来
| 弯路 | 为什么不通 |
| --- | --- |
| 直接 `pip install x-transformers` | 把 torch 升到 2.15.0，torchaudio 挂 |
| 用 `--index-url mirrors.aliyun.com/pytorch-wheels/cu121` | 不是 pip 索引，No matching distribution |
| 用 PyPI 的 torch | Windows 上是 CPU 轮子，CUDA 不可用 |

## 注意
- 装完检查 `pip check` 的 conflicts（accelerated-scan 之类会报 torch>=2.9）。
- 判据：torch.cuda.is_available() 为 True。
证据：2026-11-19 GPT-SoVITS 环境。

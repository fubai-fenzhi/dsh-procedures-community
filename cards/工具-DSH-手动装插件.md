---
id: tool-dsh-plugin-manual-install
title: 手动把 npm 插件装进 DSH profile（无 dsh CLI 时）
type: trap
rarity: common
triggers:
  - 安装 dsh 插件
  - 插件装不上
  - dsh plugin install 失败
anchors:
  - dsh.profile.bundles
  - ERR_PNPM_ABORTED_REMOVE_MODULES_DIR_NO_TTY
  - QQ_BRIDGE_SKIP_DSH_INSTALL
  - pnpm-lock
valid_until: 2027-06-30
environment: DSH Desktop / Windows
created: 2026-11-19
last_hit: null
hits: 0
---

# 工具-DSH 手动装插件

## 触发
要给 DSH 装 npm 插件（dshmarket / surfcore / dsh-plugin-wallpaper-engine / dsh-boot-suite），但系统里没有 `dsh` 命令，或安装脚本报 `dsh plugin install 返回退出码 1`。

## 首选
1. 备份 `C:\Users\Administrator\.dsh\profiles\desktop\package.json`
2. 加依赖：`dependencies["<包名>"]="<版本>"`，并追加到 `dsh.profile.bundles`
3. 用 DSH 自带 pnpm 装（ASCII 路径目录）：
```
node <dsh-runtimes>\pnpm\bin\pnpm.mjs install --registry=https://registry.npmmirror.com --reporter=append-only --no-frozen-lockfile
```
env 必须带 `CI=true`（非交互环境会拒绝重建 node_modules）。
4. 重启 DSH 生效。

## 被挡时
- 安装脚本要调 `dsh plugin ...`：加环境变量 `QQ_BRIDGE_SKIP_DSH_INSTALL=1` 跳过 CLI，脚本其余步骤照跑。
- pnpm 从官网超时：换 `--registry=https://registry.npmmirror.com`。

## 别再来
| 弯路 | 为什么不通 |
| --- | --- |
| 普通 pnpm install | `ERR_PNPM_ABORTED_REMOVE_MODULES_DIR_NO_TTY` |
| 用 npm 官网源 | 连接超时（国内） |
| 只加 dependencies 不加 bundles | 包在 node_modules 但插件不加载 |

## 注意
- bundles 决定组合顺序；插件自带 cordis.patch.yml 由 bundle 机制自动合并。
- 判据：`node_modules/<包>` 存在 + 重启后设置里出现插件页。
证据：2026-11-19 安装 dshmarket / surfcore / wallpaper / boot-suite 全部走此路径。

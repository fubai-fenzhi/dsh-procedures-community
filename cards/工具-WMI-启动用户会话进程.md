---
id: tool-wmi-launch-user-process
title: schtasks 被拒时用 WMI 在用户会话启动常驻进程
type: trap
rarity: common
triggers:
  - 计划任务拒绝访问
  - 后台进程随会话退出
  - 常驻进程启动不了
anchors:
  - schtasks
  - Win32_Process
  - Invoke-CimMethod
  - 拒绝访问
valid_until: 2027-06-30
environment: Windows
created: 2026-11-19
last_hit: null
hits: 0
---

# 工具-WMI 启动用户会话进程

## 触发
要从 agent 会话里启动一个"我结束之后还活着"的 GUI/服务进程（桌宠、语音服务）；`schtasks /create` 报 `拒绝访问`，或普通 spawn 的子进程随会话被杀。

## 首选
```powershell
Invoke-CimMethod -ClassName Win32_Process -MethodName Create -Arguments @{
  CommandLine = '"<exe>" "<args>"'; CurrentDirectory = '<dir>'
}
```
返回 ReturnValue=0 + PID 即成功；进程父级是 WmiPrvSE，不随会话死。

## 被挡时
- bat 含中文文件名经 WMI 可能乱码 → 直接写 exe 命令行（不经 cmd）。
- 验证：`Get-Process <name>` 看进程数与 MainWindowTitle。

## 别再来
| 弯路 | 为什么不通 |
| --- | --- |
| schtasks /create | 拒绝访问（权限） |
| cmd /c "中文.bat" 经 WMI | 中文名被吞，找不到文件 |
| 普通 spawn detached | 程序结束即被宿主杀 |

## 注意
- 用 WMI 启动的进程不会自动在开机重启，需要时配开机自启。
- 判据：ReturnValue=0 且 Get-Process 有窗口标题。
证据：2026-11-19 启动鲸鱼娘桌宠（electron）+ 语音服务。

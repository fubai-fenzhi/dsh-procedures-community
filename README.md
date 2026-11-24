# dsh-procedures-community

`dsh-procedures` 的社区经验卡库：存放经维护者审核的卡，用户通过 `procedures community pull` 拉取到本地全局库。

## 怎么提交

### 方式 A：本地一键生成提交链接（推荐，不需要 token）

```bash
node bin/procedures.mjs contribute --open
```

会输出预填好的 GitHub issue 链接；浏览器打开点 **Create** 即可（需要 GitHub 账号）。

### 方式 B：直接开 issue

在本仓库开 issue：标题 `[card] <文件名>`，正文贴完整卡（YAML frontmatter + 正文）。

### 方式 C：没有 GitHub 账号

把卡片内容发给维护者（QQ 群 / 私聊），维护者代开 issue 并合并。

## 审核与合并

维护者审核 → 合并进 `cards/` → 用户跑 `procedures community pull`。

## 卡格式

见 `dsh-procedures` 仓库的 `procedures/_模板.md`：YAML frontmatter（id/title/type/anchors/triggers）+ 触发 / 首选 / 被挡时 / 别再来 / 注意。

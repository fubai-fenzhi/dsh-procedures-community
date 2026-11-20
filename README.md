# dsh-procedures-community

社区经验卡库：存放经维护者审核的 `dsh-procedures` 经验卡。

## 如何贡献

本地用 CLI 提交（推荐，内容会先预览）：

```bash
node bin/procedures.mjs contribute          # 预览要提交的卡
node bin/procedures.mjs contribute --yes    # 提交（每张卡开一个 issue）
```

也可以直接在本仓库开 issue：标题 `[card] <文件名>`，正文贴完整卡（YAML frontmatter + 正文）。

维护者审核后把卡合并进 `cards/`；用户用 `procedures community pull` 拉取。
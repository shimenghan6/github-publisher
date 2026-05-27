# github-publisher

Claude Code 项目管理与发布技能。将本地项目整理到统一目录，自动同步、去硬编码、git init、生成 README，一条命令完成。

## 安装

```bash
cp SKILL.md ~/.claude/skills/github-publisher/
```

## 使用

在 Claude Code 中说：

> "把 xxx 发布到 GitHub"
> "整理项目推 GitHub"
> "更新 browser-control"

Agent 自动执行：
1. 同步源文件到 `~/github-repos/<project>/`
2. 检查并替换硬编码路径
3. git init（如果是新项目）
4. 生成 .gitignore
5. 准备推送

## 核心目录

```
~/github-repos/
├── claude-code-wechat/          # 微信桥接
├── browser-control/             # 浏览器操控
├── github-research/             # GitHub 调研
├── github-skill-downloader/     # 免 clone 下载
└── github-publisher/            # 发布管理
```

## License

MIT

# github-publisher

> "把这 6 个项目发布到 GitHub"——AI 打开浏览器，登录、填表、建仓库、推代码。6 个项目全上线，你一句话没多说，一下鼠标没碰。

**全球第一个能操控浏览器替你建 GitHub 仓库并推送的 AI Agent 技能。不依赖 gh CLI，不依赖 Personal Access Token。**

### 谁需要这个

| 你 | 为什么你需要 |
|----|------------|
| 有一批本地项目想批量推 GitHub | 6 个项目一键全推，不用逐个手动建仓库 |
| 公司网络封了 `gh` CLI | 直接操控浏览器填表建仓库，绕过 API 限制 |
| 想了解 AI 怎么操控浏览器 | 实战案例：CDP + React 表单注入 + eval 两步法 |

---

## 安装

```bash
bash install.sh      # macOS/Linux
install.bat          # Windows 双击
```

## 使用

在 Claude Code 中说：

> "把 xxx 发布到 GitHub"
> "整理项目推 GitHub"
> "更新 browser-control"

Agent 自动执行：
1. 同步源文件到 `~/github-repos/<project>/`
2. 检查并替换硬编码路径
3. git init（如果是新项目）+ 生成 .gitignore
4. 如果没有 README → 生成最小模板
5. 创建一键安装脚本
6. 准备推送

---

## 核心技术：浏览器直控 GitHub

传统 GitHub 自动化需要 `gh` CLI 或 Personal Access Token——但网络受限环境（企业防火墙/国内网络）下经常不可用。

这个项目的做法是：**AI Agent 直接操控你电脑上的浏览器**，像真人一样打开 github.com/new、填表、点按钮。绕过 API 限制，完全在浏览器端完成。

| 技术点 | 用途 |
|--------|------|
| CDP（Chrome DevTools Protocol） | 连接浏览器，注入控制 |
| `nativeInputValueSetter` | 绕过 React 表单验证 |
| eval 两步法 | 规避 setTimeout 异步陷阱 |
| SSH ed25519 | 绕过 GitHub HTTPS 认证墙 |

## 6 个踩坑记录

完整记录在 [SKILL.md](./SKILL.md) 中，涵盖 SSH 密钥类型选择、CDP 端口连接、React 表单注入、eval 异步陷阱、网络限制绕过等真实场景。

---

## 适用场景

- 有一批本地 skill/项目想批量推到 GitHub
- 公司网络封锁了 GitHub API，`gh` CLI 不可用
- 想了解 AI Agent 操控浏览器的实战案例
- 研究 CDP/浏览器自动化与 AI 的结合

---

## 文件结构

```
github-publisher/
├── README.md           # 本文件
├── SKILL.md            # Claude Code 技能定义（含 6 个踩坑）
├── install.bat         # Windows 一键安装
└── install.sh          # macOS/Linux 一键安装
```

## License

MIT

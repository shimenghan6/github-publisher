# github-publisher

**让 AI Agent 直接操控浏览器，帮你建仓库、填表单、推代码到 GitHub。**

你只需要说一句"把这些项目发布到 GitHub"，Claude Code 就会打开浏览器、自动登录、逐个创建仓库、填写表单、提交代码、一键推送——全程不需要你碰鼠标。

> 首次推广建议附上这句：**第一个能操控浏览器替你建 GitHub 仓库并推送的 AI Agent 技能。全程自动化，从本地文件到线上仓库，一句话搞定。**

---

## 能做什么

```
"把这 6 个项目发布到 GitHub"
         │
         ▼
  AI Agent 自动执行：
  ├─ 打开 Edge/Chrome，定位到 github.com/new
  ├─ 用 JavaScript 注入绕过 React 表单验证
  ├─ 逐个填表、点击"Create repository"
  ├─ 6 个仓库全部建完
  ├─ SSH 推送所有本地代码
  └─ 完成。你全程没碰键盘。
```

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

---

## 核心技术：浏览器直控 GitHub

传统方式需要 `gh` CLI 或 Personal Access Token——但这两种在网络受限环境（企业防火墙/国内网络）经常不可用。

这个项目的做法是：**AI Agent 直接操控你电脑上的浏览器**，像真人一样填表点按钮，绕过所有 API 限制。

涉及的硬核技术：

| 技术点 | 用途 |
|--------|------|
| Chrome DevTools Protocol (CDP) | 连接浏览器，注入控制 |
| `nativeInputValueSetter` | 绕过 React 表单验证 |
| eval 两步法 | 规避 setTimeout 异步陷阱 |
| SSH ed25519 | 绕过 GitHub HTTPS 认证墙 |
| SPA DOM 直接注入 | 放弃 snapshot，直接查 DOM |

## 6 个踩坑记录

完整记录在 [SKILL.md](./SKILL.md) 中，涵盖 SSH 密钥类型选择、CDP 端口连接、React 表单注入、eval 异步陷阱、网络限制绕过等真实场景。

---

## 适用场景

- 你有一堆本地 skill/项目想批量推到 GitHub
- 公司网络封锁了 GitHub API，`gh` CLI 不可用
- 你想看 AI Agent 怎么操控浏览器完成复杂操作
- 你在研究 CDP/浏览器自动化与 AI 的結合

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

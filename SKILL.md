---
name: github-publisher
description: |
  GitHub 项目管理与发布。将 Claude Code 技能/项目整理到 ~/github-repos/，
  自动同步、去硬编码、init git、生成 README 模板、准备推送。
  触发条件："发布到github", "整理项目", "推送到github", "准备开源",
  "publish to github", "github发布", "把xxx推github"
---

# GitHub 发布技能

## 首次初始化（最重要）

**任何发布操作前，必须先确保环境就绪：**

```
1. 检查 ~/github-repos/ 是否存在
   → 不存在则 mkdir ~/github-repos/，并创建根 README.md
2. 检查 ~/.claude/skills/ 是否存在
   → 不存在则提示用户"没有 skills 目录"
3. 检查 git 是否可用
   → 不可用则提示安装
```

## 核心目录

所有待发布项目统一在：`~/github-repos/`。每个子目录是一个独立 Git 仓库。

## 工作流程

### 1. 新增项目（含容错）

```
用户说："把 xxx 发布到 GitHub"

→ 1. 确保 ~/github-repos/ 存在（不在则 mkdir）
→ 2. 检查源文件从哪里来：
     - ~/.claude/skills/<name>/ 存在 → 从这里同步
     - 不存在 → 问用户"源文件在哪"
→ 3. 创建 ~/github-repos/<name>/
→ 4. 复制所有源文件（排除 account.json, credentials.json, *.log, __pycache__）
→ 5. 如果还没 git init → git init
→ 6. 创建 .gitignore（不存在则生成）
→ 7. 扫描所有文件，替换硬编码路径:
     - C:/Users/shish → 环境变量自动检测
     - C:\Users\shish → 跨平台兼容写法
→ 8. 如果没有 README.md → 生成最小模板
→ 9. ★ 创建一键安装脚本（铁律，见下方"一键安装脚本要求"）
→ 10. 更新根 README.md 的项目列表
→ 11. 更新本技能的项目清单
```

### 2. 更新已有项目

```
用户说："更新 browser-control"

→ 1. 确认 ~/github-repos/<name>/ 已存在（不在则走"新增"流程）
→ 2. 确认 ~/.claude/skills/<name>/ 有更新源（不在则跳过同步步骤）
→ 3. 同步源文件（不覆盖 .git 目录，不覆盖用户手动改过的 README）
→ 4. 重新扫描硬编码路径
```

### 3. 发布前检查清单（逐项输出结果）

```
□ ~/github-repos/ 目录存在
□ 项目目录存在
□ .git 已初始化
□ .gitignore 包含 node_modules/、__pycache__/、*.log
□ 无硬编码用户路径（搜索 C:/Users/、/home/）
□ README.md 包含：安装方式、使用说明、依赖列表
□ ★ 一键安装脚本已创建（install.ps1 / install.sh / install.py）
□ account.json、credentials.json 不在仓库中
□ 没有 node_modules/ 目录（不应该提交）
```

### 4. 推送到 GitHub

```bash
cd ~/github-repos/<项目名>
git add -A
git commit -m "<提交信息>"
# 用户需先在 GitHub 创建空仓库，然后：
git remote add origin <repo-url>
git push -u origin main
```

## 项目清单（当前）

| 项目 | 目录 | 状态 |
|------|------|------|
| claude-code-wechat | `~/github-repos/claude-code-wechat/` | 已 init，待推送 |
| browser-control | `~/github-repos/browser-control/` | 已 init，待推送 |
| github-research | `~/github-repos/github-research/` | 已 init，待推送 |
| github-skill-downloader | `~/github-repos/github-skill-downloader/` | 已 init，待推送 |
| github-publisher | `~/github-repos/github-publisher/` | 已 init，待推送 |
| claude-code-sound-notifier | `~/github-repos/claude-code-sound-notifier/` | 已 init，待推送 |

## 硬编码路径替换规则

| 原始 | 替换为 |
|------|--------|
| `C:/Users/shish/AppData/Roaming/npm/node_modules` | `execSync("npm root -g")` |
| `C:\\Users\\shish\\...\\python.exe` | `getPython()` 自动检测 |
| `C:/Users/shish/.claude/` | `resolve(homedir(), ".claude")` |

## .gitignore 模板

```
node_modules/
__pycache__/
*.pyc
*.log
.env
account.json
credentials.json
```

## 一键安装脚本要求（铁律）

**每个即将推送 GitHub 的项目，必须提供一键安装方式。** 不允许用户手动复制粘贴多步操作。

### 脚本文件命名

| 场景 | 文件名 | 说明 |
|------|--------|------|
| Windows 专属 | `install.ps1` | PowerShell 脚本，右键运行 |
| macOS/Linux | `install.sh` | bash 脚本，curl pipe |
| 跨平台 Python | `install.py` | Python 脚本，适用于复杂安装 |
| Node.js / npm 包 | 在 `package.json` 中配 `bin` | `npm install -g` 即可 |

### 脚本必须做到

1. **自动检测环境**：用户路径、Python 版本、系统类型等
2. **不影响已有配置**：如果是 JSON 合并，追加不覆盖
3. **有试跑反馈**：装完后立即验证，告诉用户成功还是失败
4. **不丢异常**：文件不存在、依赖缺失等情况要有中文提示

### README 安装栏格式

```markdown
## 一键安装

### Windows
```powershell
powershell -ExecutionPolicy Bypass -Command "irm https://raw.githubusercontent.com/<用户名>/<项目>/main/install.ps1 | iex"
```

### macOS / Linux
```bash
curl -fsSL https://raw.githubusercontent.com/<用户名>/<项目>/main/install.sh | bash
```
```

### 各类型项目的安装脚本模板

#### 1. Skill 类（复制到 ~/.claude/skills/）

```
安装流程：curl 下载 SKILL.md → 放到 ~/.claude/skills/<name>/ → 提示重启 Claude Code
```

#### 2. 配置类（合并到 settings.json）

```
安装流程：读现有 settings.json → 追加 hook/permission → 写回 → 提示开新对话
```

#### 3. Python 工具类

```
安装流程：pip install → 验证 CLI 可用 → 提示使用方式
```

#### 4. Node.js 工具类

```
安装流程：npm install -g → 验证命令可用 → 提示使用方式
```

## README.md 最小模板

```markdown
# 项目名

一句话描述。

## 安装

## 使用

## 依赖

## License

MIT
```

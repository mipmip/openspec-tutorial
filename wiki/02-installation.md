> [<< 第一章：基础概念](01-basics.md) | [目录](Home.md) | [第三章：核心命令 >>](03-commands.md)

---

# 第二章：安装与配置

## 环境要求

- **Node.js**: 20.19.0 或更高版本
- **npm**: 10.x 或更高版本

检查 Node.js 版本：
```bash
node --version
```

如果版本不够，请先升级 Node.js：
- 官网下载：https://nodejs.org/
- 或使用 nvm/fnm 管理版本

---

## 安装 OpenSpec

### 全局安装（推荐）

```bash
npm install -g @fission-ai/openspec@latest
```

### 使用其他包管理器

```bash
# 使用 pnpm
pnpm add -g @fission-ai/openspec@latest

# 使用 yarn
yarn global add @fission-ai/openspec@latest

# 使用 bun
bun add -g @fission-ai/openspec@latest
```

### 验证安装

```bash
openspec --version
```

应该输出类似：`@fission-ai/openspec/1.2.0`

---

## 初始化项目

### 全新项目

```bash
# 创建项目目录
mkdir my-project
cd my-project

# 初始化 OpenSpec
openspec init
```

输出示例：
```
✓ Created openspec/ directory
✓ Created openspec/changes/ directory
✓ Created openspec/archive/ directory
✓ Created AGENTS.md
✓ Initialized OpenSpec in my-project/
```

### 已有项目

```bash
# 直接进入已有项目目录
cd existing-project

# 初始化（不会覆盖现有文件）
openspec init
```

OpenSpec 是**非侵入式**的，不会修改你的现有代码。

---

## 项目结构

初始化后，你的项目会有：

```
my-project/
├── openspec/                 # OpenSpec 目录
│   ├── changes/             # 正在进行的变更
│   │   └── (空)
│   └── archive/             # 已完成的变更
│       └── (空)
├── AGENTS.md                # AI 助手配置
└── (你的原有项目文件)
```

### AGENTS.md 文件

这是给 AI 助手看的配置文件：

```markdown
# OpenSpec Agent Configuration

## Commands
- `/opsx:propose <description>` - Create a new change proposal
- `/opsx:apply` - Apply the current change
- `/opsx:archive` - Archive the completed change

## Workflow
1. Use `/opsx:propose` to start a new feature
2. Review the generated specs
3. Use `/opsx:apply` to implement
4. Use `/opsx:archive` when done
```

你可以编辑这个文件，添加项目特定的上下文信息。

---

## 配置工作流

### 查看当前配置

```bash
openspec config profile
```

### 选择工作流

OpenSpec 提供两种工作流：

#### 1. 精简模式（默认）

适合快速迭代：
```bash
openspec config profile minimal
```

可用命令：
- `/opsx:propose` - 创建提案
- `/opsx:apply` - 应用实现
- `/opsx:archive` - 归档

#### 2. 完整模式

适合复杂项目：
```bash
openspec config profile full
```

可用命令：
- `/opsx:new` - 新建变更
- `/opsx:propose` - 创建提案
- `/opsx:continue` - 继续实现
- `/opsx:ff` - 快进（自动实现）
- `/opsx:apply` - 应用实现
- `/opsx:verify` - 验证实现
- `/opsx:sync` - 同步状态
- `/opsx:bulk-archive` - 批量归档
- `/opsx:onboard` - 项目引导

### 应用配置

```bash
openspec update
```

这会更新 AGENTS.md 文件中的命令列表。

---

## 配置 AI 工具

### 支持的 AI 工具

OpenSpec 支持 20+ 种 AI 编程助手：

| 工具 | 配置方式 |
|------|---------|
| Claude Code | 自动检测 |
| Codex (OpenAI) | 自动检测 |
| Pi | 自动检测 |
| Kiro | 自动检测 |
| Cursor | 安装 Cursor 扩展 |
| GitHub Copilot | 使用 Copilot Chat |

### 检查支持状态

```bash
openspec doctor
```

输出示例：
```
✓ Node.js 20.19.0
✓ OpenSpec 1.2.0
✓ Claude Code detected
✓ Git repository detected
✓ AGENTS.md is up to date
```

---

## 第一个命令

安装完成后，试试第一个命令：

```bash
# 在项目目录中
/opsx:propose "添加一个 README 文件"
```

AI 会创建：
```
openspec/changes/add-readme/
├── proposal.md
├── specs/
├── design.md
└── tasks.md
```

查看生成的文件，了解 OpenSpec 的文档结构。

---

## 常见问题

### Q: 安装失败，提示权限错误

```bash
# macOS/Linux
sudo npm install -g @fission-ai/openspec@latest

# 或使用 npx（无需安装）
npx @fission-ai/openspec@latest
```

### Q: 命令找不到

```bash
# 检查 npm 全局安装路径
npm config get prefix

# 确保路径在 PATH 中
export PATH="$PATH:$(npm config get prefix)/bin"
```

### Q: 如何更新 OpenSpec

```bash
npm install -g @fission-ai/openspec@latest
openspec update
```

---

> [<< 第一章：基础概念](01-basics.md) | [目录](Home.md) | [第三章：核心命令 >>](03-commands.md)
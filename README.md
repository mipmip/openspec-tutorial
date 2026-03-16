# OpenSpec 教程 - 从零开始学习规范驱动开发

一个面向初学者的 OpenSpec 完整入门教程，通过实际案例学习如何与 AI 协作进行规范驱动开发。

## 🌐 在线阅读

**教程网站**: https://aiyinluya.github.io/openspec-tutorial/

无需安装任何工具，直接在浏览器中阅读完整教程。

## 📚 项目简介

OpenSpec 是一个 **AI 编程助手的 Spec-Driven Development（规范驱动开发）框架**。它让你在写代码之前，先与 AI 对齐需求、设计和任务，避免"做出来不是想要的"的尴尬。

### 核心哲学

```
→ fluid not rigid          → 流体而非僵化
→ iterative not waterfall  → 迭代而非瀑布
→ easy not complex         → 简单而非复杂
→ built for brownfield     → 适用于已有项目
→ scalable                 → 从个人项目到企业级
```

## 🚀 快速开始

### 安装 OpenSpec

```bash
# 需要 Node.js 20.19.0+
npm install -g @fission-ai/openspec@latest
```

### 初始化项目

```bash
# 进入你的项目目录
cd your-project

# 初始化 OpenSpec
openspec init
```

### 第一个规范

```bash
# 告诉 AI 你想做什么
/opsx:propose "添加用户登录功能"
```

AI 会自动创建：
```
openspec/changes/add-user-login/
├── proposal.md      # 提案：为什么要做
├── specs/           # 规范：需求细节
│   └── requirements.md
├── design.md        # 设计：技术方案
└── tasks.md         # 任务：实现清单
```

### 开始实现

```bash
# AI 按 tasks.md 自动实现
/opsx:apply
```

## 📖 教程大纲

| 章节 | 内容 | 文件 |
|------|------|------|
| 01-基础概念 | OpenSpec 是什么、为什么需要它 | [docs/01-basics.md](docs/01-basics.md) |
| 02-安装配置 | 安装、初始化、配置 | [docs/02-installation.md](docs/02-installation.md) |
| 03-核心命令 | /opsx:propose, /opsx:apply 等 | [docs/03-commands.md](docs/03-commands.md) |
| 04-工作流 | 完整开发流程演示 | [docs/04-workflow.md](docs/04-workflow.md) |
| 05-项目结构 | openspec/ 目录详解 | [docs/05-structure.md](docs/05-structure.md) |
| 06-文档规范 | proposal、design、tasks 怎么写 | [docs/06-documentation.md](docs/06-documentation.md) |
| 07-实战案例 | 完整项目实战 | [docs/07-examples.md](docs/07-examples.md) |
| 08-团队协作 | 多人协作最佳实践 | [docs/08-teamwork.md](docs/08-teamwork.md) |
| 09-高级技巧 | 进阶用法和技巧 | [docs/09-advanced.md](docs/09-advanced.md) |
| 10-常见问题 | FAQ 和故障排除 | [docs/10-faq.md](docs/10-faq.md) |

## 🎯 实战案例

本项目包含多个实战案例：

- **Todo App** - 简单的待办事项应用
- **Blog System** - 博客系统（用户、文章、评论）
- **E-commerce API** - 电商 API（商品、订单、支付）

## 🛠️ 支持的 AI 工具

OpenSpec 支持 20+ 种 AI 编程助手：

- Claude Code (Claude)
- Codex (OpenAI)
- Pi (Mario Zechner)
- Kiro (AWS)
- Cursor
- GitHub Copilot
- 以及更多...

## 📂 项目结构

```
openspec-tutorial/
├── README.md
├── docs/                    # 教程文档
│   ├── 01-basics.md
│   ├── 02-installation.md
│   └── ...
├── examples/                # 实战案例
│   ├── todo-app/
│   ├── blog-system/
│   └── e-commerce/
├── openspec/               # OpenSpec 生成的规范目录
│   └── changes/
├── scripts/                # 辅助脚本
└── LICENSE
```

## 🤝 贡献

欢迎提交 Issue 和 PR！

## 📄 许可证

MIT License

---

**开始学习** → [docs/01-basics.md](docs/01-basics.md)

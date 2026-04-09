# 第三章：核心命令

## 命令分为两类

OpenSpec 的命令分为 **CLI 命令** 和 **AI-native 命令**，它们的用法完全不同：

| 类型 | 使用方式 | 示例 |
|------|---------|------|
| **CLI 命令** | 在终端中运行 | `openspec new change xxx` |
| **AI-native 命令** | 在 AI 工具的对话中使用 | `/opsx:propose "xxx"` |

> **重要**：教程中会明确标注每个命令的类型。如果你在终端里运行 AI-native 命令（如 `/opsx:propose`），会得到"找不到命令"的错误。

---

## CLI 命令概览

在终端中使用的命令：

| 命令 | 说明 | 类型 |
|------|------|------|
| `openspec init` | 初始化项目 | CLI |
| `openspec update` | 更新 OpenSpec 文件 | CLI |
| `openspec new change <name>` | 创建新变更 | CLI |
| `openspec list` | 列出所有变更 | CLI |
| `openspec show <name>` | 显示变更详情 | CLI |
| `openspec archive [name]` | 归档变更 | CLI |
| `openspec validate [name]` | 验证变更/规范 | CLI |
| `openspec status` | 查看变更完成状态 | CLI |
| `openspec view` | 交互式仪表盘 | CLI |
| `openspec config ...` | 查看/修改配置 | CLI |
| `openspec change show <name>` | 显示提案详情 | CLI |
| `openspec change validate <name>` | 验证提案 | CLI |

---

## CLI: openspec init

初始化 OpenSpec 项目：

```bash
openspec init [options] [path]
```

### 常用选项

| 选项 | 说明 |
|------|------|
| `--tools <tools>` | 指定 AI 工具（逗号分隔），如 `claude,cursor` |
| `--profile <profile>` | 指定工作流预设，如 `core` |
| `--force` | 自动清理旧文件，不提示 |

### 示例

```bash
# 初始化当前目录
openspec init

# 指定 AI 工具
openspec init --tools claude,cursor,github-copilot

# 指定工作流预设
openspec init --profile core

# 强制覆盖（不询问）
openspec init --force
```

---

## CLI: openspec new

创建新的变更（Change）：

```bash
openspec new change <name>
```

### 示例

```bash
# 创建变更目录
openspec new change "add-readme"
openspec new change add-user-login
openspec new change fix-mobile-layout
```

> 注意：变更名称使用 kebab-case（短横线分隔）。

---

## CLI: openspec list

列出项目中的变更：

```bash
openspec list [options]
```

### 常用选项

| 选项 | 说明 |
|------|------|
| `--specs` | 列出规范（specs）而不是变更（changes） |
| `--json` | 输出 JSON 格式（便于程序处理） |
| `--sort <order>` | 排序方式：`recent`（默认）或 `name` |

### 示例

```bash
# 列出所有进行中的变更
openspec list

# 列出所有规范
openspec list --specs

# 输出 JSON 格式
openspec list --json

# 按名称排序
openspec list --sort name
```

---

## CLI: openspec show

显示变更或规范的详细信息：

```bash
openspec show [item-name] [options]
```

### 常用选项

| 选项 | 说明 |
|------|------|
| `--json` | 输出 JSON 格式 |
| `--type <type>` | 指定类型：`change` 或 `spec` |
| `--no-interactive` | 禁用交互式提示 |
| `--deltas-only` | 仅显示增量（JSON 格式） |

### 示例

```bash
# 显示变更详情
openspec show add-user-login

# 输出 JSON 格式
openspec show add-user-login --json

# 显示规范详情
openspec show my-spec --type spec
```

---

## CLI: openspec archive

归档已完成的变更：

```bash
openspec archive [change-name] [options]
```

### 常用选项

| 选项 | 说明 |
|------|------|
| `-y, --yes` | 跳过确认提示 |
| `--skip-specs` | 跳过规范更新（仅归档） |
| `--no-validate` | 跳过验证（不推荐） |

### 示例

```bash
# 归档变更（会提示确认）
openspec archive add-user-login

# 跳过确认
openspec archive add-user-login --yes

# 跳过规范更新（仅归档）
openspec archive add-user-login --skip-specs

# 归档并跳过验证
openspec archive add-user-login --yes --no-validate
```

### AI 会做什么

归档时，OpenSpec 会：
1. 检查所有任务是否完成
2. 更新主规范文档
3. 将变更移动到 `archive/` 目录
4. 生成变更摘要（summary.md）

---

## CLI: openspec validate

验证变更或规范：

```bash
openspec validate [item-name] [options]
```

### 常用选项

| 选项 | 说明 |
|------|------|
| `--all` | 验证所有变更和规范 |
| `--changes` | 验证所有变更 |
| `--specs` | 验证所有规范 |
| `--strict` | 启用严格验证模式 |
| `--json` | 输出 JSON 格式 |
| `--type <type>` | 指定类型 |

### 示例

```bash
# 验证单个变更
openspec validate add-user-login

# 验证所有变更
openspec validate --changes

# 验证所有（变更 + 规范）
openspec validate --all

# 严格模式
openspec validate add-user-login --strict
```

---

## CLI: openspec status

查看变更的任务完成状态：

```bash
openspec status [options]
```

### 常用选项

| 选项 | 说明 |
|------|------|
| `--change <id>` | 查看指定变更的状态 |
| `--schema <name>` | 覆盖默认 schema |
| `--json` | 输出 JSON 格式 |

### 示例

```bash
# 查看当前变更状态
openspec status

# 查看指定变更的状态
openspec status --change add-user-login

# 输出 JSON
openspec status --change add-user-login --json
```

---

## CLI: openspec view

交互式仪表盘，查看所有变更和规范的状态：

```bash
openspec view
```

---

## CLI: openspec config

查看和修改 OpenSpec 配置：

```bash
openspec config [command] [options]
```

### 子命令

| 命令 | 说明 |
|------|------|
| `openspec config path` | 显示配置文件路径 |
| `openspec config list` | 列出所有配置 |
| `openspec config get <key>` | 获取某个配置值 |
| `openspec config set <key> <value>` | 设置配置值 |
| `openspec config unset <key>` | 删除配置（恢复默认值） |
| `openspec config reset` | 重置所有配置 |
| `openspec config edit` | 在编辑器中打开配置 |
| `openspec config profile [preset]` | 选择工作流预设 |

### 示例

```bash
# 查看配置文件路径
openspec config path

# 列出所有配置
openspec config list

# 获取某个配置值
openspec config get profile

# 设置配置值
openspec config set profile core

# 选择工作流预设
openspec config profile
openspec config profile core
```

---

## CLI: openspec update

更新 OpenSpec 指令文件（如 AGENTS.md）：

```bash
openspec update [options] [path]
```

### 常用选项

| 选项 | 说明 |
|------|------|
| `--force` | 强制更新，即使工具已是最新的 |

### 示例

```bash
# 更新当前目录
openspec update

# 强制更新
openspec update --force
```

---

## CLI: openspec instructions

输出创建 artifact 或执行任务时的详细指令：

```bash
openspec instructions [artifact] [options]
```

### 常用选项

| 选项 | 说明 |
|------|------|
| `--change <id>` | 指定变更 |
| `--schema <name>` | 指定 schema |
| `--json` | 输出 JSON 格式 |

### 示例

```bash
# 输出变更的详细指令
openspec instructions --change add-user-login

# 指定 schema
openspec instructions --change add-user-login --schema core
```

---

## AI-native 命令概览

> **重要**：这些命令不是 CLI 命令，是你在 AI 工具（如 Claude Code、Cursor）对话中使用的命令。它们的格式是 `/opsx:xxx`。

AI-native 命令由 OpenSpec 通过 `AGENTS.md` 文件注入到 AI 工具的上下文中。当你初始化项目后，这些命令会自动出现在 AI 的指令里。

### 命令列表

| 命令 | 说明 |
|------|------|
| `/opsx:propose <description>` | 创建新功能提案 |
| `/opsx:apply` | 应用实现 |
| `/opsx:archive` | 归档已完成变更 |
| `/opsx:verify` | 验证实现是否符合规范 |
| `/opsx:sync` | 同步规范与代码状态 |
| `/opsx:onboard` | 新成员项目引导 |

---

## AI-native: /opsx:propose

**最重要的命令**，用于创建新功能提案。

### 基本用法

```
/opsx:propose "你想做什么"
```

### 示例

```
# 添加新功能
/opsx:propose "添加用户注册功能"

# 修复问题
/opsx:propose "修复登录页面在移动端显示错乱的问题"

# 重构代码
/opsx:propose "重构用户模块，拆分为独立的 service 层"

# 性能优化
/opsx:propose "优化首页加载速度，目标 LCP < 2.5s"
```

### AI 会做什么

执行命令后，AI 会：

1. **分析你的描述**，理解需求
2. **使用 CLI 创建变更目录**：`openspec new change <change-name>`
3. **生成四个文档**：
   - `proposal.md` - 提案说明
   - `specs/requirements.md` - 详细需求
   - `design.md` - 技术设计
   - `tasks.md` - 实现任务
4. **请你确认**，有问题可以修改

### 确认和修改

AI 生成文档后，你应该：

```
✓ 阅读 proposal.md，确认目标正确
✓ 阅读 specs/，确认需求完整
✓ 阅读 design.md，确认技术方案合理
✓ 阅读 tasks.md，确认任务清单完整

如有问题，直接告诉 AI：
"tasks.md 里缺少单元测试的任务"
"design.md 里的数据库方案改用 PostgreSQL"
```

---

## AI-native: /opsx:apply

确认规范后，让 AI 开始实现。

### 基本用法

```
/opsx:apply
```

### AI 会做什么

1. **读取 tasks.md**，了解所有任务
2. **按顺序实现**每个任务
3. **实时更新进度**
4. **完成后汇报**结果

### 实现过程中

如果 AI 遇到问题，它会：
- 暂停并询问你
- 提供几个方案让你选择
- 记录问题到 tasks.md

---

## AI-native: /opsx:archive

功能完成后，归档变更记录。

### 基本用法

```
/opsx:archive
```

AI 会调用 CLI 的 archive 命令完成归档。

---

## AI-native: /opsx:verify

验证实现是否符合规范。

### 基本用法

```
/opsx:verify
```

AI 会：
1. 对比 specs/ 中的需求
2. 检查代码实现
3. 报告差异

### 示例输出

```
✓ 手机号登录 - 已实现
✓ 验证码 5 分钟过期 - 已实现
✗ 微信登录 - 未实现（tasks.md 1.4 未完成）
⚠ 登录失败次数限制 - 规范要求 5 次，代码实现为 3 次
```

---

## AI-native: /opsx:sync

同步规范文档与当前代码状态。

### 基本用法

```
/opsx:sync
```

使用场景：
- 手动修改了代码，需要更新规范
- 规范文档与代码出现了偏差

---

## AI-native: /opsx:onboard

为新加入的团队成员生成项目概览。

### 基本用法

```
/opsx:onboard
```

AI 会生成：
- 项目背景介绍
- 已完成的功能列表（来自 archive/）
- 正在进行的功能（来自 changes/）
- 技术栈和架构说明

---

## 命令使用技巧

### 1. CLI vs AI-native 不要混淆

```bash
# ❌ 在终端运行 AI-native 命令 —— 会失败
/opsx:propose "添加登录功能"
/opsx:apply
/opsx:archive

# ✅ CLI 命令在终端运行
openspec new change add-login
openspec list
openspec archive add-login

# ✅ AI-native 命令在 AI 对话中使用
/opsx:propose "添加登录功能"
/opsx:apply
/opsx:archive
```

### 2. 描述要具体

```bash
# ❌ 太模糊（AI-native 命令示例）
/opsx:propose "改进用户体验"

# ✅ 具体明确
/opsx:propose "在用户注册流程中添加邮箱验证步骤，用户注册后需要点击邮件中的链接才能激活账号"
```

### 3. 一次只做一件事

```bash
# ❌ 太多了
/opsx:propose "添加登录、注册、找回密码、修改密码、绑定手机号功能"

# ✅ 拆分成多个变更
/opsx:propose "添加用户登录功能"
# 完成后
/opsx:propose "添加用户注册功能"
# 完成后
/opsx:propose "添加找回密码功能"
```

### 4. 在 apply 前仔细审查规范

```
规范文档是你和 AI 的"合同"
apply 之前一定要确认规范是你想要的
修改规范比修改代码容易得多
```

### 5. 保持上下文干净

```bash
# 开始新功能前，清理 AI 的上下文
# 避免旧对话影响新功能的实现
```

---

## 下一步

→ [第四章：完整工作流](04-workflow.md)

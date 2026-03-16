# 第八章：团队协作

## OpenSpec 如何支持团队协作

### 问题：传统 AI 编程的团队协作

- 每个人的 AI 对话历史不同，理解不一致
- 没有统一的需求文档
- 不知道同事在做什么
- 代码审查时不知道背景

### 解决：OpenSpec 的团队模式

```
统一规范文档
    ↓
所有人基于相同理解
    ↓
AI 实现一致
    ↓
代码审查有依据
```

---

## 团队工作流

### 1. 需求评审

```bash
# 产品/BA 创建提案
/opsx:propose "添加用户评论功能"
```

**团队评审**：
- 产品经理审查 proposal.md
- 技术负责人审查 design.md
- 测试人员补充测试场景到 specs/

### 2. 分配任务

```markdown
# tasks.md

## 1. 后端 API（@后端开发）
- [ ] 1.1 创建 Comment 数据模型
- [ ] 1.2 实现评论 CRUD 接口

## 2. 前端页面（@前端开发）
- [ ] 2.1 创建评论列表组件
- [ ] 2.2 创建评论表单组件

## 3. 测试（@测试人员）
- [ ] 3.1 编写接口测试用例
- [ ] 3.2 编写 UI 自动化测试
```

### 3. 并行开发

每个开发者在自己的分支上工作，基于相同的规范：

```bash
# 后端开发者
git checkout -b feature/comments-backend
/opsx:apply task 1.1
/opsx:apply task 1.2

# 前端开发者
git checkout -b feature/comments-frontend
/opsx:apply task 2.1
/opsx:apply task 2.2
```

### 4. 代码审查

审查时有完整的规范文档作为依据：

```markdown
## PR 描述

**变更**：添加用户评论功能
**OpenSpec**：openspec/changes/add-comments/

**实现内容**：
- ✅ 创建 Comment 数据模型
- ✅ 实现评论 CRUD 接口
- ✅ 创建评论列表组件

**与规范的差异**：
- design.md 中计划用 WebSocket 实时更新，改为轮询（性能足够，实现更简单）
```

---

## 团队配置

### 共享 AGENTS.md

在项目根目录创建团队共享的 `AGENTS.md`：

```markdown
# OpenSpec Team Configuration

## 团队信息
- 团队：XX 研发团队
- 项目：XX 产品

## 技术栈
- 前端：Vue 3 + TypeScript + Vite
- 后端：Go + Gin
- 数据库：MySQL
- 缓存：Redis

## 规范
### 代码规范
- 使用 ESLint + Prettier
- 所有代码必须通过 CI 检查
- 单元测试覆盖率 > 80%

### Git 规范
- 分支命名：feature/xxx, bugfix/xxx
- Commit 使用 Conventional Commits
- PR 必须关联 OpenSpec 变更

### OpenSpec 规范
- 每个功能必须有完整的规范文档
- 技术方案必须经过技术负责人评审
- 复杂功能必须有设计评审会议

## 常用命令
- `/opsx:propose` - 创建新功能提案
- `/opsx:apply` - 实现功能
- `/opsx:onboard` - 新成员引导

## 注意事项
- 不要修改数据库 schema 没有 migration
- 敏感信息使用环境变量
- API 变更需同步更新文档
```

---

## 代码审查清单

### 审查时检查

```markdown
## PR 审查清单

### 规范对齐
- [ ] 实现是否符合 design.md 的技术方案？
- [ ] 是否覆盖了 specs/ 中的所有需求？
- [ ] 异常场景是否都处理了？

### 代码质量
- [ ] 代码是否符合团队规范？
- [ ] 是否有足够的单元测试？
- [ ] 是否有敏感信息泄露？

### 文档
- [ ] 是否需要更新 README？
- [ ] API 文档是否更新？
- [ ] 变更是否需要归档？
```

---

## 使用 /opsx:onboard

新成员加入时，快速了解项目：

```bash
/opsx:onboard
```

AI 会生成：

```markdown
# 项目概览

## 项目简介
XX 产品是一个...

## 已完成的功能
1. 用户认证系统（2025-03-01）
2. 任务管理（2025-03-05）
3. 评论功能（2025-03-10）

## 正在进行的功能
1. 文件上传（@张三）
2. 消息通知（@李四）

## 技术栈
- 前端：Vue 3 + TypeScript
- 后端：Go + Gin
- 数据库：MySQL

## 快速开始
1. git clone xxx
2. npm install
3. npm run dev

## 重要文档
- API 文档：/docs/api.md
- 架构文档：/docs/architecture.md
```

---

## 下一步

→ [第九章：高级技巧](09-advanced.md)

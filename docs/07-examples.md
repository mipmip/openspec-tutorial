# 第七章：实战案例 - Todo App

本章通过一个完整的 Todo App 项目，演示 OpenSpec 的实际使用。

## 项目背景

我们要从零开始构建一个 Todo App，包含：
- 任务的增删改查
- 任务分类和标签
- 任务优先级
- 完成状态管理

---

## 第一个变更：基础 CRUD

### 执行命令

```bash
/opsx:propose "创建 Todo App 的基础任务管理功能，支持创建、查看、更新、删除任务"
```

### 生成的 proposal.md

```markdown
# 提案：Todo App 基础任务管理

## 背景
构建 Todo App 的核心功能，让用户可以管理日常任务。

## 目标
- 实现任务的完整 CRUD 操作
- 提供简洁的用户界面

## 范围
**包含**：
- 创建任务（标题、描述、截止日期）
- 查看任务列表
- 更新任务信息
- 删除任务
- 标记任务完成/未完成

**不包含**：
- 用户认证（后续迭代）
- 任务分类（后续迭代）
- 任务共享（后续迭代）

## 成功标准
- 用户可以创建、查看、编辑、删除任务
- 任务数据持久化到数据库
- 界面响应时间 < 100ms
```

### 生成的 specs/requirements.md

```markdown
# 需求规范

## 数据结构
任务（Task）包含：
- id: 唯一标识
- title: 标题（必填，最多 100 字）
- description: 描述（可选，最多 500 字）
- due_date: 截止日期（可选）
- completed: 是否完成（默认 false）
- created_at: 创建时间
- updated_at: 更新时间

## 功能需求
1. 创建任务：填写标题（必填）、描述、截止日期
2. 查看列表：显示所有任务，按创建时间倒序
3. 查看详情：点击任务查看完整信息
4. 编辑任务：修改标题、描述、截止日期
5. 删除任务：删除后不可恢复，需要确认
6. 完成任务：点击复选框切换完成状态

## 场景

### 创建任务
1. 点击"新建任务"按钮
2. 填写标题（必填）
3. 可选填写描述和截止日期
4. 点击"保存"
5. 任务出现在列表顶部

### 删除任务
1. 点击任务的删除按钮
2. 弹出确认对话框："确定删除这个任务吗？"
3. 点击"确定"后删除
4. 任务从列表消失

### 完成任务
1. 点击任务前的复选框
2. 任务标题显示删除线
3. 任务移到列表底部
```

### 生成的 design.md

```markdown
# 技术设计

## 技术栈
- 前端：React + TypeScript + Tailwind CSS
- 后端：Node.js + Express
- 数据库：SQLite（开发）/ PostgreSQL（生产）
- ORM：Prisma

## API 设计

### GET /api/tasks
获取任务列表

**响应**：
```json
{
  "tasks": [
    {
      "id": 1,
      "title": "完成项目报告",
      "description": "需要包含数据分析",
      "due_date": "2025-03-20",
      "completed": false,
      "created_at": "2025-03-16T10:00:00Z"
    }
  ]
}
```

### POST /api/tasks
创建任务

**请求**：
```json
{
  "title": "完成项目报告",
  "description": "需要包含数据分析",
  "due_date": "2025-03-20"
}
```

### PUT /api/tasks/:id
更新任务

### DELETE /api/tasks/:id
删除任务

### PATCH /api/tasks/:id/toggle
切换完成状态

## 数据库 Schema

```prisma
model Task {
  id          Int       @id @default(autoincrement())
  title       String    @db.VarChar(100)
  description String?   @db.VarChar(500)
  due_date    DateTime?
  completed   Boolean   @default(false)
  created_at  DateTime  @default(now())
  updated_at  DateTime  @updatedAt
}
```
```

### 生成的 tasks.md

```markdown
# 实现任务

## 1. 项目初始化
- [ ] 1.1 初始化 Node.js 项目
- [ ] 1.2 安装依赖（Express, Prisma, TypeScript）
- [ ] 1.3 配置 TypeScript
- [ ] 1.4 初始化 React 前端

## 2. 数据库
- [ ] 2.1 配置 Prisma
- [ ] 2.2 创建 Task 数据模型
- [ ] 2.3 运行数据库迁移

## 3. 后端 API
- [ ] 3.1 GET /api/tasks - 获取列表
- [ ] 3.2 POST /api/tasks - 创建任务
- [ ] 3.3 PUT /api/tasks/:id - 更新任务
- [ ] 3.4 DELETE /api/tasks/:id - 删除任务
- [ ] 3.5 PATCH /api/tasks/:id/toggle - 切换状态

## 4. 前端页面
- [ ] 4.1 创建任务列表组件
- [ ] 4.2 创建任务表单组件
- [ ] 4.3 创建任务详情组件
- [ ] 4.4 实现 API 调用
- [ ] 4.5 添加加载状态和错误处理

## 5. 测试
- [ ] 5.1 API 单元测试
- [ ] 5.2 前端组件测试
```

---

## 第二个变更：添加分类功能

基础功能完成后，继续添加分类：

```bash
/opsx:propose "为 Todo App 添加任务分类功能，用户可以创建分类并将任务归入分类"
```

---

## 第三个变更：添加优先级

```bash
/opsx:propose "为任务添加优先级（高/中/低），任务列表按优先级排序"
```

---

## 查看项目演进

完成多个变更后，archive/ 目录记录了完整历史：

```
openspec/archive/
├── 2025-03-16-basic-crud/
├── 2025-03-17-add-categories/
└── 2025-03-18-add-priority/
```

使用 `/opsx:onboard` 可以生成项目概览，帮助新成员快速了解项目。

---

## 下一步

→ [第八章：团队协作](08-teamwork.md)

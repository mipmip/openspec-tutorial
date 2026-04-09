> [<< 第四章：完整工作流](04-workflow.md) | [目录](Home.md) | [第六章：文档编写规范 >>](06-documentation.md)

---

# 第五章：项目结构详解

## openspec/ 目录结构

OpenSpec 在项目根目录创建 `openspec/` 目录，所有规范相关文件都在这里面。

```
my-project/
├── openspec/                      # OpenSpec 根目录
│   ├── changes/                  # 正在进行的变更
│   │   └── add-dark-mode/       # 单个变更
│   │       ├── proposal.md      # 提案文档
│   │       ├── specs/           # 需求规范
│   │       │   └── requirements.md
│   │       ├── design.md        # 技术设计
│   │       └── tasks.md         # 实现任务
│   │
│   └── archive/                  # 已归档的变更
│       └── 2025-03-16-add-dark-mode/
│           ├── proposal.md
│           ├── specs/
│           ├── design.md
│           ├── tasks.md
│           └── summary.md       # 实现摘要
│
├── AGENTS.md                     # AI 助手配置
└── (你的项目文件)
```

---

## changes/ 目录

存放**正在进行**的变更。

### 命名规范

变更目录名使用 **kebab-case**（短横线连接）：

```
add-dark-mode              ✅
add_dark_mode              ❌
AddDarkMode                ❌
add dark mode              ❌
```

### 好的命名示例

```
add-user-login             # 添加用户登录
fix-mobile-layout          # 修复移动端布局
refactor-auth-module       # 重构认证模块
optimize-homepage-speed    # 优化首页速度
update-dependencies        # 更新依赖
```

### 目录结构

每个变更目录包含 4 个文档：

```
changes/add-feature/
├── proposal.md             # 提案：为什么要做
├── specs/                  # 规范：要做什么
│   └── requirements.md    # 需求文档
├── design.md               # 设计：怎么做
└── tasks.md                # 任务：具体步骤
```

---

## archive/ 目录

存放**已完成**的变更。

### 命名规范

归档目录名包含**日期前缀**：

```
2025-03-16-add-dark-mode
2025-03-15-fix-login-bug
2025-03-10-refactor-api
```

### 为什么要归档

1. **保持 changes/ 目录干净** - 只看当前进行中的工作
2. **保留历史记录** - 随时查阅过去的变更
3. **生成变更日志** - 自动生成 release notes
4. **团队知识沉淀** - 新成员了解项目演进

---

## 文档详解

### 1. proposal.md - 提案

**目的**：说明为什么要做这个变更

**内容**：
```markdown
# 提案：[变更标题]

## 背景
当前存在的问题或机会

## 目标
要达成的具体目标

## 范围
- 包含什么
- 不包含什么

## 成功标准
怎么算完成了
```

**示例**：
```markdown
# 提案：添加用户登录功能

## 背景
当前系统没有用户认证，任何人都可以访问敏感数据。

## 目标
- 实现安全的用户认证
- 支持手机号和微信登录
- 登录状态保持 7 天

## 范围
**包含**：
- 手机号 + 验证码登录
- 微信 OAuth 登录
- JWT Token 机制

**不包含**：
- 邮箱登录（后续迭代）
- 多因素认证（MFA）

## 成功标准
- 用户可以成功登录
- 未登录用户无法访问受保护页面
- Token 过期后自动跳转登录页
```

---

### 2. specs/ - 需求规范

**目的**：详细描述要做什么

**内容**：
```markdown
# 需求规范

## 功能需求
1. 需求 1
2. 需求 2

## 非功能需求
- 性能要求
- 安全要求

## 场景
### 场景 1：正常流程
1. 步骤 1
2. 步骤 2

### 场景 2：异常流程
...

## 界面原型（可选）
- 截图或链接
```

**示例**：
```markdown
# 需求规范

## 功能需求
1. 用户可以用手机号 + 验证码登录
2. 验证码 5 分钟内有效
3. 验证码错误 3 次后需等待 60 秒
4. 登录成功后返回 JWT Token
5. Token 有效期 7 天

## 非功能需求
- API 响应时间 < 200ms
- 支持 1000 并发用户
- 验证码发送频率限制（1 分钟 1 条）

## 场景

### 场景 1：正常登录
1. 用户输入手机号
2. 点击"获取验证码"
3. 收到短信验证码
4. 输入验证码
5. 点击"登录"
6. 登录成功，跳转到首页

### 场景 2：验证码过期
1. 用户输入已过期验证码
2. 点击"登录"
3. 系统提示"验证码已过期，请重新获取"
4. 用户重新获取验证码

### 场景 3：频繁发送
1. 用户 1 分钟内多次点击"获取验证码"
2. 系统提示"请稍后再试"
```

---

### 3. design.md - 技术设计

**目的**：说明技术方案

**内容**：
```markdown
# 技术设计

## 架构
- 技术栈
- 整体架构图

## API 设计
- 接口列表
- 请求/响应格式

## 数据模型
- 数据库表结构

## 关键实现
- 核心逻辑
- 算法说明
```

**示例**：
```markdown
# 技术设计

## 架构
- 前端：React + TypeScript
- 后端：Node.js + Express
- 数据库：PostgreSQL
- 缓存：Redis（存储验证码）

## API 设计

### POST /api/auth/send-code
发送验证码

**请求**：
```json
{
  "phone": "13800138000"
}
```

**响应**：
```json
{
  "success": true,
  "message": "验证码已发送"
}
```

### POST /api/auth/login
验证码登录

**请求**：
```json
{
  "phone": "13800138000",
  "code": "123456"
}
```

**响应**：
```json
{
  "success": true,
  "token": "eyJhbGciOiJIUzI1NiIs...",
  "expires_in": 604800
}
```

## 数据模型

### users 表
```sql
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  phone VARCHAR(20) UNIQUE NOT NULL,
  wechat_openid VARCHAR(50),
  created_at TIMESTAMP DEFAULT NOW(),
  last_login_at TIMESTAMP
);
```

### verification_codes 表
```sql
CREATE TABLE verification_codes (
  id SERIAL PRIMARY KEY,
  phone VARCHAR(20) NOT NULL,
  code VARCHAR(6) NOT NULL,
  expires_at TIMESTAMP NOT NULL,
  created_at TIMESTAMP DEFAULT NOW()
);
```

## 关键实现

### 验证码生成
- 6 位随机数字
- 存储到 Redis，TTL 300 秒

### Token 生成
- JWT 格式
- payload 包含 user_id、phone
- 签名使用 HS256
```

---

### 4. tasks.md - 实现任务

**目的**：具体的实现步骤清单

**内容**：
```markdown
# 实现任务

## 1. 后端 API
- [ ] 1.1 创建数据库表
- [ ] 1.2 实现发送验证码接口
- [ ] 1.3 实现登录接口

## 2. 前端页面
- [ ] 2.1 创建登录页面
- [ ] 2.2 实现验证码倒计时
```

**示例**：
```markdown
# 实现任务

## 1. 后端 API
- [ ] 1.1 创建数据库表
  - [ ] 1.1.1 创建 users 表
  - [ ] 1.1.2 创建 verification_codes 表
  - [ ] 1.1.3 添加索引

- [ ] 1.2 实现发送验证码接口
  - [ ] 1.2.1 创建 POST /api/auth/send-code
  - [ ] 1.2.2 集成短信服务商（阿里云）
  - [ ] 1.2.3 实现频率限制

- [ ] 1.3 实现登录接口
  - [ ] 1.3.1 创建 POST /api/auth/login
  - [ ] 1.3.2 验证验证码
  - [ ] 1.3.3 生成 JWT Token

- [ ] 1.4 编写单元测试
  - [ ] 1.4.1 测试发送验证码
  - [ ] 1.4.2 测试登录流程

## 2. 前端页面
- [ ] 2.1 创建登录页面
  - [ ] 2.1.1 创建 LoginPage 组件
  - [ ] 2.1.2 添加路由 /login

- [ ] 2.2 实现手机号输入
  - [ ] 2.2.1 添加手机号输入框
  - [ ] 2.2.2 添加格式验证

- [ ] 2.3 实现验证码功能
  - [ ] 2.3.1 添加验证码输入框
  - [ ] 2.3.2 实现倒计时按钮
  - [ ] 2.3.3 调用发送验证码 API

- [ ] 2.4 实现登录逻辑
  - [ ] 2.4.1 调用登录 API
  - [ ] 2.4.2 存储 Token
  - [ ] 2.4.3 登录成功后跳转
```

---

### 5. summary.md - 实现摘要（归档时生成）

**目的**：记录实际实现情况

**内容**：
```markdown
# 实现摘要

## 完成时间
2025-03-16

## 实现内容
- ✅ 功能 1
- ✅ 功能 2
- ⚠️ 功能 3（部分实现）

## 新增文件
- 文件 1
- 文件 2

## 修改文件
- 文件 3
- 文件 4

## 备注
实现过程中的重要说明
```

---

## AGENTS.md

项目根目录的 `AGENTS.md` 是给 AI 助手看的配置文件。

**内容示例**：
```markdown
# OpenSpec Agent Configuration

## 项目信息
- 名称：My Project
- 技术栈：React + TypeScript + Node.js
- 数据库：PostgreSQL

## 命令
- `/opsx:propose <description>` - 创建新变更提案
- `/opsx:apply` - 应用实现
- `/opsx:archive` - 归档变更

## 规范
- 代码风格：使用 ESLint + Prettier
- 测试：所有功能必须有单元测试
- 文档：API 变更需更新 README

## 注意事项
- 不要修改数据库 schema 没有 migration
- 敏感信息使用环境变量
```

---

> [<< 第四章：完整工作流](04-workflow.md) | [目录](Home.md) | [第六章：文档编写规范 >>](06-documentation.md)
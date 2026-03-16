# 第九章：高级技巧

## 1. 上下文管理

### 保持上下文干净

OpenSpec 建议在开始新功能前清理 AI 的上下文：

```
# 开始新功能前
1. 完成并归档上一个功能
2. 清理 AI 对话上下文（新建对话）
3. 让 AI 读取 AGENTS.md 和当前 changes/
4. 执行 /opsx:propose
```

**为什么重要**：
- 旧对话中的信息可能干扰新功能
- 清晰的上下文 = 更准确的实现
- 减少 AI 的"幻觉"

### 提供足够的上下文

```bash
# 在 propose 时提供背景
/opsx:propose "添加文件上传功能，
  背景：用户需要上传头像和附件，
  技术约束：使用阿里云 OSS，
  文件大小限制：图片 5MB，文档 20MB"
```

---

## 2. 迭代式规范

### 不要一次写完所有规范

```bash
# 第一次：基础功能
/opsx:propose "添加文件上传功能（基础版）"
# 实现后...

# 第二次：增强功能
/opsx:propose "为文件上传添加进度条和断点续传"
# 实现后...

# 第三次：优化
/opsx:propose "优化文件上传性能，支持并发上传"
```

**好处**：
- 每次变更范围小，风险低
- 可以根据实际情况调整方向
- 更容易 Code Review

---

## 3. 规范复用

### 创建规范模板

对于常见功能，创建模板：

```
openspec/
└── templates/
    ├── crud-feature.md      # CRUD 功能模板
    ├── auth-feature.md      # 认证功能模板
    └── notification.md      # 通知功能模板
```

**crud-feature.md 模板**：
```markdown
# 提案：[功能名称] CRUD

## 背景
[描述背景]

## 目标
实现 [实体名] 的完整 CRUD 操作

## 范围
**包含**：
- 创建 [实体名]
- 查看 [实体名] 列表
- 查看 [实体名] 详情
- 更新 [实体名]
- 删除 [实体名]

## 数据结构
[实体名] 包含：
- id: 唯一标识
- [字段 1]: [说明]
- [字段 2]: [说明]
- created_at: 创建时间
- updated_at: 更新时间
```

---

## 4. 与 CI/CD 集成

### 在 CI 中验证规范

```yaml
# .github/workflows/openspec-check.yml
name: OpenSpec Check

on: [pull_request]

jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Check OpenSpec structure
        run: |
          # 检查 changes/ 目录下的变更是否有完整文档
          for dir in openspec/changes/*/; do
            if [ -d "$dir" ]; then
              name=$(basename "$dir")
              
              # 检查必需文件
              for file in proposal.md design.md tasks.md; do
                if [ ! -f "$dir$file" ]; then
                  echo "❌ $name 缺少 $file"
                  exit 1
                fi
              done
              
              echo "✅ $name 文档完整"
            fi
          done
```

---

## 5. 多变更并行

### 同时进行多个变更

```
openspec/changes/
├── add-file-upload/      # 张三负责
├── add-notifications/    # 李四负责
└── fix-performance/      # 王五负责
```

**注意事项**：
- 确保变更之间没有冲突
- 在 tasks.md 中标注负责人
- 定期同步进度

---

## 6. 处理规范变更

### 实现过程中需要修改规范

有时候实现过程中会发现规范需要调整：

```bash
# 告诉 AI
"在实现 1.3 时发现，design.md 中的方案有问题，
 因为 PostgreSQL 不支持这个操作，
 需要改用另一种方案"

# AI 会
# 1. 更新 design.md
# 2. 可能更新 tasks.md
# 3. 继续实现
```

**原则**：
- 规范是活的文档，可以修改
- 修改规范要记录原因
- 重大变更需要重新评审

---

## 7. 使用 /opsx:ff 快进

### 什么是快进

`/opsx:ff` 是"fast-forward"的缩写，让 AI 自动完成所有任务，不需要逐步确认：

```bash
# 普通模式：每个任务完成后等待确认
/opsx:apply

# 快进模式：自动完成所有任务
/opsx:ff
```

### 什么时候用快进

- 规范已经非常清晰
- 任务相对简单
- 你信任 AI 的实现能力

### 什么时候不用快进

- 规范还不够清晰
- 任务涉及复杂逻辑
- 需要在实现过程中做决策

---

## 8. 规范驱动的 Code Review

### 在 PR 描述中引用规范

```markdown
## PR 描述

**功能**：添加文件上传功能
**规范**：openspec/changes/add-file-upload/

### 实现内容
按照 tasks.md 完成了以下任务：
- ✅ 1.1 配置阿里云 OSS
- ✅ 1.2 实现文件上传接口
- ✅ 1.3 实现前端上传组件
- ✅ 1.4 添加文件大小限制

### 与规范的差异
- design.md 中计划支持 WebP 格式，
  但阿里云 OSS 当前版本不支持自动转换，
  已更新 design.md 记录此限制

### 测试
- 单元测试：覆盖率 85%
- 手动测试：已在 Chrome/Safari/Firefox 测试
```

---

## 下一步

→ [第十章：常见问题](10-faq.md)

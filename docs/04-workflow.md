# 第四章：完整工作流演示

本章通过一个完整的案例，演示 OpenSpec 的实际使用流程。

## 案例：添加暗黑模式

假设我们要为一个 React 项目添加暗黑模式功能。

---

## 第一步：创建提案

```bash
/opsx:propose "添加暗黑模式，用户可以在亮色和暗色主题之间切换"
```

AI 分析后，创建目录结构：

```
openspec/changes/add-dark-mode/
├── proposal.md
├── specs/
│   └── requirements.md
├── design.md
└── tasks.md
```

### proposal.md 内容示例

```markdown
# 提案：添加暗黑模式

## 背景
当前应用只有亮色主题，在夜间或弱光环境下使用体验不佳。

## 目标
- 支持亮色/暗色主题切换
- 自动跟随系统主题偏好
- 用户偏好持久化

## 成功标准
- 用户可以通过开关切换主题
- 切换后所有组件正确显示
- 刷新页面后保持用户选择
```

### specs/requirements.md 内容示例

```markdown
# 需求规范

## 功能需求
1. 添加主题切换开关（位于导航栏右侧）
2. 支持三种模式：
   - Light（亮色）
   - Dark（暗色）
   - System（跟随系统）
3. 用户选择保存到 localStorage
4. 默认值为 System

## 视觉需求
- 暗色模式背景色：#1a1a2e
- 暗色模式文字色：#eaeaea
- 所有组件需要适配两种主题

## 场景
### 场景 1：首次访问
用户首次访问，检测系统主题，自动应用

### 场景 2：手动切换
用户点击切换按钮，主题立即改变

### 场景 3：重新访问
用户再次访问，恢复上次选择的主题
```

### design.md 内容示例

```markdown
# 技术设计

## 方案
使用 React Context + CSS Variables 实现

## 文件结构
```
src/
├── components/
│   └── ThemeToggle.tsx      # 主题切换按钮
├── contexts/
│   └── ThemeContext.tsx     # 主题上下文
├── hooks/
│   └── useTheme.ts          # 主题 Hook
└── styles/
    └── theme.css            # 主题变量
```

## 实现细节
1. ThemeContext 管理当前主题状态
2. CSS Variables 定义颜色变量
3. localStorage 持久化用户选择
4. matchMedia 监听系统主题变化

## 颜色变量
```css
:root {
  --bg-primary: #ffffff;
  --text-primary: #1a1a2e;
}

[data-theme="dark"] {
  --bg-primary: #1a1a2e;
  --text-primary: #eaeaea;
}
```
```

### tasks.md 内容示例

```markdown
# 实现任务

## 1. 创建主题系统
- [ ] 1.1 创建 ThemeContext.tsx
- [ ] 1.2 创建 useTheme hook
- [ ] 1.3 创建 theme.css 变量文件

## 2. 创建切换组件
- [ ] 2.1 创建 ThemeToggle 组件
- [ ] 2.2 添加太阳/月亮图标
- [ ] 2.3 实现点击切换逻辑

## 3. 集成到应用
- [ ] 3.1 在 App.tsx 包裹 ThemeProvider
- [ ] 3.2 在导航栏添加 ThemeToggle
- [ ] 3.3 更新现有组件使用 CSS 变量

## 4. 持久化和系统适配
- [ ] 4.1 实现 localStorage 存储
- [ ] 4.2 实现系统主题监听
- [ ] 4.3 处理首次访问的默认主题
```

---

## 第二步：审查规范

**这是最关键的一步！**

仔细阅读 AI 生成的文档：

```bash
# 查看 proposal
cat openspec/changes/add-dark-mode/proposal.md

# 查看需求
cat openspec/changes/add-dark-mode/specs/requirements.md

# 查看设计
cat openspec/changes/add-dark-mode/design.md

# 查看任务
cat openspec/changes/add-dark-mode/tasks.md
```

### 确认无误

如果文档符合预期，告诉 AI：
```
规范文档看起来不错，可以开始实现了
```

### 需要修改

如果有问题，直接指出：
```
design.md 里的颜色方案我们改用 Tailwind 的 dark: 前缀，不用 CSS Variables
tasks.md 里需要添加单元测试任务
```

AI 会更新文档，直到你满意为止。

---

## 第三步：应用实现

确认规范后，开始实现：

```bash
/opsx:apply
```

### 实现过程

AI 会按 tasks.md 的顺序执行：

```
开始实现暗黑模式功能...

✓ 1.1 创建 ThemeContext.tsx
  创建了主题上下文，包含 theme、setTheme、toggleTheme

✓ 1.2 创建 useTheme hook
  封装了主题状态获取和切换方法

✓ 1.3 创建 theme.css 变量文件
  定义了亮色和暗色模式的颜色变量

✓ 2.1 创建 ThemeToggle 组件
  创建了带有太阳/月亮图标的切换按钮

✓ 2.2 添加太阳/月亮图标
  使用 lucide-react 图标库

✓ 2.3 实现点击切换逻辑
  点击时切换主题并保存到 localStorage

✓ 3.1 在 App.tsx 包裹 ThemeProvider
  在应用根节点添加了主题提供者

✓ 3.2 在导航栏添加 ThemeToggle
  在 Header 组件右侧添加了切换按钮

✓ 3.3 更新现有组件使用 CSS 变量
  更新了 Button、Card、Input 等组件

✓ 4.1 实现 localStorage 存储
  主题选择持久化到 localStorage

✓ 4.2 实现系统主题监听
  使用 matchMedia 监听系统主题变化

✓ 4.3 处理首次访问的默认主题
  首次访问时检测系统偏好

🎉 所有任务完成！
```

---

## 第四步：验证功能

实现完成后，手动测试：

```bash
# 启动应用
npm run dev

# 测试以下场景：
# 1. 点击切换按钮，主题是否正确切换
# 2. 刷新页面，主题是否保持
# 3. 切换系统主题，应用是否跟随
```

### 验证命令

```bash
/opsx:verify
```

AI 会对比 specs/ 和代码，报告是否符合规范。

---

## 第五步：归档变更

功能测试通过后，归档记录：

```bash
/opsx:archive
```

变更被移动到：
```
openspec/changes/add-dark-mode/
→ openspec/archive/2025-03-16-add-dark-mode/
```

目录结构：
```
openspec/archive/2025-03-16-add-dark-mode/
├── proposal.md
├── specs/
├── design.md
├── tasks.md
└── summary.md          # 新增：实现摘要
```

### summary.md 内容示例

```markdown
# 实现摘要

## 完成时间
2025-03-16

## 实现内容
- ✅ ThemeContext 和 useTheme hook
- ✅ ThemeToggle 组件（太阳/月亮图标）
- ✅ CSS 变量主题系统
- ✅ localStorage 持久化
- ✅ 系统主题自动适配

## 新增文件
- src/contexts/ThemeContext.tsx
- src/hooks/useTheme.ts
- src/components/ThemeToggle.tsx
- src/styles/theme.css

## 修改文件
- src/App.tsx
- src/components/Header.tsx
- src/components/Button.tsx
- src/components/Card.tsx
- src/components/Input.tsx

## 备注
实现过程顺利，无需调整规范
```

---

## 完整流程图

```
开始
  │
  ▼
/opsx:propose "描述你的需求"
  │
  ▼
AI 创建规范文档
  │
  ▼
你审查规范文档
  │
  ├─ 不满意 ──→ 告诉 AI 修改 ──→ 回到审查
  │
  └─ 满意 ─────→ /opsx:apply
                  │
                  ▼
              AI 实现代码
                  │
                  ▼
              你测试验证
                  │
              ├─ 有问题 ──→ 告诉 AI 修复 ──→ 继续测试
              │
              └─ 没问题 ──→ /opsx:archive
                              │
                              ▼
                          归档完成
                              │
                              ▼
                          开始下一个功能
```

---

## 下一步

→ [第五章：项目结构详解](05-structure.md)

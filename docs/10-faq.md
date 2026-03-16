# 第十章：常见问题

## 安装问题

### Q: 安装 OpenSpec 时提示权限错误

**A**: 使用管理员权限或 npx

```bash
# macOS/Linux
sudo npm install -g @fission-ai/openspec@latest

# Windows（管理员 PowerShell）
npm install -g @fission-ai/openspec@latest

# 或使用 npx（无需安装）
npx @fission-ai/openspec@latest
```

### Q: 安装后命令找不到

**A**: 检查 npm 全局路径

```bash
# 查看 npm 全局安装路径
npm config get prefix

# 确保路径在 PATH 中
# macOS/Linux
export PATH="$PATH:$(npm config get prefix)/bin"

# Windows
# 将路径添加到系统环境变量 PATH
```

### Q: Node.js 版本不够

**A**: 升级 Node.js 到 20.19.0+

```bash
# 使用 nvm
nvm install 20
nvm use 20

# 或使用 fnm
fnm install 20
fnm use 20

# 或从官网下载
# https://nodejs.org/
```

---

## 使用问题

### Q: /opsx:propose 生成的规范不符合预期

**A**: 提供更详细的描述

```bash
# ❌ 太模糊
/opsx:propose "改进登录"

# ✅ 更详细
/opsx:propose "添加手机号验证码登录功能，
  支持发送 6 位数字验证码，
  验证码 5 分钟有效，
  错误 3 次后锁定 60 秒"
```

### Q: AI 实现过程中偏离了规范

**A**: 及时纠正并更新规范

```
"停一下，design.md 中说的是用 JWT，
 不是 Session，请按规范实现"

或

"我更新了 design.md，改用 Session 方案，
 请按新的规范实现"
```

### Q: 实现过程中发现规范有问题

**A**: 这是正常的，规范可以修改

```
"在实现 1.3 时发现，design.md 中的方案有问题，
 因为 [原因]，需要改为 [新方案]"

AI 会更新规范文档，然后继续实现
```

### Q: /opsx:apply 执行到一半中断了

**A**: 使用 /opsx:continue 继续

```bash
/opsx:continue
```

AI 会从上次中断的地方继续。

### Q: 如何查看当前有哪些变更在进行

**A**: 查看 changes/ 目录

```bash
ls openspec/changes/
```

### Q: 如何查看已完成的功能

**A**: 查看 archive/ 目录

```bash
ls openspec/archive/
```

---

## 规范文档问题

### Q: proposal.md 应该写多详细

**A**: 足够让团队成员理解就行

```markdown
# 好的 proposal

## 背景
当前系统没有用户认证，任何人都可以访问敏感数据。

## 目标
实现手机号 + 验证码登录

## 范围
包含：手机号登录
不包含：邮箱登录、微信登录
```

不需要写技术细节，那是 design.md 的内容。

### Q: specs/ 里要写多少场景

**A**: 覆盖正常和主要异常场景

```markdown
## 场景

### 正常场景
用户输入正确验证码，登录成功

### 异常场景
1. 验证码错误
2. 验证码过期
3. 网络错误

### 边界场景
1. 频繁发送验证码
2. 手机号格式错误
```

### Q: design.md 里的技术方案必须严格执行吗

**A**: 不是，可以调整

- 实现前：可以修改 design.md
- 实现中：发现问题可以调整方案
- 实现后：在 summary.md 记录差异

### Q: tasks.md 的任务粒度应该多细

**A**: 独立可完成的最小单元

```markdown
# ❌ 太大
- [ ] 实现登录功能

# ✅ 合适
- [ ] 1.1 创建 users 数据库表
- [ ] 1.2 实现发送验证码接口
- [ ] 1.3 实现验证码登录接口
```

---

## 团队协作问题

### Q: 多人同时修改同一个变更

**A**: 避免这种情况

- 每个变更由一个人负责
- 通过 Git 分支管理
- 在 tasks.md 中标注负责人

### Q: 如何同步规范给团队成员

**A**: 通过 Git

```bash
git add openspec/changes/add-feature/
git commit -m "Add spec for user login feature"
git push
```

团队成员 pull 后就能看到最新规范。

### Q: 新成员如何快速了解项目

**A**: 使用 /opsx:onboard

```bash
/opsx:onboard
```

AI 会生成项目概览文档。

---

## 性能问题

### Q: AI 实现速度很慢

**A**: 可能的原因

1. **上下文太大** - 清理 AI 上下文，新建对话
2. **任务太多** - 拆分成多个小的变更
3. **模型选择** - 使用更强的模型（Opus 4.5、GPT 5.2）

### Q: 如何加快开发速度

**A**: 使用 /opsx:ff

```bash
# 快进模式，自动完成所有任务
/opsx:ff
```

适合规范清晰、任务简单的情况。

---

## 其他问题

### Q: OpenSpec 支持哪些 AI 工具

**A**: 支持 20+ 种

- Claude Code
- Codex (OpenAI)
- Pi
- Kiro
- Cursor
- GitHub Copilot
- ...

### Q: 如何更新 OpenSpec

**A**: 

```bash
npm install -g @fission-ai/openspec@latest
openspec update
```

### Q: 如何卸载 OpenSpec

**A**:

```bash
npm uninstall -g @fission-ai/openspec
```

### Q: OpenSpec 是免费的吗

**A**: 是的，MIT 开源协议

---

## 获取帮助

### 官方资源

- GitHub: https://github.com/Fission-AI/OpenSpec
- 文档: https://openspec.dev/
- Discord: https://discord.gg/YctCnvvshC

### 报告问题

在 GitHub Issues 报告：
https://github.com/Fission-AI/OpenSpec/issues

---

**恭喜！你已经学完了 OpenSpec 完整教程。**

现在可以开始在你的项目中使用 OpenSpec 了！

```bash
# 开始你的第一个变更
/opsx:propose "添加一个新功能"
```

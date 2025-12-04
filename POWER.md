---
name: "versioning"
displayName: "版本控制助手"
description: "自动配置 standard-version，生成 CHANGELOG，规范化提交消息"
keywords:
  - "版本"
  - "version"
  - "changelog"
  - "发布"
  - "release"
  - "提交规范"
  - "conventional"
  - "semver"
---

# 版本控制助手

帮助你快速配置项目的版本控制、CHANGELOG 自动生成和提交规范。

## 何时激活

当用户讨论以下内容时激活此 Power：

- 版本号管理
- CHANGELOG 生成
- 发布流程
- 提交消息规范
- Conventional Commits

## Onboarding

### 步骤 1：检查环境

确保已安装 Node.js 和 pnpm：

```bash
node --version   # 需要 Node.js 14+
pnpm --version   # 需要 pnpm
```

如果未安装 pnpm：

```bash
npm install -g pnpm
```

### 步骤 2：安装依赖

```bash
pnpm add -D standard-version
```

### 步骤 3：创建 Hook（可选）

添加发布提醒 Hook 到 `.kiro/hooks/release-reminder.kiro.hook`：

```json
{
  "enabled": true,
  "name": "发布提醒",
  "description": "提醒检查是否需要发布新版本",
  "version": "1",
  "when": { "type": "userTriggered" },
  "then": {
    "type": "askAgent",
    "prompt": "检查最近的提交，判断是否需要发布新版本，并建议版本号类型（patch/minor/major）"
  }
}
```

## Steering 文件映射

根据任务类型加载不同的指导文件：

| 任务场景 | Steering 文件                       | 说明                     |
| -------- | ----------------------------------- | ------------------------ |
| 提交代码 | `steering/commit-convention.md`     | Conventional Commits 规范 |
| 发布版本 | `steering/version-release.md`       | 版本发布工作流           |
| 配置项目 | `steering/changelog-config.md`      | CHANGELOG 配置模板       |

## 快速命令

```bash
# 发布新版本（自动判断）
pnpm release

# 指定版本类型
pnpm release:patch   # 0.1.0 → 0.1.1
pnpm release:minor   # 0.1.0 → 0.2.0
pnpm release:major   # 0.1.0 → 1.0.0

# 首次发布
pnpm release:first

# 预览（不实际发布）
pnpm release -- --dry-run
```

## 常见问题

### Q: 如何修改上一次提交消息？

```bash
git commit --amend -m "fix(auth): 正确的提交消息"
```

### Q: 如何跳过 CI 检查发布？

```bash
pnpm release -- --no-verify
```

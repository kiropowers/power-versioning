# 版本发布工作流

本文件指导如何正确发布新版本。

## 版本号规则（SemVer）

```
MAJOR.MINOR.PATCH
  │     │     │
  │     │     └── Bug 修复、小改动
  │     └──────── 新功能（向后兼容）
  └────────────── 破坏性变更
```

### 版本号自动计算

| 提交类型        | 版本变化 | 示例            |
| --------------- | -------- | --------------- |
| fix             | PATCH    | 0.1.0 → 0.1.1   |
| feat            | MINOR    | 0.1.0 → 0.2.0   |
| BREAKING CHANGE | MAJOR    | 0.1.0 → 1.0.0   |

## 发布命令

### 自动发布（推荐）

```bash
# 根据提交类型自动判断版本号
pnpm release
```

### 指定版本类型

```bash
# Bug 修复版本
pnpm release:patch   # 0.1.0 → 0.1.1

# 新功能版本
pnpm release:minor   # 0.1.0 → 0.2.0

# 大版本更新
pnpm release:major   # 0.1.0 → 1.0.0
```

### 首次发布

```bash
pnpm release:first
```

### 预览模式

```bash
# 查看将要发生的变更，不实际执行
pnpm release -- --dry-run
```

## 发布前检查清单

### 代码质量

- [ ] 所有测试通过
- [ ] 代码已审查
- [ ] 无 lint 错误
- [ ] 无 TypeScript 错误

### 文档

- [ ] README 已更新
- [ ] API 文档已更新
- [ ] 迁移指南已编写（如有破坏性变更）

### 版本

- [ ] 版本号正确
- [ ] CHANGELOG 预览正确
- [ ] 依赖版本已锁定

## 发布流程

### 标准流程

```bash
# 1. 确保在主分支
git checkout main
git pull origin main

# 2. 运行测试
pnpm test

# 3. 预览发布
pnpm release -- --dry-run

# 4. 确认无误后发布
pnpm release

# 5. 推送到远程
git push --follow-tags origin main
```

### standard-version 自动执行的操作

1. 分析提交历史
2. 计算新版本号
3. 更新 package.json 版本
4. 生成/更新 CHANGELOG.md
5. 创建 Git commit
6. 创建 Git tag

## 特殊场景

### 预发布版本

```bash
# Alpha 版本
pnpm release -- --prerelease alpha
# 0.1.0 → 0.1.1-alpha.0

# Beta 版本
pnpm release -- --prerelease beta
# 0.1.0 → 0.1.1-beta.0

# RC 版本
pnpm release -- --prerelease rc
# 0.1.0 → 0.1.1-rc.0
```

### 指定版本号

```bash
# 直接指定版本
pnpm release -- --release-as 1.0.0
```

### 跳过某些步骤

```bash
# 跳过 CHANGELOG 生成
pnpm release -- --skip.changelog

# 跳过 Git commit
pnpm release -- --skip.commit

# 跳过 Git tag
pnpm release -- --skip.tag
```

## 发布后操作

### 推送代码和标签

```bash
git push --follow-tags origin main
```

### 发布到 npm（如果是 npm 包）

```bash
pnpm publish
```

### 创建 GitHub Release（可选）

1. 打开 GitHub 仓库
2. 点击 "Releases"
3. 点击 "Draft a new release"
4. 选择刚创建的 tag
5. 复制 CHANGELOG 内容
6. 发布

## 回滚版本

如果发布出错，需要回滚：

```bash
# 删除本地 tag
git tag -d v0.2.0

# 删除远程 tag（如果已推送）
git push origin :refs/tags/v0.2.0

# 回退 commit
git reset --hard HEAD~1

# 强制推送（谨慎使用）
git push -f origin main
```

## 常见问题

### Q: 发布后发现 CHANGELOG 有误？

```bash
# 修改 CHANGELOG.md
# 然后修正 commit
git add CHANGELOG.md
git commit --amend --no-edit
git tag -f v0.2.0
git push -f --tags
```

### Q: 如何跳过 CI 检查？

```bash
pnpm release -- --no-verify
```

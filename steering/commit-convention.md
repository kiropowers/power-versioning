# Conventional Commits 提交规范

本文件指导如何编写规范的 Git 提交消息。

## 提交格式

```
<type>(<scope>): <subject>

<body>

<footer>
```

### 必填部分

- `type`: 提交类型（必填）
- `subject`: 简短描述（必填，50 字符以内）

### 可选部分

- `scope`: 影响范围
- `body`: 详细描述
- `footer`: 关联 Issue 或破坏性变更说明

## Type 类型说明

| Type     | 说明     | 版本影响 | 示例                          |
| -------- | -------- | -------- | ----------------------------- |
| feat     | 新功能   | MINOR    | `feat(user): 添加登录功能`    |
| fix      | Bug 修复 | PATCH    | `fix(auth): 修复 token 过期`  |
| docs     | 文档更新 | 无       | `docs(readme): 更新安装说明`  |
| style    | 代码格式 | 无       | `style: 格式化代码`           |
| refactor | 重构     | 无       | `refactor(api): 重构用户接口` |
| perf     | 性能优化 | PATCH    | `perf(db): 优化查询性能`      |
| test     | 测试     | 无       | `test(user): 添加单元测试`    |
| chore    | 构建/工具 | 无       | `chore: 更新依赖`             |
| ci       | CI 配置  | 无       | `ci: 添加 GitHub Actions`     |

## 提交示例

### 基础提交

```bash
# 新功能
git commit -m "feat(user): 添加用户注册功能"

# Bug 修复
git commit -m "fix(auth): 修复登录状态丢失问题"

# 文档更新
git commit -m "docs(api): 添加接口文档"
```

### 带 Body 的提交

```bash
git commit -m "feat(order): 添加订单导出功能

支持导出为 CSV 和 Excel 格式
添加导出进度显示"
```

### 破坏性变更

```bash
git commit -m "feat(api)!: 重构用户接口

BREAKING CHANGE: 用户接口路径从 /user 改为 /users
旧接口将在 v2.0 移除"
```

### 关联 Issue

```bash
git commit -m "fix(login): 修复密码验证问题

Closes #123
Refs #456"
```

## 最佳实践

### ✅ 好的提交

```bash
feat(user): 添加用户头像上传功能
fix(cart): 修复购物车数量计算错误
docs(readme): 添加快速开始指南
refactor(auth): 提取认证逻辑到独立模块
```

### ❌ 避免的提交

```bash
# 太模糊
update code
fix bug
add feature

# 太长
feat: 添加了用户登录功能并且修复了一些bug还有更新了文档

# 混合多个变更
feat: 添加登录功能，修复注册bug，更新文档
```

## 提交粒度原则

1. **一个提交只做一件事**
2. **提交后代码应该可运行**
3. **相关变更放在一起**
4. **关联 Issue 编号**

## Scope 建议

根据项目结构定义 scope：

```
# 按模块
user, auth, order, payment, cart

# 按层级
api, service, model, view, util

# 按功能
login, register, checkout, search
```

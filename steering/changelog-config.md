# CHANGELOG 配置指南

本文件指导如何配置 CHANGELOG 生成。

## 基础配置

### package.json 脚本

```json
{
  "name": "your-project",
  "version": "0.1.0",
  "scripts": {
    "release": "standard-version",
    "release:minor": "standard-version --release-as minor",
    "release:major": "standard-version --release-as major",
    "release:patch": "standard-version --release-as patch",
    "release:first": "standard-version --first-release"
  },
  "devDependencies": {
    "standard-version": "^9.5.0"
  }
}
```

## 自定义配置

### .versionrc.json

创建 `.versionrc.json` 自定义 CHANGELOG 格式：

```json
{
  "types": [
    {"type": "feat", "section": "✨ 新功能"},
    {"type": "fix", "section": "🐛 Bug 修复"},
    {"type": "docs", "section": "📚 文档"},
    {"type": "style", "section": "💄 样式"},
    {"type": "refactor", "section": "♻️ 重构"},
    {"type": "perf", "section": "⚡ 性能优化"},
    {"type": "test", "section": "✅ 测试"},
    {"type": "chore", "section": "🔧 其他"},
    {"type": "ci", "section": "👷 CI/CD"}
  ]
}
```

### 完整配置示例

```json
{
  "types": [
    {"type": "feat", "section": "✨ 新功能"},
    {"type": "fix", "section": "🐛 Bug 修复"},
    {"type": "docs", "section": "📚 文档"},
    {"type": "style", "section": "💄 样式", "hidden": true},
    {"type": "refactor", "section": "♻️ 重构"},
    {"type": "perf", "section": "⚡ 性能优化"},
    {"type": "test", "section": "✅ 测试", "hidden": true},
    {"type": "chore", "section": "🔧 其他", "hidden": true},
    {"type": "ci", "section": "👷 CI/CD", "hidden": true}
  ],
  "commitUrlFormat": "{{host}}/{{owner}}/{{repository}}/commit/{{hash}}",
  "compareUrlFormat": "{{host}}/{{owner}}/{{repository}}/compare/{{previousTag}}...{{currentTag}}",
  "issueUrlFormat": "{{host}}/{{owner}}/{{repository}}/issues/{{id}}",
  "userUrlFormat": "{{host}}/{{user}}",
  "releaseCommitMessageFormat": "chore(release): {{currentTag}}",
  "header": "# 更新日志\n\n本文件记录项目的所有重要变更。\n"
}
```

## 配置选项说明

### types - 提交类型配置

| 属性    | 说明                              |
| ------- | --------------------------------- |
| type    | 提交类型（对应 commit 的 type）   |
| section | CHANGELOG 中的分组标题            |
| hidden  | 是否隐藏此类型（不显示在 CHANGELOG） |

### URL 模板

| 模板             | 说明             |
| ---------------- | ---------------- |
| commitUrlFormat  | 提交链接格式     |
| compareUrlFormat | 版本对比链接格式 |
| issueUrlFormat   | Issue 链接格式   |
| userUrlFormat    | 用户链接格式     |

### 可用变量

- `{{host}}` - Git 仓库主机
- `{{owner}}` - 仓库所有者
- `{{repository}}` - 仓库名称
- `{{hash}}` - 提交哈希
- `{{previousTag}}` - 上一个版本标签
- `{{currentTag}}` - 当前版本标签
- `{{id}}` - Issue ID
- `{{user}}` - 用户名

## 生成的 CHANGELOG 示例

```markdown
# 更新日志

本文件记录项目的所有重要变更。

## [0.2.0](https://github.com/user/repo/compare/v0.1.0...v0.2.0) (2024-01-15)

### ✨ 新功能

* **user:** 添加用户登录功能 ([abc1234](https://github.com/user/repo/commit/abc1234))
* **order:** 添加订单导出功能 ([def5678](https://github.com/user/repo/commit/def5678))

### 🐛 Bug 修复

* **auth:** 修复 token 过期问题 ([ghi9012](https://github.com/user/repo/commit/ghi9012))

### 📚 文档

* **readme:** 更新安装说明 ([jkl3456](https://github.com/user/repo/commit/jkl3456))

## [0.1.0](https://github.com/user/repo/releases/tag/v0.1.0) (2024-01-01)

### ✨ 新功能

* 项目初始化
```

## 初始 CHANGELOG 模板

首次使用前，创建 `CHANGELOG.md`：

```markdown
# 更新日志

本文件记录项目的所有重要变更。

格式基于 [Keep a Changelog](https://keepachangelog.com/zh-CN/)，
版本号遵循 [语义化版本](https://semver.org/lang/zh-CN/)。
```

## 高级配置

### 跳过某些提交

在 `.versionrc.json` 中配置：

```json
{
  "skip": {
    "bump": false,
    "changelog": false,
    "commit": false,
    "tag": false
  }
}
```

### 自定义 Tag 前缀

```json
{
  "tagPrefix": "v"
}
```

### 指定 CHANGELOG 文件

```json
{
  "infile": "CHANGELOG.md"
}
```

## 可选：添加提交检查

### 安装 commitlint

```bash
pnpm add -D @commitlint/cli @commitlint/config-conventional
```

### commitlint.config.js

```javascript
module.exports = {
  extends: ['@commitlint/config-conventional'],
  rules: {
    'type-enum': [2, 'always', [
      'feat', 'fix', 'docs', 'style', 'refactor',
      'perf', 'test', 'chore', 'ci', 'revert'
    ]],
    'subject-case': [0],  // 允许中文
    'subject-max-length': [2, 'always', 100]
  }
};
```

### 配置 husky（Git hooks）

```bash
pnpm add -D husky
npx husky install
npx husky add .husky/commit-msg 'npx --no -- commitlint --edit "$1"'
```

## 快速配置命令

一键配置（复制到终端执行）：

```bash
# 安装依赖
pnpm add -D standard-version

# 创建配置文件
cat > .versionrc.json << 'EOF'
{
  "types": [
    {"type": "feat", "section": "✨ 新功能"},
    {"type": "fix", "section": "🐛 Bug 修复"},
    {"type": "docs", "section": "📚 文档"},
    {"type": "refactor", "section": "♻️ 重构"},
    {"type": "perf", "section": "⚡ 性能优化"}
  ]
}
EOF

# 创建初始 CHANGELOG
cat > CHANGELOG.md << 'EOF'
# 更新日志

本文件记录项目的所有重要变更。
EOF

echo "配置完成！运行 pnpm release:first 发布首个版本"
```

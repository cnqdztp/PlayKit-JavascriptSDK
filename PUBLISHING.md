# 发布到 npm 指南

## 📋 发布前检查清单

### 1. 登录 npm 账号

如果还没有 npm 账号，先注册：
```bash
# 访问 https://www.npmjs.com/signup 注册账号
```

登录 npm：
```bash
npm login
# 或者使用
npm adduser
```

输入你的：
- Username (用户名)
- Password (密码)
- Email (邮箱)
- One-time password (如果启用了 2FA 双因素认证)

验证登录状态：
```bash
npm whoami
```

### 2. 检查包名是否可用

```bash
npm view playkit-sdk
```

如果显示 "npm ERR! 404"，说明包名可用。
如果显示包信息，说明包名已被占用，需要更改 package.json 中的 name。

**建议的备选包名：**
- `@your-username/playkit-sdk` (使用 scoped package)
- `playkit-ai-sdk`
- `playkit-game-sdk`
- `playkit-js-sdk`

### 3. 更新版本号

当前版本：`1.0.0-beta.1`

根据语义化版本规范 (Semver)：
- **补丁版本** (1.0.1): 修复 bug
- **次版本** (1.1.0): 添加新功能（向后兼容）
- **主版本** (2.0.0): 破坏性更改

更新版本号：
```bash
# 自动更新补丁版本 (1.0.0-beta.1 -> 1.0.0-beta.2)
npm version prepatch --preid=beta

# 或者发布正式版本
npm version 1.0.0
```

### 4. 构建项目

```bash
npm run build
```

检查 dist 目录是否生成了所有文件：
- `dist/index.cjs.js` (CommonJS)
- `dist/index.esm.js` (ES Module)
- `dist/index.umd.js` (UMD for browser)
- `dist/index.d.ts` (TypeScript types)

### 5. 测试本地包

在发布前，可以在本地测试：

```bash
# 在当前项目目录
npm pack

# 这会生成一个 .tgz 文件，例如：playkit-sdk-1.0.0-beta.1.tgz
# 在另一个测试项目中安装：
npm install /path/to/playkit-sdk-1.0.0-beta.1.tgz
```

### 6. 发布到 npm

#### Beta 版本发布（推荐首次发布）

```bash
npm publish --tag beta
```

这样用户需要显式安装 beta 版本：
```bash
npm install playkit-sdk@beta
```

#### 正式版本发布

```bash
npm publish
```

#### 发布 scoped package（如果使用 @username/package-name）

```bash
# 公开发布
npm publish --access public

# 私有发布（需要付费账户）
npm publish --access restricted
```

### 7. 验证发布

发布成功后，访问：
```
https://www.npmjs.com/package/playkit-sdk
```

测试安装：
```bash
npm install playkit-sdk@beta
# 或
npm install playkit-sdk
```

## 🔄 更新已发布的包

1. 修改代码
2. 更新版本号：
   ```bash
   npm version patch  # 1.0.0 -> 1.0.1
   npm version minor  # 1.0.0 -> 1.1.0
   npm version major  # 1.0.0 -> 2.0.0
   ```
3. 构建：`npm run build`
4. 发布：`npm publish`

## ⚠️ 注意事项

### package.json 配置检查

当前配置已经很完整：

```json
{
  "name": "playkit-sdk",                    // ✅ 包名
  "version": "1.0.0-beta.1",                // ✅ 版本号
  "description": "PlayKit SDK for JavaScript", // ✅ 描述
  "main": "dist/index.cjs.js",              // ✅ CommonJS 入口
  "module": "dist/index.esm.js",            // ✅ ES Module 入口
  "browser": "dist/index.umd.js",           // ✅ 浏览器入口
  "types": "dist/index.d.ts",               // ✅ TypeScript 类型
  "files": ["dist", "README.md", "LICENSE"], // ✅ 发布文件
  "keywords": [...],                        // ✅ 搜索关键词
  "author": "capsuleer",                    // ✅ 作者
  "license": "MIT",                         // ✅ 许可证
  "repository": {...},                      // ✅ 代码仓库
}
```

### 发布前自动构建

package.json 中已经配置了 `prepublishOnly` 钩子：
```json
"prepublishOnly": "npm run build"
```

这会在发布前自动运行构建，确保发布的是最新代码。

### LICENSE 文件

确保项目根目录有 LICENSE 文件。如果没有，创建一个 MIT LICENSE：

```txt
MIT License

Copyright (c) 2025 capsuleer

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction...
```

### .npmignore 文件

可以创建 `.npmignore` 文件排除不需要发布的文件：

```
src/
examples/
node_modules/
.git/
.vscode/
*.log
.env
tsconfig.json
rollup.config.js
```

但由于已经使用了 `files` 字段，这个是可选的。

## 🚀 快速发布命令

```bash
# 1. 登录 npm
npm login

# 2. 检查包名
npm view playkit-sdk

# 3. 构建项目
npm run build

# 4. 发布 beta 版本
npm publish --tag beta

# 5. 验证
npm view playkit-sdk
```

## 📊 发布后管理

### 查看包信息
```bash
npm view playkit-sdk
```

### 废弃某个版本
```bash
npm deprecate playkit-sdk@1.0.0-beta.1 "Please upgrade to 1.0.0"
```

### 删除已发布的版本（慎用）
```bash
# 只能在发布后 72 小时内删除
npm unpublish playkit-sdk@1.0.0-beta.1
```

### 查看下载统计
访问：https://npm-stat.com/charts.html?package=playkit-sdk

## 🎯 建议的发布策略

1. **首次发布**: 使用 beta 标签
   ```bash
   npm publish --tag beta
   ```

2. **稳定后**: 发布正式版本
   ```bash
   npm version 1.0.0
   npm publish
   ```

3. **持续迭代**: 使用语义化版本
   - Bug 修复: `npm version patch`
   - 新功能: `npm version minor`
   - 破坏性更改: `npm version major`

## 🔗 相关链接

- npm 官方文档: https://docs.npmjs.com/
- 语义化版本: https://semver.org/
- npm 包页面: https://www.npmjs.com/package/playkit-sdk (发布后可访问)

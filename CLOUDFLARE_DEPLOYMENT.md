# Cloudflare Pages 部署指南

## 📋 前置准备

1. 确保你有 Cloudflare 账号
2. 安装依赖：
```bash
npm install
```

## 🚀 部署步骤

### 1. 安装 Wrangler CLI（可选）

```bash
npm install -g wrangler
```

### 2. 登录 Cloudflare

```bash
wrangler login
```

### 3. 构建项目

```bash
npm run pages:build
```

### 4. 部署到 Cloudflare Pages

#### 方式 A：通过 Wrangler CLI 部署

```bash
npm run pages:deploy
```

#### 方式 B：通过 Cloudflare Dashboard 部署（推荐）

1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com/)
2. 进入 **Workers & Pages**
3. 点击 **Create application** > **Pages** > **Connect to Git**
4. 选择你的 GitHub 仓库
5. 配置构建设置：
   - **Framework preset**: Next.js
   - **Build command**: `npx @cloudflare/next-on-pages`
   - **Build output directory**: `.vercel/output/static`
   - **Root directory**: `/`
   - **Node version**: `20.11.0`

### 5. 配置环境变量

在 Cloudflare Pages 项目设置中添加以下环境变量：

```
GITHUB_TOKEN=your_github_token_here
GITHUB_OWNER=your_github_username
GITHUB_REPO=shiftatmidnight.com
ACCESS_PASSWORD=your_admin_password
NEXT_PUBLIC_GOOGLE_ANALYTICS_ID=your_ga_id
DOMAIN=https://shiftatmidnight.com
NODE_ENV=production
```

**重要提示：**
- 生产环境变量在 **Settings** > **Environment variables** > **Production**
- 预览环境变量在 **Preview** 标签下

### 6. 自定义域名（可选）

1. 在 Cloudflare Pages 项目中，进入 **Custom domains**
2. 点击 **Set up a custom domain**
3. 输入你的域名（如 `shiftatmidnight.com`）
4. 按照提示配置 DNS 记录

## 🔧 本地开发

### 使用 Cloudflare Pages 环境

1. 创建 `.dev.vars` 文件（参考 `.dev.vars.example`）
2. 运行开发服务器：

```bash
npm run pages:watch
```

### 使用标准 Next.js 开发环境

```bash
npm run dev
```

## ⚠️ 重要变更说明

### API 路由改动

所有 API 路由已改为 Edge Runtime，移除了 Node.js `fs` 模块的使用：

- ✅ 所有数据现在从 GitHub 读取
- ✅ 移除了本地文件系统依赖
- ✅ 添加了 `export const runtime = 'edge'` 声明

### 前端调用改动

如果你的前端代码中有类似这样的调用：

```javascript
// 旧代码（需要修改）
fetch('/api/getCategory?source=local&locale=en')

// 新代码（移除 source 参数）
fetch('/api/getCategory?locale=en')
```

现在所有 API 都直接从 GitHub 获取数据。

## 📊 性能优化建议

1. **启用 Cloudflare 缓存**：在 `next.config.mjs` 中配置缓存策略
2. **使用 Cloudflare Images**：优化图片加载
3. **启用 Auto Minify**：在 Cloudflare Dashboard 中启用 CSS/JS/HTML 压缩

## 🐛 常见问题

### 构建失败

如果遇到构建错误，检查：
- Node 版本是否为 20.x
- 所有依赖是否正确安装
- 环境变量是否正确配置

### API 调用失败

检查：
- GitHub Token 是否有正确的权限
- 环境变量是否在 Cloudflare Dashboard 中正确设置
- GitHub 仓库路径是否正确

### 下载链接问题

下载链接保持不变，继续使用 GitHub Releases：
```
https://github.com/codeczl/shiftatmidnight/releases/download/v1.00/ShiftAtMidnight.zip
```

## 📚 相关文档

- [Cloudflare Pages 文档](https://developers.cloudflare.com/pages/)
- [@cloudflare/next-on-pages 文档](https://github.com/cloudflare/next-on-pages)
- [Next.js Edge Runtime 文档](https://nextjs.org/docs/app/building-your-application/rendering/edge-and-nodejs-runtimes)

## 🔄 从 Vercel 迁移

如果你之前使用 Vercel：

1. 保留 Vercel 部署作为备份
2. 先在 Cloudflare 上测试完整功能
3. 确认无误后更新 DNS 指向 Cloudflare
4. 下载资源继续使用 GitHub Releases，无需改动

## 💡 提示

- 首次部署可能需要 5-10 分钟
- 后续部署通常在 2-3 分钟内完成
- Cloudflare Pages 提供无限带宽和请求
- 支持自动 HTTPS 和全球 CDN

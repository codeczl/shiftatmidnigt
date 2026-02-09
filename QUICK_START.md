# 🚀 Cloudflare Pages 快速开始

## 一键验证配置

```bash
npm run verify:migration
```

这个命令会检查所有必要的配置是否正确。

## 快速部署（3 步）

### 1️⃣ 安装依赖

```bash
npm install
```

### 2️⃣ 测试构建

```bash
npm run pages:build
```

如果构建成功，你会看到 `.vercel/output/static` 目录。

### 3️⃣ 部署

#### 选项 A：通过 Cloudflare Dashboard（推荐）

1. 提交代码到 GitHub
2. 访问 https://dash.cloudflare.com/
3. **Workers & Pages** > **Create** > **Connect to Git**
4. 选择仓库，配置：
   - Build command: `npx @cloudflare/next-on-pages`
   - Build output: `.vercel/output/static`
   - Node version: `20.11.0`
5. 添加环境变量（见下方）
6. 点击 **Deploy**

#### 选项 B：通过 CLI

```bash
npm run pages:deploy
```

## 🔐 环境变量

在 Cloudflare Dashboard 的 **Settings** > **Environment variables** 中添加：

```
GITHUB_TOKEN=ghp_xxxxxxxxxxxx
GITHUB_OWNER=codeczl
GITHUB_REPO=shiftatmidnight.com
ACCESS_PASSWORD=your_password
NEXT_PUBLIC_GOOGLE_ANALYTICS_ID=G-XXXXXXXXXX
DOMAIN=https://shiftatmidnight.com
NODE_ENV=production
```

## 🧪 本地测试

1. 创建 `.dev.vars` 文件：
```bash
cp .dev.vars.example .dev.vars
```

2. 编辑 `.dev.vars` 填入真实值

3. 运行开发服务器：
```bash
npm run dev
```

## ✅ 验证部署

部署后访问你的网站，测试：

- [ ] 首页加载
- [ ] 语言切换
- [ ] 文章列表
- [ ] 下载链接
- [ ] 管理后台

## 📚 详细文档

- [CLOUDFLARE_DEPLOYMENT.md](./CLOUDFLARE_DEPLOYMENT.md) - 完整部署指南
- [MIGRATION_CHECKLIST.md](./MIGRATION_CHECKLIST.md) - 迁移检查清单

## 🆘 遇到问题？

1. 运行验证脚本：`npm run verify:migration`
2. 查看 [CLOUDFLARE_DEPLOYMENT.md](./CLOUDFLARE_DEPLOYMENT.md) 的故障排查部分
3. 检查 Cloudflare Pages 的构建日志

## 💡 提示

- 首次部署需要 5-10 分钟
- 后续部署 2-3 分钟
- 下载链接继续使用 GitHub Releases，无需改动
- 所有数据从 GitHub 读取，无需本地文件

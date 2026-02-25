---
title: 部署 CMS 到 GitHub Pages
toc: true
---

本指南说明如何将 Blogo CMS（Next.js）静态导出并托管在 GitHub Pages 上，使其可通过公网访问。

> **注意**：CMS 本身不含敏感数据，所有操作通过浏览器 + GitHub API 完成，静态托管完全安全。  
> 遇到问题？请查看 [常见问题](/faq/)。

---

## 前置条件

- 已有 GitHub 账号
- 本地已安装 Node.js 18+ 和 Git

---

## 第一步：在 GitHub 创建仓库

1. 登录 [github.com](https://github.com)，点击右上角 **+** → **New repository**
2. 填写仓库信息：
   - **Repository name**：若用根路径访问填 `<username>.github.io`，若用子路径填 `blog-cms` 等
   - **Visibility**：**Public**
   - **不要**勾选 README、.gitignore、license
3. 点击 **Create repository**，记下仓库地址

---

## 第二步：将本地代码推送到 GitHub

```bash
cd blog-cms

npm install
git init
git add .
git commit -m "feat: initial commit"
git remote add origin https://github.com/<username>/<repo>.git
git branch -M main
git push -u origin main
```

> 推送时若要求密码，请使用 **Personal Access Token**（PAT）而非账号密码。  
> `package-lock.json` **必须提交**，否则 Actions 的 `npm ci` 会报错。

---

## 第三步：修改 next.config.ts

根据仓库名选择配置。

### 情况 A：根路径部署（仓库名为 `<username>.github.io`）

```ts
import path from 'path'
import type { NextConfig } from 'next'

const nextConfig: NextConfig = {
  output: 'export',
  trailingSlash: true,
  images: {
    unoptimized: true,
    remotePatterns: [
      { protocol: 'https', hostname: 'avatars.githubusercontent.com' },
      { protocol: 'https', hostname: '**.githubusercontent.com' },
    ],
  },
  outputFileTracingRoot: path.join(__dirname),
}

export default nextConfig
```

### 情况 B：子路径部署（如 `blog-cms`）

```ts
import path from 'path'
import type { NextConfig } from 'next'

const REPO_NAME = 'blog-cms'   // 改为你的仓库名

const nextConfig: NextConfig = {
  output: 'export',
  trailingSlash: true,
  basePath: `/${REPO_NAME}`,
  assetPrefix: `/${REPO_NAME}/`,
  images: {
    unoptimized: true,
    remotePatterns: [
      { protocol: 'https', hostname: 'avatars.githubusercontent.com' },
      { protocol: 'https', hostname: '**.githubusercontent.com' },
    ],
  },
  outputFileTracingRoot: path.join(__dirname),
}

export default nextConfig
```

---

## 第四步：添加 .nojekyll 文件

```bash
touch public/.nojekyll
```

---

## 第五步：创建 GitHub Actions 工作流

创建 `blog-cms/.github/workflows/deploy-cms.yml`：

```yaml
name: Deploy CMS to GitHub Pages

on:
  push:
    branches:
      - main

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run build
      - uses: actions/upload-pages-artifact@v3
        with:
          path: ./out

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - uses: actions/deploy-pages@v4
        id: deployment
```

---

## 第六步：在 GitHub 启用 Pages

> ⚠️ **必须先完成此步骤，再推送触发 Actions。** 否则会报错「创建部署失败（状态：404）」。

1. 打开 `https://github.com/<username>/<repo>/settings/pages`
2. **Source** 选择 **GitHub Actions**
3. 点击 **Save**

> 仓库必须为 **Public** 才能激活 GitHub Pages。

---

## 第七步：提交并推送

```bash
cd blog-cms
git add .
git commit -m "feat: add static export config and GitHub Actions for Pages"
git push
```

---

## 第八步：验证

等待 Actions 完成（约 1~3 分钟），访问：

- 根路径：`https://<username>.github.io/`
- 子路径：`https://<username>.github.io/blog-cms/`

---

## 后续更新

每次修改后推送 `main` 分支即可自动重新部署：

```bash
git add .
git commit -m "your commit message"
git push
```

---

遇到问题请查看 [常见问题](/faq/)。

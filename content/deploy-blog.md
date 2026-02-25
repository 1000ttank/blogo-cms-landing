---
title: 部署 Hexo 博客
toc: true
---

本文档面向第一次使用 Blogo CMS 的用户，带你从零完成 Hexo 博客的 GitHub 配置与部署。

> 遇到问题？请查看 [常见问题](/faq/)。

---

## 前置要求

- 已有 [GitHub](https://github.com) 账号
- 本地已安装 Node.js 20+（运行 `node -v` 确认版本）
- 本地已安装 Git（运行 `git --version` 确认安装）

---

## 整体流程概览

```
第一步  创建 GitHub Personal Access Token
第二步  在 GitHub 创建 blog 仓库
第三步  修改 blog/_config.yml 配置
第四步  在仓库设置 GH_TOKEN Secret
第五步  将 blog/ 推送到 GitHub（Actions 自动创建 gh-pages 分支）
第六步  开启 GitHub Pages（选择 gh-pages 分支）
第七步  启动 CMS，填写登录信息
```

---

## 第一步：创建 GitHub Personal Access Token

Token 是 CMS 与 GitHub 通信的凭证，用于读写博客文章。

1. 登录 GitHub，点击右上角头像 → **Settings**
2. 左侧菜单滚动到最底部，点击 **Developer settings**
3. 点击 **Personal access tokens** → **Tokens (classic)**
4. 点击右上角 **Generate new token** → **Generate new token (classic)**
5. 填写表单：
   - **Note（备注）**：填写 `blogo-cms`（用于区分用途，随意填写）
   - **Expiration（有效期）**：建议选 `No expiration` 或 `1 year`
   - **Select scopes（权限）**：勾选 `repo`（展开后会自动勾选所有 repo 子项）
6. 点击底部 **Generate token**
7. **立即复制生成的 Token**（页面刷新或关闭后将无法再次查看），格式为 `ghp_xxx...`，请保存到安全处。

---

## 第二步：在 GitHub 创建 blog 仓库

1. 点击 GitHub 右上角 **+** → **New repository**
2. 填写仓库信息：
   - **Repository name**：填写 `blog`（或你喜欢的名称）
   - **Visibility**：选 **Public**
   - **Initialize this repository with**：**三项全部不勾选**
3. 点击 **Create repository**，记录仓库地址：`https://github.com/你的用户名/blog`

---

## 第三步：修改 blog/_config.yml 配置

打开本地 `blog/_config.yml`，修改以下三处：

```yaml
# 第 1 处：网站信息
title: 我的博客
author: 你的名字

# 第 2 处：网站地址
url: https://你的用户名.github.io/blog
root: /blog/

# 第 3 处：部署配置
deploy:
  type: git
  repo: https://github.com/你的用户名/blog.git
  branch: gh-pages
```

> **特殊情况**：若仓库名为 `用户名.github.io`（个人主页），则 `url` 填 `https://用户名.github.io`，`root` 填 `/`。

---

## 第四步：在仓库设置 GH_TOKEN Secret

1. 打开 `blog` 仓库 → **Settings** → **Secrets and variables** → **Actions**
2. 点击 **New repository secret**
3. **Name**：`GH_TOKEN`（必须完全一致）
4. **Secret**：粘贴第一步的 Token
5. 点击 **Add secret**

---

## 第五步：将 blog/ 推送到 GitHub

```bash
cd /path/to/Hexo-NX-CMS/blog

npm install
git init
git add .
git commit -m "feat: initial hexo blog setup"
git branch -M main
git remote add origin https://github.com/你的用户名/blog.git
git push -u origin main
```

推送成功后，打开仓库 **Actions** 标签页，等待构建完成（约 1–2 分钟）。`gh-pages` 分支会自动创建。

---

## 第六步：开启 GitHub Pages

> ⚠️ 请务必在第五步 Actions 运行成功后再操作。

1. 仓库 **Settings** → **Pages**
2. **Source** 选择 **Deploy from a branch**
3. **Branch** 选择 `gh-pages`，路径选 `/ (root)`
4. 点击 **Save**

博客地址：`https://你的用户名.github.io/blog/`

---

## 第七步：启动 CMS 并登录

```bash
cd /path/to/Hexo-NX-CMS/blog-cms
npm install
npm run dev
```

访问 `http://localhost:3000`，填写 Token、用户名、仓库名登录。

---

## 验证流程

1. CMS 登录成功 → 仪表盘显示文章统计
2. 新建文章 → 保存后 `source/_posts/` 出现新文件
3. Actions 自动构建 → 约 1–2 分钟后完成
4. 访问博客 → 新文章已发布

---

## 附：更换 Hexo 主题

默认主题 `landscape` 较简单，推荐更换为 Fluid、NexT、Butterfly 等。

### 方式一：npm 安装（推荐）

以 **Fluid** 为例：

```bash
cd blog
npm install --save hexo-theme-fluid
cp node_modules/hexo-theme-fluid/_config.yml _config.fluid.yml
```

修改 `_config.yml`：`theme: fluid`，然后 `git add . && git commit -m "feat: add fluid theme" && git push`

### 方式二：git clone

```bash
cd blog
git clone https://github.com/next-theme/hexo-theme-next themes/next
rm -rf themes/next/.git   # 避免 submodule
```

修改 `_config.yml`：`theme: next`，提交并推送。

> 也可在 CMS 的 **配置管理** 页面在线编辑主题配置。

---

遇到问题请查看 [常见问题](/faq/)。

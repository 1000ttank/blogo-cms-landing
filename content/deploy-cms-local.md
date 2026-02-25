
---
title: 本地部署 CMS
toc: true
---

本指南说明如何在本地运行 Blogo CMS，通过 localhost 访问并管理您的 Hexo 博客。

> 遇到问题？请查看 [常见问题](/faq/)。

---

## 前置条件

- 已安装 [Node.js](https://nodejs.org/zh-cn) 20+
- 已安装 [Git](https://git-scm.com/)
- 已有 [GitHub](https://github.com/) 账号及 [Personal Access Token](/PAT/)（`repo` 权限）
- 一个 [Hexo 博客](/deploy-blog/)

---

## 第一步：获取项目代码

```bash
git clone https://github.com/1000ttank/blog-cms.git
cd blog-cms
```

---

## 第二步：安装依赖

```bash
npm install
```

---

## 第三步：启动开发服务器

```bash
npm run dev
```

默认访问地址：`http://localhost:3000`

---

## 第四步：登录 CMS

1. 在浏览器打开 `http://localhost:3000`
2. 填写登录信息：
   - **Token**：您的 GitHub Personal Access Token（需具备 `repo` 权限）
   - **用户名**：GitHub 用户名
   - **仓库名**：Hexo 博客所在仓库名（如 `blog`）

3. 点击登录，即可开始管理文章

---

## 使用说明

- 本地部署仅用于开发或本地管理，数据通过 GitHub API 读写，不存储在本地；
- 若尚未部署 Hexo 博客，请先参考 [部署 Hexo 博客](/deploy-blog/)

---

> 遇到问题请查看 [常见问题](/faq/)。

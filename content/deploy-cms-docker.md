---
title: Docker 部署 CMS
toc: true
---

本指南说明如何使用 Docker 在本地或服务器上运行 Blogo CMS，便于扩展与迁移。

> 遇到问题？请查看 [常见问题](/faq/)。

---

## 前置条件

- 已安装 [Docker](https://docs.docker.com/get-docker/)
- 已有 [GitHub Personal Access Token](https://github.com/settings/tokens)（`repo` 权限）

---

## 快速部署

项目已包含 `Dockerfile`，在 `blog-cms` 目录下执行：

```bash
git clone https://github.com/1000ttank/blog-cms.git
cd blog-cms
docker build -t blogo-cms . && docker run -p 3000:80 --rm blogo-cms
```

或使用 Docker Compose：

```bash
git clone https://github.com/1000ttank/blog-cms.git
cd blog-cms
docker compose up -d
```

访问 `http://localhost:3000` 即可使用 CMS。

---

## 手动构建

### 第一步：构建镜像

```bash
cd blog-cms
docker build -t blogo-cms .
```

### 第二步：运行容器

```bash
docker run -p 3000:80 --rm blogo-cms
```

或使用 `docker-compose.yml`（项目已包含）：

```yaml
services:
  cms:
    build: .
    ports:
      - "3000:80"
    restart: unless-stopped
```

```bash
docker compose up -d
```

---

## 使用说明

- Docker 部署适合在自有服务器或 VPS 上运行，便于与 Nginx、HTTPS 等配合
- 数据仍通过 GitHub API 读写，容器内不持久化敏感数据
- 更新镜像：修改代码后重新 `docker compose build` 并 `docker compose up -d`

---
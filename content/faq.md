---
title: 常见问题
toc: true
---

## Hexo 博客部署相关

### Q：GitHub Actions 报错 `remote: Permission to ... denied` 或 `Process completed with exit code 2`

这两种错误都是 `hexo deploy` 推送 `gh-pages` 分支时认证失败。依次检查：

1. Secret 名称是否完全为 `GH_TOKEN`（全大写，无多余空格）
2. Token 是否具有 `repo` 权限
3. Token 是否已过期（前往 GitHub → Settings → Developer settings → Tokens 确认）

---

### Q：博客地址打开是 404

确认 GitHub Pages 已设置为 `gh-pages` 分支。`gh-pages` 分支在 Actions 首次运行成功后才会创建，之后在 Settings → Pages 中设置即可。

---

### Q：CMS 登录提示「验证失败」

检查 Token 是否已过期，或仓库名称、用户名是否填写有误（区分大小写）。

---

### Q：保存文章后博客没有立即更新

每次通过 CMS 保存文章会自动提交并触发 Actions 重新部署，等待约 1–2 分钟后刷新博客即可。可在 CMS 的 **构建历史** 页面查看状态。

---

### Q：打开博客页面后没有显示内容

可能是缺少主题配置，详见 [部署 Hexo 博客](/deploy-blog/#附更换-hexo-主题) 中的「更换 Hexo 主题」部分。

---

## CMS 部署相关

### Q：页面空白或 JS/CSS 资源 404

- 检查 `next.config.ts` 中 `basePath` 是否与仓库名完全一致
- 确认 `public/.nojekyll` 文件已创建并提交
- 确认 Actions 工作流中 `path: ./out` 正确

---

### Q：Actions 失败，提示 `npm ci` 错误

确保 `package-lock.json` 已提交到仓库（`.gitignore` 中不能有此文件名）。

---

### Q：Actions deploy 步骤报错「创建部署失败（状态：404）」

原因是 GitHub Pages 尚未启用。解决方法：

1. 打开 `https://github.com/<username>/<repo>/settings/pages`
2. **Source** 选择 **GitHub Actions** 并保存
3. 重新触发 Actions：`git commit --allow-empty -m "chore: trigger pages deployment" && git push`

---

### Q：git push 被拒绝，提示 non-fast-forward

远程仓库有本地没有的提交，先同步再推送：

```bash
git pull --rebase origin main
git push
```

若提示输入密码，请使用 **GitHub Token**（非账号密码）。

---

### Q：登录后刷新页面 Token 丢失

正常行为，Token 存储在浏览器 `localStorage`，清除缓存后需重新登录。

---

### Q：如何使用自定义域名？

在仓库 **Settings → Pages → Custom domain** 填写域名，同时在 `public/` 下创建 `CNAME` 文件，内容为域名。此时 `next.config.ts` 中 `basePath` 应置为空（按根路径配置）。

---

## 通用

### Q：`git push` 报错 `rejected ... non-fast-forward`

远程分支存在本地没有的新提交，需要先拉取再推送：

```bash
git pull --rebase origin main
git push
```
